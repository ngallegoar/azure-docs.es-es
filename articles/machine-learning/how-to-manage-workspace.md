---
title: Creación de áreas de trabajo en el portal
titleSuffix: Azure Machine Learning
description: Aprenda a crear, ver y eliminar áreas de trabajo de Azure Machine Learning en Azure Portal o con el SDK de Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sgilley
author: sdgilley
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, fasttrack-edit
ms.openlocfilehash: 9abfbe03a4192411a3790bb6d6e488d674c13109
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92897167"
---
# <a name="create-and-manage-azure-machine-learning-workspaces"></a>Creación y administración de áreas de trabajo de Azure Machine Learning 

En este artículo creará, verá y eliminará [**áreas de trabajo de Azure Machine Learning**](concept-workspace.md) para [Azure Machine Learning](overview-what-is-azure-ml.md) con Azure Portal o el [SDK de Python](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py&preserve-view=true).

A medida que cambian las necesidades o aumentan los requisitos de automatización, también puede crear y eliminar áreas de trabajo [mediante la CLI](reference-azure-machine-learning-cli.md) o [a través de la extensión de VS Code](tutorial-setup-vscode-extension.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* Si usa el SDK de Python, [instalar el SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).

## <a name="create-a-workspace"></a>Crear un área de trabajo

# <a name="python"></a>[Python](#tab/python)

* **Especificación predeterminada.** De manera predeterminada, los recursos dependientes y el grupo de recursos se crearán automáticamente. Este código crea un área de trabajo llamada `myworkspace` y un grupo de recursos denominado `myresourcegroup` en `eastus2`.
    
    ```python
    from azureml.core import Workspace
    
    ws = Workspace.create(name='myworkspace',
                   subscription_id='<azure-subscription-id>',
                   resource_group='myresourcegroup',
                   create_resource_group=True,
                   location='eastus2'
                   )
    ```
    Establezca `create_resource_group` en False si tiene un grupo de recursos de Azure existente que quiera usar para el área de trabajo.

* <a name="create-multi-tenant"></a>**Varios inquilinos.**  Si tiene varias cuentas, agregue el id. de inquilino de la cuenta de Azure Active Directory que desea usar.  Busque el id. de inquilino en [Azure Portal](https://portal.azure.com) en **Azure Active Directory, Identidades externas**.

    ```python
    from azureml.core.authentication import InteractiveLoginAuthentication
    from azureml.core import Workspace
    
    interactive_auth = InteractiveLoginAuthentication(tenant_id="my-tenant-id")
    ws = Workspace.create(name='myworkspace',
                subscription_id='<azure-subscription-id>',
                resource_group='myresourcegroup',
                create_resource_group=True,
                location='eastus2',
                auth=interactive_auth
                )
    ```

* **[Nube soberana](reference-machine-learning-cloud-parity.md)** . Necesitará código adicional para autenticarse en Azure si está trabajando en una nube soberana.

    ```python
    from azureml.core.authentication import InteractiveLoginAuthentication
    from azureml.core import Workspace
    
    interactive_auth = InteractiveLoginAuthentication(cloud="<cloud name>") # for example, cloud="AzureUSGovernment"
    ws = Workspace.create(name='myworkspace',
                subscription_id='<azure-subscription-id>',
                resource_group='myresourcegroup',
                create_resource_group=True,
                location='eastus2',
                auth=interactive_auth
                )
    ```

* **Uso de recursos de Azure existentes**.  También puede crear un área de trabajo que use los recursos de Azure existentes con el formato de identificador de recursos de Azure. Busque los identificadores de recursos de Azure específicos en Azure Portal o con el SDK. En este ejemplo se da por supuesto que el grupo de recursos, la cuenta de almacenamiento, el almacén de claves, App Insights y el registro de contenedor ya existen.

   ```python
   import os
   from azureml.core import Workspace
   from azureml.core.authentication import ServicePrincipalAuthentication

   service_principal_password = os.environ.get("AZUREML_PASSWORD")

   service_principal_auth = ServicePrincipalAuthentication(
      tenant_id="<tenant-id>",
      username="<application-id>",
      password=service_principal_password)

                        auth=service_principal_auth,
                             subscription_id='<azure-subscription-id>',
                             resource_group='myresourcegroup',
                             create_resource_group=False,
                             location='eastus2',
                             friendly_name='My workspace',
                             storage_account='subscriptions/<azure-subscription-id>/resourcegroups/myresourcegroup/providers/microsoft.storage/storageaccounts/mystorageaccount',
                             key_vault='subscriptions/<azure-subscription-id>/resourcegroups/myresourcegroup/providers/microsoft.keyvault/vaults/mykeyvault',
                             app_insights='subscriptions/<azure-subscription-id>/resourcegroups/myresourcegroup/providers/microsoft.insights/components/myappinsights',
                             container_registry='subscriptions/<azure-subscription-id>/resourcegroups/myresourcegroup/providers/microsoft.containerregistry/registries/mycontainerregistry',
                             exist_ok=False)
   ```

Para más información, vea [Referencia del SDK del área de trabajo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true).

Si tiene problemas para obtener acceso a su suscripción, consulte [Configuración de la autenticación para recursos y flujos de trabajo de Azure Machine Learning](how-to-setup-authentication.md), así como el cuaderno de [Autenticación en Azure Machine Learning](https://aka.ms/aml-notebook-auth).

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con las credenciales de la suscripción de Azure. 

1. En la esquina superior izquierda de Azure Portal, seleccione **+ Crear un recurso**.

      ![Crear un nuevo recurso](./media/how-to-manage-workspace/create-workspace.gif)

1. Use la barra de búsqueda para encontrar **Machine Learning**.

1. Seleccione **Machine Learning**.

1. En el panel **Machine Learning** , seleccione **Crear** para comenzar.

1. Especifique la siguiente información para configurar la nueva área de trabajo:

   Campo|Descripción 
   ---|---
   Nombre del área de trabajo |Escriba un nombre único que identifique el área de trabajo. En este ejemplo, se usa **docs-ws**. Los nombres deben ser únicos en el grupo de recursos. Utilice un nombre que sea fácil de recordar y que se diferencie del de las áreas de trabajo creadas por otros. El nombre del área de trabajo no distingue mayúsculas de minúsculas.
   Subscription |Seleccione la suscripción de Azure que quiera usar.
   Resource group | Use un grupo de recursos existente en su suscripción o escriba un nombre para crear un nuevo grupo de recursos. Un grupo de recursos almacena los recursos relacionados con una solución de Azure. En este ejemplo, se usa **docs-aml**. Necesita el rol *colaborador* o *propietario* para usar un grupo de recursos existente.  Para obtener más información sobre el acceso, consulte [Administración del acceso a un área de trabajo de Azure Machine Learning](how-to-assign-roles.md).
   Region | Seleccione la región de Azure más cercana a los usuarios y los recursos de datos para crear el área de trabajo.

    ![Configuración de un área de trabajo](./media/how-to-manage-workspace/create-workspace-form.png)

1. Cuando haya terminado de configurar el área de trabajo, seleccione **Revisar y crear**. También puede usar las secciones [Redes](#networking) y [Opciones avanzadas](#advanced) para configurar otros valores del área de trabajo.

1. Revise la configuración y realice cualquier cambio o corrección adicional. Cuando esté satisfecho con la configuración, seleccione **Crear**.

   > [!Warning] 
   > La creación del área de trabajo en la nube puede tardar varios minutos.

   Cuando finalice el proceso, aparecerá un mensaje de implementación correcta. 
 
 1. Para ver la nueva área de trabajo, seleccione **Ir al recurso**.
 
---

### <a name="networking"></a>Redes  

> [!IMPORTANT]  
> Para obtener más información sobre el uso de un punto de conexión privado y una red virtual con el área de trabajo, vea [Aislamiento de red y privacidad](how-to-network-security-overview.md).


# <a name="python"></a>[Python](#tab/python)

El SDK de Azure Machine Learning para Python proporciona la clase [PrivateEndpointConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py&preserve-view=true), que se puede usar con [Workspace.create()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---tags-none--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--adb-workspace-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-&preserve-view=true) para crear un área de trabajo con un punto de conexión privado. Esta clase requiere una red virtual existente.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. La configuración de red predeterminada es usar un __Punto de conexión público__ , que es accesible en la red pública de Internet. Para limitar el acceso al área de trabajo a una instancia de Azure Virtual Network que ha creado, puede seleccionar __Punto de conexión privado__ (versión preliminar) en __Método de conectividad__ y, después, usar __+ Agregar__ para configurar el punto de conexión.   

   :::image type="content" source="media/how-to-manage-workspace/select-private-endpoint.png" alt-text="Selección del punto de conexión privado":::  

1. En el formulario __Crear punto de conexión privado__ , establezca la ubicación, el nombre y la red virtual que se va a usar. Si desea usar el punto de conexión con una zona DNS privada, seleccione __Integrar con la zona DNS privada__ y seleccione la zona mediante el campo __Zona DNS privada__. Seleccione __Aceptar__ para crear el punto de conexión.   

   :::image type="content" source="media/how-to-manage-workspace/create-private-endpoint.png" alt-text="Creación del punto de conexión privado":::   

1. Cuando haya terminado de configurar las redes, puede seleccionar __Revisar y crear__ , o bien avanzar hasta la configuración de __Avanzado__ opcional.

---

> [!IMPORTANT]  
> El uso de un punto de conexión privado con áreas de trabajo de Azure Machine Learning se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.     
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="multiple-workspaces-with-private-endpoint"></a>Varias áreas de trabajo con punto de conexión privado

Al crear un punto de conexión privado, se crea una nueva zona DNS privada denominada __privatelink.api.azureml.ms__. Contiene un vínculo a la red virtual. Si crea varias áreas de trabajo con puntos de conexión privados en el mismo grupo de recursos, solo la red virtual del primer punto de conexión privado se puede agregar a la zona DNS. Para agregar entradas para las redes virtuales usadas por áreas de trabajo o puntos de conexión privados adicionales, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), seleccione el grupo de recursos que contiene el área de trabajo. A continuación, seleccione el recurso de zona DNS privada denominado __privatelink.api.azureml.ms__.
2. En __Configuración__ , seleccione __Vínculos de red virtual__.
3. Seleccione __Agregar__. En la página __Agregar el vínculo de red virtual__ , proporcione un __Nombre de vínculo__ único y, a continuación, seleccione la __Red virtual__ que se va a agregar. Seleccione __Aceptar__ para agregar el vínculo de red.

Para obtener más información, vea [Configuración de DNS para puntos de conexión privados de Azure](/azure/private-link/private-endpoint-dns).

### <a name="vulnerability-scanning"></a>Examen de vulnerabilidades

Azure Security Center proporciona administración unificada de la seguridad y protección avanzada contra amenazas para cargas de trabajo en la nube híbrida. Debe permitir que Azure Security Center examine los recursos y seguir sus recomendaciones. Para más información, consulte [Introducción a Azure Defender para registros de contenedor](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) e [Introducción a Azure Defender para Kubernetes](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration).

### <a name="advanced"></a>Avanzado

De forma predeterminada, las métricas y los metadatos del área de trabajo se almacenan en una instancia de Azure Cosmos DB que Microsoft mantiene. Estos datos se cifran con claves administradas por Microsoft.

Para limitar los datos que Microsoft recopila sobre el área de trabajo, seleccione __Área de trabajo de alto impacto de negocio__ en el portal o establezca `hbi_workspace=true ` en Python. Para más información sobre esta configuración, consulte [Cifrado en reposo](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]  
> La selección de un alto impacto de negocio solo puede realizarse al crear un área de trabajo. Este valor no se puede cambiar tras la creación del área de trabajo.   

#### <a name="use-your-own-key"></a>Usar su propia clave

El usuario puede proporcionar su propia clave para el cifrado de datos. Así, se crea la instancia de Azure Cosmos DB que almacena las métricas y los metadatos en la suscripción de Azure.

[!INCLUDE [machine-learning-customer-managed-keys.md](../../includes/machine-learning-customer-managed-keys.md)]

Siga estos pasos para proporcionar su propia clave:

> [!IMPORTANT]  
> Antes de seguir estos pasos, debe completar las siguientes acciones:   
>
> 1. Autorice __Machine Learning App__ (en Administración de identidades y acceso) con permisos de colaborador en la suscripción.  
> 1. Siga los pasos de [Configuración de claves administradas por el cliente](/azure/cosmos-db/how-to-setup-cmk) para:
>     * Registrar el proveedor de Azure Cosmos DB
>     * Creación y configuración de una instancia de Azure Key Vault
>     * Generar una clave
>   
>     No es necesario crear manualmente la instancia de Azure Cosmos DB; se crea una automáticamente durante la creación del área de trabajo. Esta instancia de Azure Cosmos DB se crea en un grupo de recursos independiente con un nombre basado en este patrón: `<your-workspace-resource-name>_<GUID>`.   
>   
> Este valor no se puede cambiar tras la creación del área de trabajo. Si elimina la instancia de Azure Cosmos DB que usa el área de trabajo, también debe eliminar el área de trabajo que la está usando.

# <a name="python"></a>[Python](#tab/python)

Use `cmk_keyvault` y `resource_cmk_uri` para especificar la clave administrada por el cliente.

```python
from azureml.core import Workspace
   ws = Workspace.create(name='myworkspace',
               subscription_id='<azure-subscription-id>',
               resource_group='myresourcegroup',
               create_resource_group=True,
               location='eastus2'
               cmk_keyvault='subscriptions/<azure-subscription-id>/resourcegroups/myresourcegroup/providers/microsoft.keyvault/vaults/<keyvault-name>', 
               resource_cmk_uri='<key-identifier>'
               )

```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Seleccione __Claves administradas por el cliente__ y, después, seleccione __Hacer clic para seleccionar una clave__.

    :::image type="content" source="media/how-to-manage-workspace/advanced-workspace.png" alt-text="Claves administradas por el cliente":::

1. En el formulario __Seleccionar clave en Azure Key Vault__ , seleccione una instancia de Azure Key Vault existente, una clave que contenga y la versión de la clave. Esta clave se usa para cifrar los datos almacenados en Azure Cosmos DB. Por último, use el botón __Seleccionar__ para usar esta clave.

   :::image type="content" source="media/how-to-manage-workspace/select-key-vault.png" alt-text="Seleccione la clave":::

---

### <a name="download-a-configuration-file"></a>Descarga de un archivo de configuración

Si va a crear una [instancia de proceso](tutorial-1st-experiment-sdk-setup.md#azure), omita este paso.  La instancia de proceso ya ha creado una copia de este archivo.

# <a name="python"></a>[Python](#tab/python)

Si tiene previsto usar código en el entorno local que haga referencia a esta área de trabajo (`ws`), escriba el archivo de configuración:

```python
ws.write_config()
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

Si tiene previsto usar código en el entorno local que haga referencia a esta área de trabajo, seleccione **Descargar config.json** en la sección **Información general** del área de trabajo.  

   ![Descargar config.json](./media/how-to-manage-workspace/configure.png)

---

Coloque el archivo en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *.azureml* o en un directorio principal. Al crear una instancia de proceso, este archivo se agrega automáticamente al directorio correcto de la máquina virtual.

## <a name="connect-to-a-workspace"></a>Conexión a un área de trabajo

En el código de Python, cree un objeto de área de trabajo para conectarse al área de trabajo.  Este código leerá el contenido del archivo de configuración para encontrar el área de trabajo.  Si aún no está autenticado, recibirá un mensaje para iniciar sesión.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
```

* <a name="connect-multi-tenant"></a>**Varios inquilinos.**  Si tiene varias cuentas, agregue el id. de inquilino de la cuenta de Azure Active Directory que desea usar.  Busque el id. de inquilino en [Azure Portal](https://portal.azure.com) en **Azure Active Directory, Identidades externas**.

    ```python
    from azureml.core.authentication import InteractiveLoginAuthentication
    from azureml.core import Workspace
    
    interactive_auth = InteractiveLoginAuthentication(tenant_id="my-tenant-id")
    ws = Workspace.from_config(auth=interactive_auth)
    ```

* **[Nube soberana](reference-machine-learning-cloud-parity.md)** . Necesitará código adicional para autenticarse en Azure si está trabajando en una nube soberana.

    ```python
    from azureml.core.authentication import InteractiveLoginAuthentication
    from azureml.core import Workspace
    
    interactive_auth = InteractiveLoginAuthentication(cloud="<cloud name>") # for example, cloud="AzureUSGovernment"
    ws = Workspace.from_config(auth=interactive_auth)
    ```
    
Si tiene problemas para obtener acceso a su suscripción, consulte [Configuración de la autenticación para recursos y flujos de trabajo de Azure Machine Learning](how-to-setup-authentication.md), así como el cuaderno de [Autenticación en Azure Machine Learning](https://aka.ms/aml-notebook-auth).

## <a name="find-a-workspace"></a><a name="view"></a>Buscar un área de trabajo

Vea una lista de todas las áreas de trabajo que puede usar.

# <a name="python"></a>[Python](#tab/python)

Busque las suscripciones en la [página Suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).  Copie el identificador y úselo en el código siguiente para ver todas las áreas de trabajo disponibles para esa suscripción.

```python
from azureml.core import Workspace

Workspace.list('<subscription-id>')
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. En el campo de búsqueda superior, escriba **Machine Learning**.  

1. Seleccione **Machine Learning**.

   ![Búsqueda del área de trabajo de Azure Machine Learning](./media/how-to-manage-workspace/find-workspaces.png)

1. Examine la lista de áreas de trabajo encontradas. Puede filtrar en función de suscripción, grupos de recursos y ubicaciones.  

1. Seleccione un área de trabajo para mostrar sus propiedades.

---


## <a name="delete-a-workspace"></a>Eliminar un área de trabajo

Cuando ya no necesite un área de trabajo, elimínela.  

# <a name="python"></a>[Python](#tab/python)

Elimine el área de trabajo `ws`:

```python
ws.delete(delete_dependent_resources=False, no_wait=False)
```

La acción predeterminada es no eliminar los recursos asociados al área de trabajo, es decir, el registro de contenedor, la cuenta de almacenamiento, el almacén de claves y Application Insights.  Establezca `delete_dependent_resources` en True para eliminar también estos recursos.

# <a name="portal"></a>[Portal](#tab/azure-portal)

En [Azure Portal](https://portal.azure.com/), seleccione **Eliminar** en la parte superior del área de trabajo que desea eliminar.

:::image type="content" source="./media/how-to-manage-workspace/delete-workspace.png" alt-text="Eliminación del área de trabajo":::

---

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="troubleshooting"></a>Solución de problemas

### <a name="resource-provider-errors"></a>Errores del proveedor de recursos

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Movimiento del área de trabajo

> [!WARNING]
> No se admite mover el área de trabajo de Azure Machine Learning a otra suscripción ni mover la suscripción propietaria a un nuevo inquilino. Si lo hace, pueden producirse errores.

### <a name="deleting-the-azure-container-registry"></a>Eliminación de la instancia de Azure Container Registry

El área de trabajo de Azure Machine Learning usa Azure Container Registry (ACR) para algunas operaciones. La primera vez que se necesite una instancia de ACR, se creará automáticamente.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="examples"></a>Ejemplos

Ejemplos de creación de un área de trabajo:
* Uso de Azure Portal para [crear un área de trabajo y una instancia de proceso](tutorial-1st-experiment-sdk-setup.md).
* Uso del SDK de Python para [crear un área de trabajo en su propio entorno](tutorial-1st-experiment-sdk-setup-local.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga un área de trabajo, obtenga información sobre cómo [entrenar e implementar un modelo](tutorial-train-models-with-aml.md).
