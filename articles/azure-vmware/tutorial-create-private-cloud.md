---
title: 'Tutorial: Implementación del clúster de vSphere en Azure'
description: Aprenda a implementar un clúster de vSphere en Azure mediante la solución de VMWare en Azure (AVS)
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: fc753f43563650357cf43c102e94f0057b62a406
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873737"
---
# <a name="tutorial-deploy-an-avs-private-cloud-in-azure"></a>Tutorial: Implementación de una nube privada de AVS en Azure

La solución de VMware en Azure (AVS) permite implementar un clúster de vSphere en Azure. La implementación inicial mínima es de tres hosts. Se pueden agregar más hosts de uno en uno, hasta un máximo de 16 hosts por clúster. 

Dado que AVS no le permitirá administrar su nube privada con su instancia local de vCenter inicialmente, deberá conectar y configurar adicionalmente una instancia de vCenter local, una red virtual, etc. Estos procedimientos y los requisitos previos relacionados se tratarán en esta serie de tutoriales.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una nube privada de AVS
> * Comprobar la nube privada implementada

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Permisos y derechos administrativos adecuados para crear una nube privada.
- Asegúrese de que tiene las opciones de red correctamente configuradas como se describe en [Tutorial: Lista de comprobación de red](tutorial-network-checklist.md).

## <a name="register-the-resource-provider"></a>Registrar el proveedor de recursos

Para poder usar la solución de VMware en Azure, primero debe registrar el proveedor de recursos. En el ejemplo siguiente se registra el proveedor de recursos con su suscripción.

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

Para ver otras formas de registrar el proveedor de recursos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-private-cloud"></a>Creación de una nube privada

Puede crear una nube privada de AVS mediante [Azure Portal](#azure-portal) o con la [CLI de Azure](#azure-cli).

### <a name="azure-portal"></a>Azure Portal

En Azure Portal, seleccione **+ Crear un recurso**. En el cuadro de texto **Buscar en el Marketplace**, escriba `Azure VMware Solution` y seleccione **Azure VMware Solution** en la lista. En la ventana **Azure VMware Solution**, seleccione **Crear**.

En la pestaña **Básica**, escriba valores en los campos. En la tabla siguiente se muestra una lista detallada de las propiedades.

| Campo   | Value  |
| ---| --- |
| **Suscripción** | La suscripción que va a usar para la implementación.|
| **Grupos de recursos** | El grupo de recursos para los recursos de nube privada. |
| **Ubicación** | Seleccione una ubicación, como **Este de EE. UU.**|
| **Nombre del recurso** | El nombre de la nube privada de AVS. |
| **SKU** | Seleccione el valor de SKU siguiente: AV36 |
| **Hosts** | Número de hosts que se van a agregar al clúster de nube privada. El valor predeterminado es 3. Este valor se puede aumentar o reducir después de la implementación.  |
| **Contraseña del administrador de vCenter** | Escriba una contraseña de administrador de la nube. |
| **Contraseña de administrador de NSX-T** | Escriba una contraseña de administrador de NSX-T. |
| **Bloque de direcciones** | Escriba un bloque de direcciones IP para la red CIDR para la nube privada. Por ejemplo, 10.175.0.0/22. |

:::image type="content" source="./media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="Creación de una nube privada" border="true":::

Cuando termine, seleccione **Revisar y crear**. En la siguiente pantalla, compruebe la información especificada. Si toda la información es correcta, seleccione **Crear**.

> [!NOTE]
> Este paso tarda aproximadamente dos horas. 

### <a name="azure-cli"></a>Azure CLI

También puede usar la CLI de Azure para crear una nube privada de AVS en Azure. Para ello, puede usar Azure Cloud Shell, tal y como se describe en los pasos siguientes.

#### <a name="open-azure-cloud-shell"></a>Apertura de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta.

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a https://shell.azure.com/bash para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione **Entrar** para ejecutarlos.

#### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para crear un grupo de recursos, use el comando [az group create](/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Creación de una nube privada

Para crear una nube privada de AVS, debe proporcionar un nombre de grupo de recursos, un nombre para la nube privada, una ubicación y el tamaño del clúster.


|Propiedad  |Descripción  |
|---------|---------|
|Nombre del grupo de recursos     | El nombre del grupo de recursos en el que va a implementar la nube privada.        |
|Nombre de la nube privada     | El nombre de la nube privada.        |
|Location     | La ubicación usada para la nube privada.         |
|Tamaño del clúster     | El tamaño del clúster. El valor mínimo es 3.         |
|Bloque de red     | El intervalo de CIDR que se va a usar para la nube privada. Se recomienda que sea único en su entorno local, así como en el entorno de Azure.        |

```azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22
```

## <a name="verify-deployment-was-successful"></a>Comprobación de que la implementación se ha realizado correctamente

Vaya al grupo de recursos que ha creado y seleccione la nube privada. Cuando se complete la implementación, verá la siguiente pantalla con el estado **Correcto**.

:::image type="content" source="./media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Validación de la nube privada implementada" border="true":::

## <a name="delete-a-private-cloud"></a>Eliminación de una nube privada

Si tiene una nube privada de AVS que ha comprobado que ya no necesita, puede eliminarla. Al eliminar una nube privada, se eliminan todos los clústeres junto con todos sus componentes.

Para ello, vaya a su nube privada en Azure Portal y seleccione **Eliminar**. En la página de confirmación, confirme el nombre de la nube privada y seleccione **Sí**.

> [!CAUTION]
> La eliminación de la nube privada es una operación irreversible. Una vez eliminada la nube privada, no se pueden recuperar los datos, ya que se finalizan todas las cargas de trabajo en ejecución, se eliminan los componentes y se destruyen todos los datos y valores de configuración de la nube privada, incluidas las direcciones IP públicas. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Crear una nube privada de AVS
> * Comprobar la nube privada implementada

Continúe con el siguiente tutorial para aprender a crear una red virtual para su uso con la nube privada como parte de la configuración de la administración local de los clústeres de nube privada.

> [!div class="nextstepaction"]
> [Creación de una red virtual](tutorial-configure-networking.md)
