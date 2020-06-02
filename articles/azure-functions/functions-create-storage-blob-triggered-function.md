---
title: Creación de una función en Azure desencadenada por Blob Storage
description: Use Azure Functions para crear una función sin servidor que se invoca mediante elementos agregados al contenedor de Blob Storage.
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: bf6865d2756579f457dded90b247326d2eec137c
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123129"
---
# <a name="create-a-function-in-azure-thats-triggered-by-blob-storage"></a>Creación de una función en Azure desencadenada por Blob Storage

Obtenga información sobre cómo crear una función que se desencadena cuando se cargan o se actualizan archivos en un contenedor de Blob Storage.

## <a name="prerequisites"></a>Prerrequisitos

+ Suscripción a Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-function-app"></a>Creación de una Function App de Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ha creado correctamente la nueva aplicación de funciones.

:::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-create-success.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

Después, cree una función en la nueva Function App.

<a name="create-function"></a>

## <a name="create-an-azure-blob-storage-triggered-function"></a>Creación de una función desencadenada por Azure Blob Storage

1. Seleccione **Functions** y, a continuación, **+ Agregar** para agregar una función nueva.

   :::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-quickstart-choose-template.png" alt-text="Elija una plantilla de función en Azure Portal." border="true":::

1. Elija la plantilla **Azure Blob Storage trigger** (Desencadenador de Azure Blob Storage).

1. Utilice la configuración que se especifica en la tabla debajo de la imagen.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-2.png" alt-text="Asignar un nombre y configurar la función desencadenada de Blob Storage" border="true":::

    | Configuración | Valor sugerido | Descripción |
    |---|---|---|
    | **Nueva función** | Único en la Function App | Nombre de la función desencadenada por este blob. |
    | **Path**   | samples-workitems/{name}    | Ubicación de Blob Storage que se está supervisando. El nombre de archivo del blob se pasa en el enlace como parámetro _name_.  |
    | **Conexión de la cuenta de almacenamiento** | AzureWebJobsStorage | Puede usar la conexión de cuenta de almacenamiento que ya usa la Function App o crear una nueva.  |

1. Seleccione **Crear función** para crear la función.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-3.png" alt-text="Cree la función desencadenada por Blob Storage." border="true":::

A continuación, cree el contenedor **samples-workitems**.

## <a name="create-the-container"></a>Cree el contenedor.

1. En la función, en la página **Información general**, seleccione el grupo de recursos.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-resource-group.png" alt-text="Seleccione el grupo de recursos de Azure Portal." border="true":::

1. Busque y seleccione la cuenta de almacenamiento del grupo de recursos.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-account-access.png" alt-text="Acceda a la cuenta de almacenamiento." border="true":::

1. Elija **Containers** y, a continuación, elija **+ Contenedor**. 

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-add-container.png" alt-text="Agregue el contenedor en la cuenta de almacenamiento de Azure Portal." border="true":::

1. En el campo **Nombre**, escriba `samples-workitems` y seleccione **Crear**.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-name-blob-container.png" alt-text="Escriba el contenedor de almacenamiento." border="true":::

Ahora que tiene un contenedor de blobs, puede probar la función. Para ello, cargue un archivo en el contenedor.

## <a name="test-the-function"></a>Prueba de la función

1. De nuevo en Azure Portal, vaya a la función. Expanda **Registros** en la parte inferior de la página y asegúrese de que el streaming de registros no está en pausa.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-log-expander.png" alt-text="Expanda el inicio de sesión en Azure Portal" border="true":::.

1. En una ventana del explorador independiente, vaya al grupo de recursos en Azure Portal y seleccione la cuenta de almacenamiento.

1. Seleccione **Containers** y, después, seleccione el contenedor **samples-workitems**.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-container.png" alt-text="Vaya al contenedor samples-workitems en Azure Portal." border="true":::

1. Seleccione **Cargar** y, después, el icono de la carpeta para elegir el archivo que quiera cargar.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png" alt-text="Cargue un archivo en el contenedor de blobs." border="true":::

1. Busque un archivo en el equipo local, como un archivo de imagen, y selecciónelo. Seleccione **Abrir** y, a continuación, **Cargar**.

1. Vuelva a sus registros de función y compruebe que se ha leído el blob.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png" alt-text="Vea el mensaje en los registros." border="true":::

    >[!NOTE]
    > Cuando la Function App se ejecuta en el plan de consumo predeterminado, puede haber un retraso de hasta varios minutos entre el momento en que se agrega o se actualiza el blob y el momento en que se desencadena la función. Si necesita una latencia baja en las funciones desencadenadas por el blob, considere la posibilidad de ejecutar la Function App en un plan de App Service.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una función que se ejecuta cuando se agrega o se actualiza un blob en Blob Storage. Para obtener más información sobre los desencadenadores de Blob Storage, vea [Enlaces de Blob Storage en Azure Functions](functions-bindings-storage-blob.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
