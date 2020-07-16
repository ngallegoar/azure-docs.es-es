---
title: Azure Service Fabric CLI- sfctl rpm
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para el servicio del administrador de reparaciones.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 7317fd66303aaabf5232106aa7391439880bebaf
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260285"
---
# <a name="sfctl-rpm"></a>rpm de sfctl
Consulte y envíe comandos al servicio del administrador de reparaciones.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| approve-force | Fuerza la aprobación de la tarea de reparación determinada. |
| delete | Elimina una tarea de reparación completada. |
| list | Obtiene una lista de tareas de reparación que coinciden con los filtros especificados. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm approve-force
Fuerza la aprobación de la tarea de reparación determinada.

Esta API es compatible con la plataforma Service Fabric; no está diseñada para utilizarse directamente desde el código.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --task-id [Obligatorio] | El identificador de la tarea de reparación. |
| --version | El número de versión actual de la tarea de reparación. Si es distinto de cero, la solicitud se realizará correctamente solo si este valor coincide con la versión actual real de la tarea de reparación. Si es cero, no se realiza ninguna comprobación de la versión. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-rpm-delete"></a>sfctl rpm delete
Elimina una tarea de reparación completada.

Esta API es compatible con la plataforma Service Fabric; no está diseñada para utilizarse directamente desde el código.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --task-id [Obligatorio] | El identificador de la tarea de reparación completada que se va a eliminar. |
| --version | El número de versión actual de la tarea de reparación. Si es distinto de cero, la solicitud se realizará correctamente solo si este valor coincide con la versión actual real de la tarea de reparación. Si es cero, no se realiza ninguna comprobación de la versión. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-rpm-list"></a>sfctl rpm list
Obtiene una lista de tareas de reparación que coinciden con los filtros especificados.

Esta API es compatible con la plataforma Service Fabric; no está diseñada para utilizarse directamente desde el código.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --executor-filter | El nombre del ejecutar de reparación cuyas tareas notificadas deben incluirse en la lista. |
| --state-filter | Una operación OR bit a bit de los siguientes valores, especificando qué estados de tareas deben incluirse en la lista de resultados. <ul><li>1\. Creado</li><li>2\. Notificado</li><li>4\. Preparando</li><li>8\. Aprobado</li><li>16. Ejecutando</li><li>32. Restaurando</li><li>64. Completado</li></ul>
| --task-id-filter | El prefijo del identificador de tarea de reparación que debe coincidir. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](./scripts/sfctl-upgrade-application.md).
