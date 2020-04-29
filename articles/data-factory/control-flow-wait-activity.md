---
title: Actividad Wait en Azure Data Factory
description: La actividad Wait pausa la ejecución de la canalización para el período especificado.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/12/2018
ms.openlocfilehash: e6158938d01b6e5da74ed046d2a74e0dfd827f47
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417919"
---
# <a name="execute-wait-activity-in-azure-data-factory"></a>Ejecutar la actividad Wait en Azure Data Factory
Cuando use una actividad Wait en una canalización, esta espera durante el período de tiempo especificado antes de continuar con la ejecución de actividades sucesivas. 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]


## <a name="syntax"></a>Sintaxis

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
name | Nombre de la actividad `Wait`. | String | Sí
type | Debe establecerse en **Wait**. | String | Sí
waitTimeInSeconds | El número de segundos que esperará la canalización antes de seguir con el procesamiento. | Entero | Sí

## <a name="example"></a>Ejemplo

> [!NOTE]
> En esta sección se proporcionan definiciones JSON y comandos de PowerShell de ejemplo para ejecutarlos en la canalización. Para ver una guía con instrucciones paso a paso para crear una canalización de Data Factory con definiciones de JSON y Azure PowerShell, consulte [Tutorial: Creación de una factoría de datos y una canalización con PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Canalización con actividad Wait
En este ejemplo, la canalización tiene dos actividades: **Until** y **Wait**. La actividad Wait está configurada para que espere durante un segundo. La canalización ejecuta la actividad Web en un bucle con un tiempo de espera de un segundo entre cada ejecución. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
    }
}

```

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
