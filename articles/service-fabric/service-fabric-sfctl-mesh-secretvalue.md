---
title: 'CLI de Azure Service Fabric: sfctl mesh secretvalue'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para obtener y eliminar los recursos de Service Fabric Mesh secretvalue.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: b2fe3a7995cdd88a8f63d0d865e72e9f161cf9d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905961"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Obtiene y elimina recursos de mesh secretvalue.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| delete | Elimina el valor especificado del recurso de secreto con nombre. |
| list | Enumera los nombres de todos los valores del recurso de secreto especificado. |
| show | Enumera el valor especificado del recurso de secreto. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue delete
Elimina el valor especificado del recurso de secreto con nombre.

Elimina el recurso del valor de secreto identificado por el nombre. El nombre del recurso suele ser la versión asociada con ese valor. La eliminación no se producirá si el valor especificado está en uso.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --secret-name -n [obligatorio] | Nombre del recurso de secreto. |
| --version -v     [obligatorio] | Nombre de la versión del secreto. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue list
Enumera los nombres de todos los valores del recurso de secreto especificado.

Obtiene información sobre todos los recursos de valor de secreto del recurso de secreto especificado. La información incluye los nombres de los recursos de valor de secreto, pero no los valores reales.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --secret-name -n [obligatorio] | Nombre del recurso de secreto. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue show
Enumera el valor especificado del recurso de secreto.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --secret-name -n [obligatorio] | Nombre del recurso de secreto. |
| --version -v     [obligatorio] | Nombre de la versión del secreto. |
| --show-value | Muestra el valor real de la versión de secreto. |

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
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).