---
title: Almacenamiento de las salidas de trabajos y tareas en un almacén de datos
description: Conozca las diferentes opciones para guardar los datos de salida de las tareas y los trabajos del servicio Batch. Puede almacenar datos en Azure Storage o en otro almacén de datos.
ms.topic: how-to
ms.date: 11/14/2018
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4ebe0b6d57225eff9f3f1251d5e491c95e9b7ffc
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85965117"
---
# <a name="persist-job-and-task-output"></a>Trabajo persistente y resultado de la tarea

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Algunos ejemplos comunes de resultados de tareas incluyen:

- Archivos creados cuando la tarea procesa los datos de entrada.
- Archivos de registro asociados con la ejecución de tareas.

En este artículo se describen diversas opciones para almacenar la salida de tareas.

## <a name="options-for-persisting-output"></a>Opciones para almacenar la salida

Según el escenario, puede adoptar enfoques diferentes para almacenar la salida de tarea:

- [Usar la API del servicio Batch](batch-task-output-files.md).  
- [Usar la biblioteca de Batch File Conventions para .NET](batch-task-output-file-conventions.md).  
- Implementar el estándar Batch File Conventions en su organización.
- Implementar una solución personalizada de movimiento de archivos.

En las secciones siguientes se describe brevemente cada enfoque, así como las consideraciones de diseño general para almacenar la salida.

### <a name="use-the-batch-service-api"></a>Usar la API del servicio Batch.

El servicio Batch es compatible con la especificación de archivos de salida en Azure Storage para los datos de tareas cuando [agrega una tarea a un trabajo](/rest/api/batchservice/add-a-task-to-a-job) o [agrega una colección de tareas a un trabajo](/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

Para más información sobre el almacenamiento de salidas de tareas con la API del servicio Batch, consulte [Persist task data to Azure Storage with the Batch service API](batch-task-output-files.md) (Almacenamiento de datos de tareas en Azure Storage con la API del servicio Batch).

### <a name="use-the-batch-file-conventions-library-for-net"></a>Usar la biblioteca Batch File Conventions para .NET

Batch define un conjunto opcional de convenciones para nombrar los archivos de salida de tareas en Azure Storage. El [estándar Batch File Conventions](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions) describe estas convenciones. El estándar de convenciones de archivos determina los nombres del contenedor de destino y la ruta de acceso del blob en Azure Storage para un archivo de salida dado según los nombres del trabajo y de la tarea.

Es cosa suya si decide usar el estándar de convenciones de archivos para nombrar los archivos de datos de salida. Si lo desea, puede nombrar también el contenedor de destino y el blob. Si usa el estándar de convenciones de archivos para la denominación de archivos de salida, estos se podrán ver en [Azure Portal][portal].

Los desarrolladores que compilan soluciones de Batch con C# y .NET pueden usar la [biblioteca File Conventions para .NET][nuget_package] para almacenar los datos de tareas en una cuenta de Azure Storage, de acuerdo con el [estándar Batch File Conventions](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions). La biblioteca File Conventions administra el movimiento de archivos de salida a Azure Storage y nombra los contenedores y blobs de destino de una manera conocida.

Para más información sobre el almacenamiento de salidas de tareas con la biblioteca File Conventions para .NET, consulte [Persist job and task data to Azure Storage with the Batch File Conventions library for .NET](batch-task-output-file-conventions.md) (Almacenamiento de datos de trabajos y tareas en Azure Storage con la biblioteca de convenciones de archivo de Batch para .NET).

### <a name="implement-the-batch-file-conventions-standard"></a>Implementar el estándar Batch File Conventions

Si va a usar un lenguaje distinto de .NET, puede implementar el [estándar Batch File Conventions](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/batch/Microsoft.Azure.Batch.Conventions.Files#conventions) en su propia aplicación.

Quizás quiera implementar el estándar de nomenclatura File Conventions por su cuenta cuando desee un esquema de nomenclatura probado o cuando quiera ver la salida de tarea en Azure Portal.

### <a name="implement-a-custom-file-movement-solution"></a>Implementar una solución personalizada de movimiento de archivos

También puede implementar su propia solución completa de movimiento de archivos. Use este enfoque en los siguientes casos:

- Quiere almacenar datos de tareas en un almacén de datos distinto de Azure Storage. Para cargar archivos en un almacén de datos como Azure SQL o Azure DataLake, puede crear un script personalizado o un ejecutable para cargar en esa ubicación. Luego, puede llamarlo en la línea de comandos después de ejecutar el ejecutable principal. Por ejemplo, en un nodo de Windows, puede llamar a estos dos comandos:`doMyWork.exe && uploadMyFilesToSql.exe`
- Quiere crear puntos de comprobación o realizar la carga anticipada de los resultados iniciales.
- Quiere mantener un control granular sobre el control de errores. Por ejemplo, puede que quiera implementar su propia solución si desea usar acciones de dependencia de tareas para realizar determinadas acciones de carga basadas en códigos específicos de salida de tarea. Para más información sobre las acciones de dependencia de tareas, consulte [Creación de dependencias de tareas para ejecutar las tareas que dependan de otras tareas](batch-task-dependencies.md).

## <a name="design-considerations-for-persisting-output"></a>Consideraciones de diseño para el almacenamiento de la salida

Cuando diseñe su solución de Batch, tenga en cuenta los siguientes factores relacionados con las salidas de trabajos y tareas.

- **Duración del nodo de proceso**: los nodos de proceso a menudo son transitorios, especialmente en los grupos con escalado automático habilitado. La salida de una tarea que se ejecuta en un nodo solo está disponible mientras existe el nodo, y solo dentro del período de retención de archivos que ha definido para la tarea. Si una tarea produce una salida que puede ser necesaria una vez completada la tarea, la tarea debe cargar sus archivos de salida en un almacén duradero, como Azure Storage.

- **Almacenamiento de la salida**: Azure Storage es el almacén de datos recomendado para la salida de tareas; no obstante, puede usar cualquier almacenamiento duradero. La escritura de la salida de tarea en Azure Storage está integrada en la API del servicio Batch. Si usa otra forma de almacenamiento duradero, deberá escribir la lógica de la aplicación para almacenar la salida de tarea por sí mismo.

- **Recuperación de la salida**: puede recuperar la salida de tarea directamente de los nodos de proceso del grupo o desde Azure Storage u otro almacén de datos si la ha almacenado. Para recuperar la salida de una tarea directamente desde un nodo de proceso, necesita el nombre del archivo y la ubicación de la salida en el nodo. Si almacena la salida de tarea en Azure Storage, necesitará la ruta de acceso completa al archivo en Azure Storage para descargar los archivos de salida con el SDK de Azure Storage.

- **Visualización de la salida**: si navega a una tarea de Batch en Azure Portal y selecciona **Archivos en el nodo**, se mostrarán todos los archivos asociados a la tarea y no solo los archivos de salida que le interesan. Es más, los archivos de un nodo de proceso solo están disponibles mientras existe el nodo y solo durante el tiempo de retención del archivo que se haya establecido para la tarea. Para ver la salida de tarea que ha almacenado en Azure Storage, puede usar Azure Portal o una aplicación cliente de Azure Storage, como el [Explorador de Azure Storage][storage_explorer]. Para ver los datos de salida en Azure Storage con el portal u otra herramienta, debe saber la ubicación del archivo y desplazarse hasta ella directamente.

## <a name="next-steps"></a>Pasos siguientes

- Examine el uso de las nuevas características de la API del servicio Batch para almacenar los datos de tareas en [Persist task data to Azure Storage with the Batch service API](batch-task-output-files.md) (Almacenamiento de datos de tareas en Azure Storage con la API del servicio Batch).
- Obtenga información sobre cómo usar la biblioteca de convenciones de archivo de Batch para .NET en [Persist job and task data to Azure Storage with the Batch File Conventions library for .NET](batch-task-output-file-conventions.md). (Guardar datos de trabajos y tareas en Azure Storage con la biblioteca de convenciones de archivo para .NET).
- Consulte el proyecto de ejemplo [PersistOutputs][github_persistoutputs] en GitHub, que demuestra cómo usar la biblioteca de cliente de Batch para .NET y la biblioteca File Conventions para .NET con la finalidad de almacenar salidas de tareas en almacenamiento duradero.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: https://storageexplorer.com/
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs 
