---
title: Actividad Data Flow
description: Cómo ejecutar flujos de datos desde una canalización de factoría de datos.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 04/30/2020
ms.openlocfilehash: a2e80b9320509144456663672ac5ae03f522459a
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735392"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Actividad de Data Flow en Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Utilice la actividad de Data Flow para transformar y trasladar datos a través de flujos de datos de asignación. Si no está familiarizado con los flujos de datos, consulte esta [introducción a los flujos de datos de asignación](concepts-data-flow-overview.md).

## <a name="syntax"></a>Sintaxis

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "compute": {
         "coreCount": 8,
         "computeType": "General"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
dataflow | Referencia al flujo de datos que se está ejecutando. | DataFlowReference | Sí
integrationRuntime | Entorno de proceso en el que se ejecuta el flujo de datos. Si no se especifica, se usará la resolución automática del entorno de Azure Integration Runtime. Solo se admiten entornos de ejecución de integración de resolución automática de la región. | IntegrationRuntimeReference | No
compute.coreCount | Número de núcleos utilizados en el clúster de Spark. Solo se puede especificar si se usa la resolución automática de Azure Integration Runtime | 8, 16, 32, 48, 80, 144, 272 | No
compute.computeType | Tipo de proceso utilizado en el clúster de Spark. Solo se puede especificar si se usa la resolución automática de Azure Integration Runtime | "General", "ComputeOptimized", "MemoryOptimized" | No
staging.linkedService | Si usa un origen o un receptor de Azure SQL Data Warehouse, es la cuenta de almacenamiento que se utiliza como almacenamiento provisional de PolyBase. | LinkedServiceReference | Solo si el flujo de datos lee o escribe en una instancia de Azure SQL Data Warehouse.
staging.folderPath | Si usa un origen o un receptor de Azure SQL Data Warehouse, es la ruta de la carpeta de la cuenta de almacenamiento de blobs que se utiliza como almacenamiento provisional de PolyBase. | String | Solo si el flujo de datos lee o escribe en una instancia de Azure SQL Data Warehouse.

![Ejecución de flujo de datos](media/data-flow/activity-data-flow.png "Ejecución de flujo de datos")

### <a name="dynamically-size-data-flow-compute-at-runtime"></a>Ajuste dinámico del tamaño del proceso de flujo de datos en tiempo de ejecución

Las propiedades Recuento de núcleos y Tipo de proceso se pueden configurar dinámicamente para ajustarse al tamaño de los datos de origen entrantes en tiempo de ejecución. Use actividades de canalización como Búsqueda u Obtener metadatos para averiguar el tamaño de los datos del conjunto de datos de origen. Tras ello, use Agregar contenido dinámico en las propiedades de la actividad del flujo de datos.

![Flujo de datos dinámicos](media/data-flow/dyna1.png "Flujo de datos dinámicos")

[Este es un breve tutorial en formato de vídeo en el que se explica esta técnica.](https://www.youtube.com/watch?v=jWSkJdtiJNM)

### <a name="data-flow-integration-runtime"></a>Entorno de ejecución de integración de Data Flow

Seleccione el entorno de ejecución de integración que desee usar con la ejecución de la actividad de Data Flow. De forma predeterminada, Data Factory usará la función de resolución automática de Azure Integration Runtime con cuatro núcleos de trabajo y sin período de vida (TTL). Este entorno de ejecución de integración tendrá un tipo de proceso de uso general y se ejecutará en la misma región que la fábrica. Puede crear entornos de ejecución de integración de Azure propios que definan regiones específicas, el tipo de proceso, el número de núcleos y el período de vida de la ejecución de actividades del flujo de datos. En este momento, solo se admiten en la actividad de flujo de datos los entornos de ejecución de integración de resolución automática de la región.

En el caso de las ejecuciones de canalización, el clúster que se utiliza es de trabajo y tarda varios minutos en arrancar antes de que se inicie la ejecución. Si no se especifica ningún período de vida, cada ejecución de la canalización tardará ese tiempo en iniciarse. Si especifica un período de vida, habrá un grupo de clústeres semiactivos preparados durante el tiempo que se especifique después de la última ejecución, lo que reducirá los tiempos de inicio. Por ejemplo, si tiene un período de vida de 60 minutos y ejecuta un flujo de datos cada hora, el grupo de clústeres permanecerá activo. Para más información, consulte [Azure Integration Runtime](concepts-integration-runtime.md).

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!IMPORTANT]
> La selección del entorno de ejecución de integración en la actividad de Data Flow solo se aplica a las *ejecuciones activadas* de su canalización. La canalización con flujos de datos se depurará en el clúster que se haya especificado en la sesión de depuración.

### <a name="polybase"></a>PolyBase

Si utiliza Azure SQL Data Warehouse como origen o receptor, debe elegir una ubicación de almacenamiento provisional para la carga por lotes de PolyBase. PolyBase utiliza la carga por lotes en lugar de cargar los datos fila a fila. PolyBase reduce considerablemente el tiempo de carga en Azure SQL Data Warehouse.

## <a name="parameterizing-data-flows"></a>Flujos de datos con parámetros

### <a name="parameterized-datasets"></a>Conjuntos de datos con parámetros

Si el flujo de datos utiliza conjuntos de datos con parámetros, establezca los valores de los parámetros en la pestaña **Configuración**.

![Parámetros de ejecución de flujo de datos](media/data-flow/params.png "Parámetros")

### <a name="parameterized-data-flows"></a>Flujos de datos con parámetros

Si el flujo de datos tiene parámetros, establezca los valores dinámicos de los parámetros de flujo de datos en la pestaña **Parámetros**. Puede usar el lenguaje de expresiones de canalización de ADF o el lenguaje de expresión de Data Flow para asignar valores dinámicos o literales a los parámetros. Para más información, consulte [Parámetros de Data Flow](parameters-data-flow.md).

### <a name="parameterized-compute-properties"></a>Propiedades de proceso con parámetros.

Puede parametrizar el número de núcleos o el tipo de proceso si usa la resolución automática de Azure Integration Runtime y especifica valores para compute.coreCount y compute.computeType.

![Ejemplo de parámetros de ejecución de flujo de datos](media/data-flow/parameterize-compute.png "Ejemplo de parámetro")

## <a name="pipeline-debug-of-data-flow-activity"></a>Depuración de la canalización de actividades de Data Flow

Para ejecutar una canalización de depuración con una actividad de Data Flow, debe activar el modo de depuración del flujo de datos a través del control deslizante **Data Flow Debug** (Depuración de flujo de datos) en la barra superior. El modo de depuración permite ejecutar el flujo de datos en un clúster de Spark activo. Para más información, consulte [Modo de depuración](concepts-data-flow-debug-mode.md).

![Botón Depurar](media/data-flow/debugbutton.png "Botón Depurar")

La canalización de depuración se ejecuta en el clúster de depuración activo, no en el entorno de ejecución de integración especificado en la configuración de la actividad de Data Flow. Puede elegir el entorno de proceso de depuración al iniciar el modo de depuración.

## <a name="monitoring-the-data-flow-activity"></a>Supervisión de la actividad de Data Flow

La supervisión de la actividad de Data Flow se realiza de forma especial, ya que permite ver información sobre las particiones, el tiempo de cada fase y el linaje de datos. Abra el panel supervisión utilizando el icono con forma de gafas que encontrará en **Acciones**. Para más información, consulte [Supervisión de flujos de datos](concepts-data-flow-monitoring.md).

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>Uso de los resultados de actividad de Data Flow en una actividad posterior

La actividad de flujo de datos genera métricas con respecto al número de filas escritas en cada receptor y las filas leídas de cada origen. Estos resultados se devuelven en la sección `output` del resultado de ejecución de la actividad. Las métricas devueltas están en el formato del siguiente código JSON.

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

Por ejemplo, para obtener el número de filas escritas en un receptor denominado "sink1" en una actividad denominada "dataflowActivity", use `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten`.

Para obtener el número de filas leídas de un origen denominado "source1" que se usó en ese receptor, use `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead`.

> [!NOTE]
> Si un receptor tiene cero filas escritas, no se mostrará en las métricas. Se puede comprobar la existencia con la función `contains`. Por ejemplo, `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')` comprobará si se escribieron filas en sink1.

## <a name="next-steps"></a>Pasos siguientes

Consulte las actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
