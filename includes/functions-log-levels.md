---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/04/2020
ms.author: glenga
ms.openlocfilehash: 72588ccfd9ad00305cbdeaf8132999731d86b9b1
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92164933"
---
Se asigna un *nivel de registro* a cada registro. El valor es un número entero que indica la importancia relativa:

|LogLevel    |Código| Descripción |
|------------|---|--------------|
|Seguimiento       | 0 |Registros que contienen los mensajes más detallados. Estos mensajes pueden contener datos confidenciales de la aplicación. Estos mensajes están deshabilitados de forma predeterminada y nunca se deben habilitar en un entorno de producción.|
|Depurar       | 1 | Registros que se usan para la investigación interactiva durante el desarrollo. Estos registros deben contener principalmente información útil para la depuración y no tienen ningún valor a largo plazo. |
|Información | 2 | Registros que realizan el seguimiento del flujo general de la aplicación. Estos registros deben tener un valor a largo plazo. |
|Advertencia     | 3 | Registros que resaltan un evento anormal o inesperado en el flujo de la aplicación, pero que no hacen que se detenga la ejecución de la aplicación. |
|Error       | 4 | Registros que resaltan en qué momento se detiene el flujo de ejecución actual debido a un error. Estos errores deben indicar un error en la actividad actual, no un error en toda la aplicación. |
|Crítico    | 5 | Registros que describen una aplicación no recuperable o un bloqueo del sistema, o bien un error catastrófico que requiere atención inmediata. |
|None        | 6 | Deshabilita el registro de la categoría especificada. |

La configuración del archivo [*host.json*](../articles/azure-functions/functions-host-json.md) determina la cantidad de registro que envía una aplicación de Functions a Application Insights.  
