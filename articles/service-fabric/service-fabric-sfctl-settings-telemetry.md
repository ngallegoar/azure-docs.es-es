---
title: 'CLI de Azure Service Fabric: sfctl settings telemetry'
description: Obtenga información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para configurar la telemetría de sfctl.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 6af5fa944ef399756f9e890ddd77a7f5f32e2bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76903014"
---
# <a name="sfctl-settings-telemetry"></a>sfctl settings telemetry
Configura los valores de telemetría locales para esta instancia de sfctl.

La telemetría de sfctl recopila el nombre del comando sin que se proporcionen los parámetros ni sus valores, la versión de sfctl, el tipo de sistema operativo, la versión de Python, el éxito o error del comando o el mensaje de error devuelto.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| set-telemetry | Activar o desactivar la telemetría. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>sfctl settings telemetry set-telemetry
Activar o desactivar la telemetría.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --off | Desactivar la telemetría. |
| --on | Activar la telemetría. Este es el valor predeterminado. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

### <a name="examples"></a>Ejemplos

Desactivar la telemetría.

```
sfctl settings telemetry set_telemetry --off
```

Activar la telemetría.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).