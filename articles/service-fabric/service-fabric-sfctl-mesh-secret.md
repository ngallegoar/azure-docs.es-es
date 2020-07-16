---
title: 'CLI de Azure Service Fabric: sfctl mesh secret'
description: Obtenga información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para obtener y eliminar los recursos de secreto de Service Fabric Mesh.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 2c91a59e57613934c946e3cd56089b330893a51a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260345"
---
# <a name="sfctl-mesh-secret"></a>sfctl mesh secret
Obtiene y elimina recursos de mesh secret.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| delete | Elimina el recurso de secreto. |
| list | Enumera todos los recursos de secreto. |
| show | Obtiene el secreto de recurso con el nombre especificado. |

## <a name="sfctl-mesh-secret-delete"></a>sfctl mesh secret delete
Elimina el recurso de secreto.

Elimina el recurso de secreto especificado y todos sus valores con nombre.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name -n [Obligatorio] | Nombre del recurso de secreto. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-secret-list"></a>sfctl mesh secret list
Enumera todos los recursos de secreto.

Obtiene la información sobre todos los recursos de secreto de un grupo de recursos determinado. La información incluye la descripción y otras propiedades del secreto.

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-secret-show"></a>sfctl mesh secret show
Obtiene el secreto de recurso con el nombre especificado.

Obtiene la información sobre el recurso de secreto con el nombre especificado. La información incluye la descripción y otras propiedades del secreto.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name -n [Obligatorio] | Nombre del recurso de secreto. |

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
