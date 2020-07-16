---
title: 'CLI de Azure Service Fabric: sfctl mesh code-package-log'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para obtener registros para un paquete de código especificado.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 9ac1d85a1a498f9f6fcd0a03f8f819d1cdfcac33
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86257307"
---
# <a name="sfctl-mesh-code-package-log"></a>sfctl mesh code-package-log
Obtiene los registros para el contenedor del paquete de código especificado para la réplica de servicio dada.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| get | Obtiene los registros del contenedor. |

## <a name="sfctl-mesh-code-package-log-get"></a>sfctl mesh code-package-log get
Obtiene los registros del contenedor.

Obtiene los registros del contenedor del paquete de código especificado de la réplica de servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --app-name: nombre de la aplicación [obligatorio] | Nombre de la aplicación. |
| --code-package-name [obligatorio] | El nombre del paquete de código del servicio. |
| --réplica-name [obligatorio] | Nombre de la réplica de Service Fabric. |
| --service-name [obligatorio] | El nombre del servicio. |
| --tail | Número de líneas para mostrar desde el final de los registros. El valor predeterminado es 100. "all" para mostrar los registros completos. |

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
