---
title: Elemento de interfaz de usuario StorageBlobSelector
description: Describe el elemento de la interfaz de usuario Microsoft.Storage.StorageBlobSelector para Azure Portal.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/27/2020
ms.author: tomfitz
ms.openlocfilehash: 1085b70df67a3f16a7f7f8c5ae85c9ab271b62ac
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754296"
---
# <a name="microsoftstoragestorageblobselector-ui-element"></a>Elemento de interfaz de usuario Microsoft.Storage.StorageBlobSelector

Un control para seleccionar un blob de una cuenta de Azure Storage.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

Al usuario se le presenta la opción de buscar blobs de almacenamiento disponibles.

:::image type="content" source="./media/managed-application-elements/microsoft-storage-storageblobselector-browse.png" alt-text="Microsoft.Storage.StorageBlobSelector: examinar":::

Después de seleccionar **Examinar** , el usuario puede seleccionar una cuenta de almacenamiento.

:::image type="content" source="./media/managed-application-elements/microsoft-storage-storageblobselector-select.png" alt-text="Microsoft.Storage.StorageBlobSelector: seleccionar almacenamiento":::

El usuario verá los contenedores en la cuenta de almacenamiento y podrá seleccionar uno.

:::image type="content" source="./media/managed-application-elements/microsoft-storage-storageblobselector-containers.png" alt-text="Microsoft.Storage.StorageBlobSelector: seleccionar contenedor":::

En el contenedor, el usuario puede seleccionar un archivo.

:::image type="content" source="./media/managed-application-elements/microsoft-storage-storageblobselector-file.png" alt-text="Microsoft.Storage.StorageBlobSelector: archivo":::

El control se actualiza para mostrar el nombre del archivo seleccionado.

:::image type="content" source="./media/managed-application-elements/microsoft-storage-storageblobselector-result.png" alt-text="Microsoft.Storage.StorageBlobSelector: mostrar el archivo seleccionado":::

## <a name="schema"></a>Schema

```json
{
  "name": "storageBlobSelection",
  "type": "Microsoft.Storage.StorageBlobSelector",
  "visible": true,
  "toolTip": "Select storage blob",
  "label": "Package (.zip, .cspkg)",
  "options": {
    "text": "Select Package"
  },
  "constraints": {
    "allowedFileExtensions": [ "zip", "cspkg" ]
  }
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
{
  "blobName": "test.zip",
  "sasUri": "https://azstorageaccnt1.blob.core.windows.net/container1/test.zip?sp=r&se=2020-10-10T07:46:22Z&sv=2019-12-12&sr=b&sig=X4EL8ZsRmiP1TVxkVfTcGyMj2sHg1zCbFBXsDmnNOyg%3D"
}

```

## <a name="remarks"></a>Observaciones

La propiedad **constraints.allowedFileExtensions** especifica los tipos de archivo permitidos.

## <a name="next-steps"></a>Pasos siguientes

* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
