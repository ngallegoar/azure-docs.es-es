---
title: Uso de identidades administradas para el control de acceso (versión preliminar)
titleSuffix: Azure Machine Learning
description: Aprenda a usar identidades administradas para controlar el acceso a los recursos de Azure desde el área de trabajo de Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: c4ea7609c343532f17144e388be7583eab427eee
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92440457"
---
# <a name="use-managed-identities-with-azure-machine-learning-preview"></a>Utilice identidades administradas con Azure Machine Learning (versión preliminar)

Las [identidades administradas](/azure/active-directory/managed-identities-azure-resources/overview) permiten configurar el área de trabajo con los *permisos mínimos necesarios para acceder a los recursos* . 

Al configurar el área de trabajo de Azure Machine Learning de forma confiable, es importante asegurarse de que los distintos servicios asociados al área de trabajo tienen el nivel de acceso correcto. Por ejemplo, durante el flujo de trabajo de Machine Learning, el área de trabajo necesita acceso a Azure Container Registry (ACR) para las imágenes de Docker y a las cuentas de almacenamiento para los datos de entrenamiento. 

Además, las identidades administradas permiten un control exhaustivo de los permisos; por ejemplo, puede conceder o revocar el acceso de recursos de proceso específicos a una instancia específica de ACR.

En este artículo, aprenderá a usar las identidades administradas para:

 * Configurar y usar ACR para el área de trabajo de Azure Machine Learning sin tener que habilitar el acceso de usuario administrador a ACR.
 * Acceder a una instancia privada de ACR externa al área de trabajo para extraer las imágenes base para el entrenamiento o la inferencia.

> [!IMPORTANT]
> El uso de identidades administradas para controlar el acceso a los recursos con Azure Machine Learning está actualmente en versión preliminar. La funcionalidad de versión preliminar se proporciona "tal cual", sin ninguna garantía de soporte técnico ni contrato de nivel de servicio. Para obtener más información, consulte [Condiciones de uso complementarias de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo de Azure Machine Learning. Para más información, consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
- La [extensión de la CLI de Azure para el servicio Machine Learning](reference-azure-machine-learning-cli.md).
- El [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
- Para asignar roles, el inicio de sesión de la suscripción de Azure debe tener el rol de [operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) u otro rol que conceda las acciones necesarias (como el de __propietario__ ).
- Debe estar familiarizado con la creación y el uso de [identidades administradas](/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="configure-managed-identities"></a>Configuración de identidades administradas

En algunas situaciones, es necesario impedir el acceso de usuario administrador a Azure Container Registry. Por ejemplo, es posible que la instancia de ACR sea compartida y deba impedir el acceso de administrador a otros usuarios. O bien, que una directiva de nivel de suscripción impida la creación de ACR con el usuario administrador habilitado.

> [!IMPORTANT]
> Al usar Azure Machine Learning para la inferencia en Azure Container Instance (ACI), el acceso de usuario administrador en ACR es __obligatorio__ . No lo deshabilite si planea implementar modelos en ACI con fines de inferencia.

Al crear una instancia de ACR sin habilitar el acceso de usuario administrador, las identidades administradas se usan para acceder a ACR con la finalidad de compilar y extraer imágenes de Docker.

Puede traer su propia instancia de ACR con el usuario administrador deshabilitado al crear el área de trabajo. Como alternativa, deje que Azure Machine Learning cree la instancia de ACR del área de trabajo y, después, deshabilite el usuario administrador.

### <a name="bring-your-own-acr"></a>Traiga su propia instancia de ACR

Si la directiva de suscripción no permite el usuario administrador de ACR, debe crear primero la instancia de ACR sin el usuario administrador y, a continuación, asociarla al área de trabajo. Además, si tiene una instancia de ACR con el usuario administrador deshabilitado, puede conectarla al área de trabajo.

[Cree una instancia de ACR desde la CLI de Azure](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli) sin establecer el argumento ```--admin-enabled```, o desde Azure Portal sin habilitar el usuario administrador. A continuación, al crear el área de trabajo de Azure Machine Learning, especifique el identificador de recurso de Azure de la instancia de ACR. En el ejemplo siguiente se muestra cómo crear una nueva área de trabajo de Azure Machine Learning que use una instancia de ACR existente:

> [!TIP]
> Para obtener el valor del parámetro `--container-registry`, use el comando [az acr show](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az_acr_show) para mostrar la información de la instancia de ACR. El campo `id` contiene el identificador de recurso de su instancia de ACR.

```azurecli-interactive
az ml workspace create -w <workspace name> \
-g <workspace resource group> \
-l <region> \
--container-registry /subscriptions/<subscription id>/resourceGroups/<acr resource group>/providers/Microsoft.ContainerRegistry/registries/<acr name>
```

### <a name="let-azure-machine-learning-service-create-workspace-acr"></a>Permitir que el servicio Azure Machine Learning cree la instancia de ACR del área de trabajo

Si no trae su propia instancia de ACR, el servicio Azure Machine Learning creará una automáticamente al realizar una operación que requiera una. Por ejemplo, el envío de una ejecución de entrenamiento a Proceso de Machine Learning, la creación de un entorno o la implementación de un punto de conexión de servicio Web. La instancia de ACR creada por el área de trabajo tendrá habilitado el usuario administrador. Deberá deshabilitarlo manualmente.

1. Crear un área de trabajo

    ```azurecli-interactive
    az ml workspace show -n <my workspace> -g <my resource group>
    ```

1. Realice una acción que requiera ACR. Por ejemplo, el [tutorial sobre el entrenamiento de un modelo](tutorial-train-models-with-aml.md).

1. Obtenga el nombre de la instancia de ACR creada por el clúster:

    ```azurecli-interactive
    az ml workspace show -w <my workspace> \
    -g <my resource group>
    --query containerRegistry
    ```

    Este comando devuelve un valor similar al siguiente texto: Solo desea la última parte del texto, que es el nombre de la instancia de ACR:

    ```text
    /subscriptions/<subscription id>/resourceGroups/<my resource group>/providers/MicrosoftContainerReggistry/registries/<ACR instance name>
    ```

1. Actualice la instancia de ACR para deshabilitar el usuario administrador:

    ```azurecli-interactive
    az acr update --name <ACR instance name> --admin-enabled false
    ```

### <a name="create-compute-with-managed-identity-to-access-docker-images-for-training"></a>Creación de un proceso con una identidad administrada para acceder a imágenes de Docker para el entrenamiento

Para acceder a la instancia de ACR del área de trabajo, cree un clúster de proceso de Machine Learning con una identidad administrada asignada por el sistema habilitada. Puede habilitar la identidad desde Azure Portal o Studio al crear el proceso, o bien desde la CLI de Azure mediante

# <a name="python"></a>[Python](#tab/python)

Al crear un clúster de proceso con la clase [AmlComputeProvisioningConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcomputeprovisioningconfiguration?view=azure-ml-py), use el parámetro `identity_type` para establecer el tipo de identidad administrada.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interaction
az ml computetarget create amlcompute --name cpucluster -w <workspace> -g <resource group> --vm-size <vm sku> --assign-identity '[system]'
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

Para obtener información sobre cómo configurar la identidad administrada al crear un clúster de proceso en Studio, consulte [Configuración de una identidad administrada](how-to-create-attach-compute-studio.md#managed-identity).

---

A una identidad administrada se le concede automáticamente el rol ACRPull en la instancia de ACR del área de trabajo para habilitar la extracción de imágenes de Docker para el entrenamiento.

> [!NOTE]
> Si crea el proceso antes de la creación de la instancia de ACR del área de trabajo, debe asignar el rol ACRPull manualmente.

## <a name="access-base-images-from-private-acr"></a>Acceso a imágenes base desde la instancia de ACR privada

De forma predeterminada, Azure Machine Learning usa imágenes base de Docker que proceden de un repositorio público administrado por Microsoft. A continuación, crea el entorno de entrenamiento o inferencia basado en esas imágenes. Para obtener más información, consulte [¿Qué son los entornos de Azure Machine Learning?](concept-environments.md)

Para usar una imagen base personalizada interna para su empresa, puede usar identidades administradas para acceder a su instancia de ACR privada. Hay dos posibles casos de uso:

 * Uso de la imagen base para el entrenamiento tal cual.
 * Creación de una imagen administrada de Azure Machine Learning mediante la imagen personalizada como base.

### <a name="pull-docker-base-image-to-machine-learning-compute-cluster-for-training-as-is"></a>Extracción de la imagen base de Docker en el clúster de proceso de Machine Learning para el entrenamiento tal cual

Cree el clúster de proceso de Machine Learning con la identidad administrada asignada por el sistema según se ha descrito anteriormente. A continuación, determine el id. de entidad de seguridad de la identidad administrada.

```azurecli-interactive
az ml computetarget amlcompute identity show --name <cluster name> -w <workspace> -g <resource group>
```

Opcionalmente, puede actualizar el clúster de proceso para asignar una identidad administrada asignada por el usuario:

```azurecli-interactive
az ml computetarget amlcompute identity assign --name cpucluster \
-w $mlws -g $mlrg --identities <my-identity-id>
```

Para permitir que el clúster de proceso extraiga las imágenes base, conceda el rol ACRPull de Managed Service Identity en la instancia de ACR privada.

```azurecli-interactive
az role assignment create --assignee <principal ID> \
--role acrpull \
--scope "/subscriptions/<subscription ID>/resourceGroups/<private ACR resource group>/providers/Microsoft.ContainerRegistry/registries/<private ACR name>"
```

Por último, al enviar una ejecución de entrenamiento, especifique la ubicación de la imagen base en la [definición del entorno](how-to-use-environments.md#use-existing-environments).

```python
from azureml.core import Environment
env = Environment(name="private-acr")
env.docker.base_image = "<ACR name>.azurecr.io/<base image repository>/<base image version>"
env.python.user_managed_dependencies = True
```

> [!IMPORTANT]
> Para asegurarse de que la imagen base se extrae directamente en el recurso de proceso, establezca `user_managed_dependencies = True` y no especifique un archivo Dockerfile. De lo contrario, el servicio Azure Machine Learning intentará crear una nueva imagen de Docker y se producirá un error, porque solo el clúster de proceso tiene acceso para extraer la imagen base desde ACR.

### <a name="build-azure-machine-learning-managed-environment-into-base-image-from-private-acr-for-training-or-inference"></a>Creación el entorno administrado de Azure Machine Learning en una imagen base desde la instancia de ACR privada para el entrenamiento o la inferencia

En este escenario, el servicio Azure Machine Learning crea el entorno de entrenamiento o inferencia sobre una imagen base que el usuario suministra desde una instancia de ACR privada. Dado que la tarea de creación de la imagen se lleva a cabo en la instancia de ACR del área de trabajo mediante ACR Tasks, debe realizar pasos adicionales para permitir el acceso.

1. Cree una __identidad administrada asignada por el usuario__ y concédale acceso ACRPull para la __instancia de ACR privada__ .  
1. Conceda a la __identidad administrada asignada por el sistema__ del área de trabajo el rol de operador de identidad administrada en la __identidad administrada asignada por el usuario__ del paso anterior. Este rol permite al área de trabajo asignar la identidad administrada asignada por el usuario a la tarea de ACR Tasks para crear el entorno administrado. 

    1. Obtenga el identificador de entidad de seguridad de la identidad administrada asignada por el sistema del área de trabajo:

        ```azurecli-interactive
        az ml workspace show -w <workspace name> -g <resource group> --query identityPrincipalId
        ```

    1. Conceda el rol de operador de identidad administrada:

        ```azurecli-interactive
        az role assignment create --assignee <principal ID> --role managedidentityoperator --scope <UAI resource ID>
        ```

        El identificador de recurso UAI es el identificador de recurso de Azure de la identidad asignada por el usuario, con el formato `/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<UAI name>`.

1. Especifique el id. de cliente y la instancia de ACR externa de la __identidad administrada asignada por el usuario__ en las conexiones del área de trabajo mediante el [método Workspace.set_connection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#set-connection-name--category--target--authtype--value-):

    ```python
    workspace.set_connection(
        name="privateAcr", 
        category="ACR", 
        target = "<acr url>", 
        authType = "RegistryConnection", 
        value={"ResourceId": "<UAI resource id>", "ClientId": "<UAI client ID>"})
    ```

Una vez completada la configuración, puede usar las imágenes base de la instancia de ACR privada al crear entornos para el entrenamiento o la inferencia. En el fragmento de código siguiente se muestra cómo especificar el nombre de imagen y la instancia de ACR de la imagen base en una definición de entorno:

```python
from azureml.core import Environment

env = Environment(name="my-env")
env.docker.base_image = "<acr url>/my-repo/my-image:latest"
```

Opcionalmente, puede especificar la dirección URL del recurso de identidad administrada y el id. de cliente en la propia definición de entorno mediante [RegistryIdentity](https://docs.microsoft.com/python/api/azureml-core/azureml.core.container_registry.registryidentity?view=azure-ml-py). Si usa la identidad del registro explícitamente, invalida las conexiones del área de trabajo especificadas anteriormente:

```python
from azureml.core.container_registry import RegistryIdentity

identity = RegistryIdentity()
identity.resource_id= "<UAI resource ID>"
identity.client_id="<UAI client ID>”
env.docker.base_image_registry.registry_identity=identity
env.docker.base_image = "my-acr.azurecr.io/my-repo/my-image:latest"
```

## <a name="use-docker-images-for-inference"></a>Uso de imágenes de Docker para la inferencia

Una vez que haya configurado ACR sin el usuario administrador tal y como se ha descrito anteriormente, puede acceder a las imágenes de Docker para la inferencia sin claves de administración de Azure Kubernetes Service (AKS). Al crear la instancia de AKS o conectarla al área de trabajo, se asigna automáticamente a la entidad de servicio del clúster acceso ACRPull a la instancia de ACR del área de trabajo.

> [!NOTE]
> Si trae su propio clúster de AKS, el clúster debe tener habilitada la entidad de servicio en lugar de la identidad administrada.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información en [Seguridad de empresa para Azure Machine Learning](concept-enterprise-security.md).
