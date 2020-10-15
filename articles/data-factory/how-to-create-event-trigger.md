---
title: Creación de desencadenadores basados en eventos en Azure Data Factory
description: Aprenda a crear un desencadenador en Azure Data Factory que ejecute una canalización en respuesta a un evento.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 10/18/2018
ms.openlocfilehash: 10f0079f47e5d2fd99b358fcc5cfb4c80aa9bd91
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84508903"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Creación de un desencadenador que ejecuta una canalización en respuesta a un evento
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describen los desencadenadores basados en eventos que se pueden crear en las canalizaciones de Data Factory.

La arquitectura dirigida por eventos (EDA) es un patrón de integración de datos común que implica la producción, detección, consumo y reacción a los eventos. Los escenarios de integración de datos a menudo requieren que los clientes de Data Factory desencadenen canalizaciones basadas en eventos como la llegada o eliminación de un archivo en la cuenta de Azure Storage. Data Factory está ahora integrado con [Azure Event Grid](https://azure.microsoft.com/services/event-grid/), que le permite desencadenar canalizaciones en un evento.

Si desea ver una demostración y una introducción de diez minutos de esta característica, vea el siguiente vídeo de nueve minutos de duración:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Event-based-data-integration-with-Azure-Data-Factory/player]


> [!NOTE]
> La integración descrita en este artículo depende de [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Asegúrese de que el proveedor de la suscripción se registra con el proveedor de recursos de Event Grid. Para más información, consulte [Tipos y proveedores de recursos](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). Debe poder realizar la acción *Microsoft.EventGrid/eventSubscriptions/* *. Esta acción forma parte del rol integrado Colaborador de EventSubscription EventGrid.

## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory

En esta sección se muestra cómo crear un desencadenador de eventos dentro de la interfaz de usuario de Azure Data Factory.

1. Vaya al **Lienzo de creación**

1. En la esquina inferior izquierda, haga clic en el botón **Desencadenadores**.

1. Haga clic en **+ Nuevo** para abrir la navegación del lado Crear desencadenador.

1. Seleccionar un tipo de desencadenador **Evento**

    ![Crear un desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image1.png)

1. Seleccione la cuenta de almacenamiento en la lista desplegable de suscripción de Azure o manualmente con el identificador de recurso de la cuenta de almacenamiento. Elija en qué contenedor quiere que se produzcan los eventos. La selección de contenedores es opcional, pero tenga en cuenta que si se seleccionan todos los contenedores el número de eventos puede resultar grande.

   > [!NOTE]
   > Actualmente, el desencadenador de eventos solo es compatible con las cuentas de almacenamiento de Azure Data Lake Storage Gen2 y de la versión 2 de uso general. Debe tener como mínimo acceso de *Propietario* en la cuenta de almacenamiento.  Debido a una limitación de Azure Event Grid, Azure Data Factory solo admite un máximo de 500 desencadenadores de eventos por cuenta de almacenamiento.

1. Las propiedades **Blob path begins with** y **Blob path ends with** permiten especificar los contenedores, las carpetas y los nombres de blob en los que quiere recibir eventos. El desencadenador de eventos requiere que se defina al menos una de estas propiedades. Como se muestra en ejemplos que podrá encontrar en este mismo artículo, se pueden usar varios patrones para las propiedades **Blob path begins with** y **Blob path ends with**.

    * **Blob path begins with:** la ruta de acceso del blob debe comenzar con una ruta de acceso de carpeta. Entre los valores válidos se incluyen `2018/` y `2018/april/shoes.csv`. No se puede seleccionar este campo si no se ha seleccionado un contenedor.
    * **Blob path ends with:** la ruta de acceso del blob debe terminar con un nombre de archivo o una extensión. Entre los valores válidos se incluyen `shoes.csv` y `.csv`. El nombre de contenedor y carpeta es opcional, pero, cuando se especifica, deben estar separados por un segmento `/blobs/`. Por ejemplo, un contenedor denominado "Orders" puede tener un valor de `/orders/blobs/2018/april/shoes.csv`. Para especificar una carpeta en cualquier contenedor, omita el carácter "/" inicial. Por ejemplo, `april/shoes.csv` desencadenará un evento en cualquier archivo denominado `shoes.csv` en la carpeta a denominada "April" en cualquier contenedor. 

1. Seleccione si el desencadenador responderá a un evento **Blob creado**, **Blob eliminado** o a ambos. En la ubicación de almacenamiento especificada, cada evento desencadenará las canalizaciones Data Factory asociadas al desencadenador.

    ![Configuración del desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image2.png)

1. Seleccione si el desencadenador omitirá o no los blobs con cero bytes.

1. Una vez que haya configurado el desencadenador, haga clic en **Siguiente: Vista previa de datos**. Esta pantalla muestra los blobs existentes que coinciden con la configuración del desencadenador de eventos. Asegúrese de que tiene filtros específicos. La configuración de filtros demasiado amplios puede coincidir con un gran número de archivos creados o eliminados y puede afectar significativamente al costo. Una vez comprobadas las condiciones de filtro, haga clic en **Finalizar**.

    ![Vista previa de datos del desencadenador de eventos](media/how-to-create-event-trigger/event-based-trigger-image3.png)

1. Para adjuntar una canalización a este desencadenador, vaya al lienzo de canalización, haga clic en **Agregar desencadenador** y seleccione **Nuevo/editar**. Cuando aparezca la barra de navegación lateral, haga clic en el desplegable **Choose trigger** (Elegir desencadenador...) y seleccione el desencadenador que creó. Haga clic en **Siguiente: Vista previa de los datos** para confirmar que la configuración es correcta y, después, **Siguiente** para validar que la vista previa de los datos es correcta.

1. Si la canalización tiene parámetros, puede especificarlos en el parámetro de ejecución del desencadenador. El desencadenador de eventos captura el nombre de archivo y la ruta de acceso del blob en las propiedades `@trigger().outputs.body.folderPath` y `@trigger().outputs.body.fileName`. Para usar los valores de estas propiedades en una canalización, debe asignar las propiedades a los parámetros de la canalización. Después de asignar las propiedades a los parámetros, puede tener acceso a los valores capturados por el desencadenador mediante la expresión `@pipeline().parameters.parameterName` en toda la canalización. Cuando haya terminado, haga clic en **Finalizar**.

    ![Asignación de propiedades a los parámetros de la canalización](media/how-to-create-event-trigger/event-based-trigger-image4.png)

En el ejemplo anterior, el desencadenador está configurado para activarse cuando se crea una ruta de acceso de blob que termina en .csv en la carpeta prueba-de-eventos en el contenedor datos-de-ejemplo. Las propiedades **folderPath** y **fileName** capturan la ubicación del nuevo blob. Por ejemplo, cuando se agrega MoviesDB.csv a la ruta de acceso datos-de-ejemplo/prueba-de-eventos, `@trigger().outputs.body.folderPath` tiene un valor de `sample-data/event-testing` y `@trigger().outputs.body.fileName` tiene un valor de `moviesDB.csv`. Estos valores se asignan en el ejemplo a los parámetros de canalización `sourceFolder` y `sourceFile`, que se pueden utilizar en toda la canalización como `@pipeline().parameters.sourceFolder` y `@pipeline().parameters.sourceFile` respectivamente.

## <a name="json-schema"></a>Esquema JSON

En la tabla siguiente se proporciona información general acerca de los elementos de esquema que están relacionados con los desencadenadores basados en eventos:

| **Elemento de JSON** | **Descripción** | **Tipo** | **Valores permitidos** | **Obligatorio** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | El identificador de recursos de Azure Resource Manager de la cuenta de almacenamiento. | String | Identificador de Azure Resource Manager | Sí |
| **eventos** | El tipo de eventos que provocan la activación de este desencadenador. | Array    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Sí, cualquier combinación de estos valores. |
| **blobPathBeginsWith** | La ruta de acceso del blob debe comenzar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, `/records/blobs/december/` solo activa el desencadenador de blobs en la carpeta `december` bajo el contenedor `records`. | String   | | Tendrá que proporcionar un valor para al menos una de estas propiedades: `blobPathBeginsWith` o `blobPathEndsWith`. |
| **blobPathEndsWith** | La ruta de acceso del blob debe finalizar con el patrón proporcionado para que se active el desencadenador. Por ejemplo, `december/boxes.csv` solo activa el desencadenador de blobs denominado "`boxes`" en una carpeta `december`. | String   | | Tendrá que proporcionar un valor para al menos una de estas propiedades: `blobPathBeginsWith` o `blobPathEndsWith`. |
| **ignoreEmptyBlobs** | Indica si los blobs de cero bytes desencadenarán o no la ejecución de una canalización. De manera predeterminada, se establece en true. | Boolean | true o false | No |

## <a name="examples-of-event-based-triggers"></a>Ejemplos de desencadenadores basados en eventos

En esta sección encontrará ejemplos de configuración de desencadenadores basados en eventos.

> [!IMPORTANT]
> Debe incluir el segmento `/blobs/` de la ruta de acceso, tal como se muestra en los siguientes ejemplos, siempre que especifique el contenedor y la carpeta, el contenedor y el archivo, o el contenedor, la carpeta y el archivo. En el caso de **blobPathBeginsWith**, la interfaz de usuario de Data Factory agregará automáticamente `/blobs/` entre la carpeta y el nombre del contenedor en el JSON del desencadenador.

| Propiedad | Ejemplo | Descripción |
|---|---|---|
| **Ruta de acceso de blobs que empieza con** | `/containername/` | Recibe eventos de cualquier blob del contenedor. |
| **Ruta de acceso de blobs que empieza con** | `/containername/blobs/foldername/` | Recibe eventos de los blobs en el contenedor `containername` y la carpeta `foldername`. |
| **Ruta de acceso de blobs que empieza con** | `/containername/blobs/foldername/subfoldername/` | También puede hacer referencia a una subcarpeta. |
| **Ruta de acceso de blobs que empieza con** | `/containername/blobs/foldername/file.txt` | Recibe eventos de un blob denominado `file.txt` en la carpeta `foldername`, en el contenedor `containername`. |
| **Ruta de acceso de blobs que termina con** | `file.txt` | Recibe eventos para un blob denominado `file.txt` en cualquier ruta de acceso. |
| **Ruta de acceso de blobs que termina con** | `/containername/blobs/file.txt` | Recibe eventos de un blob denominado `file.txt` en el contenedor `containername`. |
| **Ruta de acceso de blobs que termina con** | `foldername/file.txt` | Recibe eventos de un blob denominado `file.txt` en la carpeta `foldername` en cualquier contenedor. |

## <a name="next-steps"></a>Pasos siguientes
Para obtener información detallada acerca de los desencadenadores, consulte el artículo [Pipeline execution and triggers](concepts-pipeline-execution-triggers.md#trigger-execution) (Ejecución de canalizaciones y desencadenadores).
