---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 131b21ea7bc47df9654dd7c163eb22adb68e6678
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185424"
---
## <a name="create-a-spatial-anchors-resource"></a>Creación de un recurso de Spatial Anchors

### <a name="portal"></a>[Portal](#tab/azure-portal)

Vaya a <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

En el menú izquierdo, seleccione **Crear un recurso**.

Use el cuadro de búsqueda para buscar **Spatial Anchors**.

![Captura de pantalla que muestra los resultados de una búsqueda de Spatial Anchors.](./media/spatial-anchors-get-started-create-resource/portal-search.png)

Seleccione **Spatial Anchors** y, después, seleccione **Crear**.

En el panel **Cuenta de Spatial Anchors**, haga lo siguiente:

* Escriba un nombre de recurso único con caracteres alfanuméricos normales.  
* Seleccione la suscripción a la que desea asociar el recurso.  
* Cree un grupo de recursos, para lo que debe seleccionar **Crear nuevo**. Asígnele el nombre **myResourceGroup** y luego seleccione **Aceptar**.  

  [!INCLUDE [resource group intro text](resource-group.md)]
  
* Seleccione la ubicación (región) en la que desea colocar el recurso.  
* Seleccione **New** (Nuevo) para empezar a crear el recurso.

![Captura de pantalla del panel Spatial Anchors para crear un recurso.](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Una vez creado el recurso, Azure Portal muestra que se ha completado la implementación. 
   
![Captura de pantalla que muestra que la implementación del recurso se ha completado.](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

Haga clic en **Go to resource** (Ir al recurso). Ahora puede ver las propiedades del recurso. 
   
Copie el valor de **Id. de cuenta** del recurso en un editor de texto para usarlo más adelante.

![Captura de pantalla del panel de propiedades de recursos.](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Copie también el valor de **Account Domain** (Dominio de cuenta) del recurso en un editor de texto para usarlo más adelante.

![Captura de pantalla que muestra el valor del dominio de la cuenta del recurso.](./media/spatial-anchors-get-started-create-resource/view-resource-domain.png)

En **Configuración**, seleccione **Clave**. Copie el valor de **Clave principal**, **Clave de cuenta**, en un editor de texto para su uso posterior.

![Captura de pantalla del panel de claves de la cuenta.](./media/spatial-anchors-get-started-create-resource/view-account-key.png)

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Empiece por preparar el entorno para la CLI de Azure:

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](azure-cli-prepare-your-environment-no-header.md)]

1. Después de iniciar sesión, use el comando [az account set](/cli/azure/account#az_account_set) para seleccionar la suscripción en la que desea configurar la cuenta de Spatial Anchors:

   ```azurecli
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

1. Ejecute el comando [az group create](/cli/azure/group#az_group_create) para crear un grupo de recursos o use un grupo de recursos existente:

   ```azurecli
   az group create --name myResourceGroup --location eastus2
   ```

   [!INCLUDE [resource group intro text](resource-group.md)]

   Para ver las cuentas de Spatial Anchors actuales de un grupo de recursos, use el comando [az spatial-anchors-account list](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_list):

   ```azurecli
   az spatial-anchors-account list --resource-group myResourceGroup
   ```

   También puede ver las cuentas de Spatial Anchors de su suscripción:

   ```azurecli
   az spatial-anchors-account list
   ```

1. Ejecute el comando [az spatial-anchors-account create](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_create) para crear su cuenta de Spatial Anchors:

   ```azurecli
   az spatial-anchors-account create --resource-group myResourceGroup --name MySpatialAnchorsQuickStart --location eastus2
   ```

1. Para ver las propiedades de los recursos, use el comando [az spatial-anchors-account show](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_show):

   ```azurecli
   az spatial-anchors-account show --resource-group myResourceGroup --name MySpatialAnchorsQuickStart
   ```

   Copie también los valores de **Account ID** (Identificador de cuenta) y **Account Domain** (Dominio de cuenta) del recurso en un editor de texto para usarlo más adelante.

1. Ejecute el comando [az spatial-anchors-account key show](/cli/azure/ext/mixed-reality/spatial-anchors-account/key#ext_mixed_reality_az_spatial_anchors_account_key_show) para obtener sus claves principales y secundarias:

   ```azurecli
   az spatial-anchors-account key show --resource-group myResourceGroup --name MySpatialAnchorsQuickStart
   ```

   Copie los valores de clave en un editor de texto para su posterior uso.

   Si tiene que volver a generar las claves, use el comando [az spatial-anchors-account key renew](/cli/azure/ext/mixed-reality/spatial-anchors-account/key#ext_mixed_reality_az_spatial_anchors_account_key_renew):

   ```azurecli
   az spatial-anchors-account key renew --resource-group myResourceGroup --name example --key primary
   az spatial-anchors-account key renew --resource-group myResourceGroup --name example --key secondary
   ```

Puede eliminar una cuenta mediante el comando [az spatial-anchors-account delete](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_delete):

```azurecli
az spatial-anchors-account delete --resource-group myResourceGroup --name MySpatialAnchorsQuickStart
```

---
