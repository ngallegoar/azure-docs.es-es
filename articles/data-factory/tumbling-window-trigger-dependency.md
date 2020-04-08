---
title: Creación de dependencias en un desencadenador de ventana de saltos de tamaño constante
description: Aprenda a crear dependencia en un desencadenador de ventana de saltos de tamaño constante en Azure Data Factory.
services: data-factory
ms.author: daperlov
author: djpmsft
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/29/2019
ms.openlocfilehash: 0557c9b9eb65654c4a11c1389ace4776ab60a61d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532577"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Creación de una dependencia de un desencadenador de ventana de saltos de tamaño constante

En este artículo se exponen los pasos necesarios para crear una dependencia en un desencadenador de ventana de saltos de tamaño constante. Para obtener información general acerca de los desencadenadores de ventanas de saltos de tamaño constante, consulte [Cómo crear un desencadenador de ventana de saltos de tamaño constante](how-to-create-tumbling-window-trigger.md).

Con el fin de generar una cadena de dependencia y de asegurarse de que un desencadenador se ejecuta solo después de la correcta ejecución de otro desencadenador en la factoría de datos, utilice esta característica avanzada para crear una dependencia de ventana de saltos de tamaño constante.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Creación de una dependencia en la interfaz de usuario de Data Factory

Para crear una dependencia en un desencadenador, seleccione **Desencadenador > Avanzado > Nuevo**, y luego elija el desencadenador del que se va a depender con el desplazamiento y tamaño adecuados. Seleccione **Finalizar** y publique los cambios de la factoría de datos para que las dependencias surta efecto.

![Creación de dependencias](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png "Creación de dependencias")

## <a name="tumbling-window-dependency-properties"></a>Propiedades de dependencia de ventana de saltos de tamaño constante

Un desencadenador de ventana de saltos de tamaño constante con una dependencia tiene las siguientes propiedades:

```json
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": <<Started/Stopped/Disabled - readonly>>,
        "typeProperties": {
            "frequency": <<Minute/Hour>>,
            "interval": <<int>>,
            "startTime": <<datetime>>,
            "endTime": <<datetime – optional>>,
            "delay": <<timespan – optional>>,
            "maxConcurrency": <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                "intervalInSeconds": <<int>>,
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan – optional>>,
                    "referenceTrigger": {
                        "referenceName": "MyTumblingWindowDependency1",
                        "type": "TriggerReference"
                    }
                },
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan>>
                }
            ]
        }
    }
}
```

La tabla siguiente proporciona la lista de los atributos necesarios para definir una dependencia de ventana de saltos de tamaño constante.

| **Nombre de la propiedad** | **Descripción**  | **Tipo** | **Obligatorio** |
|---|---|---|---|
| type  | Todos los desencadenadores de ventana de saltos de tamaño constante existentes se muestran en este menú desplegable. Elija el desencadenador del que se va a depender.  | TumblingWindowTriggerDependencyReference o SelfDependencyTumblingWindowTriggerReference. | Sí |
| offset | Desplazamiento del desencadenador de dependencia. Proporcione un valor en formato de intervalo de tiempo; admite desplazamientos negativos y positivos. Esta propiedad es obligatoria si el desencadenador depende de sí mismo; en todos los demás casos, es opcional. La autodependencia debe tener siempre un valor de desplazamiento negativo. Si no se especifica ningún valor, la ventana es igual al desencadenador. | TimeSpan<br/>(hh:mm:ss) | Autodependencia: Sí<br/>Otros: No |
| tamaño | Tamaño de la ventana de saltos de tamaño constante de dependencia. Proporcione un valor de intervalo de tiempo positivo. Esta propiedad es opcional. | TimeSpan<br/>(hh:mm:ss) | No  |

> [!NOTE]
> Un desencadenador de ventana de saltos de tamaño constante puede depender de un máximo de otros dos desencadenadores.

## <a name="tumbling-window-self-dependency-properties"></a>Propiedades de la autodependencia de ventana de saltos de tamaño constante

En los escenarios en los que el desencadenador no debe continuar a la siguiente ventana hasta que se haya completado correctamente la ventana anterior, cree una autodependencia. Un desencadenador de dependencia automática que depende del éxito de las ejecuciones anteriores durante la hora anterior tendrá las propiedades siguientes:

```json
{
    "name": "DemoSelfDependency",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00Z",
            "delay": "00:01:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": "01:00:00",
                    "offset": "-01:00:00"
                }
            ]
        }
    }
}
```
## <a name="usage-scenarios-and-examples"></a>Escenarios de uso y ejemplos

A continuación se muestran las ilustraciones de los escenarios y el uso de las propiedades de dependencia de ventana de saltos de tamaño constante.

### <a name="dependency-offset"></a>Desplazamiento de la dependencia

![Ejemplo de desplazamiento](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png "Ejemplo de desplazamiento")

### <a name="dependency-size"></a>Tamaño de la dependencia

![Ejemplo de tamaño](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png "Ejemplo de tamaño")

### <a name="self-dependency"></a>Autodependencia

![Autodependencia](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png "Autodependencia")

### <a name="dependency-on-another-tumbling-window-trigger"></a>Dependencia de otro desencadenador de ventana de saltos de tamaño constante

Un trabajo de procesamiento de datos de telemetría diario que dependa de otro trabajo diario que agregue la salida de los últimos siete días de salida y genere secuencias de ventana gradual de siete días:

![Ejemplo de dependencia](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png "Ejemplo de dependencia")

### <a name="dependency-on-itself"></a>Dependencia de sí mismo

Un trabajo diario sin interrupciones en los flujos de salida del trabajo:

![Ejemplo de autodependencia](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png "Ejemplo de autodependencia")

Puede ver una demostración de cómo crear canalizaciones dependientes en Azure Data Factory mediante el desencadenador de ventanas de saltos de tamaño constante en el vídeo siguiente:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Create-dependent-pipelines-in-your-Azure-Data-Factory/player]

## <a name="monitor-dependencies"></a>Supervisión de dependencias

Puede supervisar la cadena de dependencias y las ventanas correspondientes desde la página de supervisión de ejecución del desencadenador. Vaya a  **Supervisión > Trigger Runs (Ejecuciones de desencadenador)** . En la columna de acciones, puede volver a ejecutar el desencadenador o ver sus dependencias.

![Supervisión de las ejecuciones del desencadenador](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png "Supervisión de las ejecuciones del desencadenador")

Si hace clic en "View Trigger Dependencies" (Ver dependencias de desencadenador), puede ver el estado de las dependencias. Si se produce un error en uno de los desencadenadores de dependencia, debe volver a ejecutarlo correctamente para que se ejecute el desencadenador dependiente. Un desencadenador de ventana de saltos de tamaño constante esperará las dependencias durante siete días antes de que se agote el tiempo de espera.

![Supervisión de dependencias](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png "Supervisión de dependencias")

Para ver una vista más clara de la programación de dependencia del desencadenador, seleccione la vista Gantt.

![Supervisión de dependencias](media/tumbling-window-trigger-dependency/tumbling-window-dependency09.png "Supervisión de dependencias")

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Cómo crear un desencadenador de ventana de saltos de tamaño constante](how-to-create-tumbling-window-trigger.md)
