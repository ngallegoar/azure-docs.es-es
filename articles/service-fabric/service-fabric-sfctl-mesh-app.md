---
title: 'CLI de Service Fabric de Azure: aplicación de sfctl mesh'
description: Obtenga información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar los recursos de la aplicación de Service Fabric mesh.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 7c571136e6bef64c2024c81db85ef736f64a4e01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906062"
---
# <a name="sfctl-mesh-app"></a>sfctl mesh app
Obtiene y elimina recursos de la aplicación.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| delete | Elimina el recurso de aplicación. |
| list | Enumera todos los recursos de aplicación. |
| show | Obtiene el recurso de aplicación con el nombre especificado. |

## <a name="sfctl-mesh-app-delete"></a>sfctl mesh app delete
Elimina el recurso de aplicación.

Elimina el recurso de aplicación identificado por el nombre.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name -n [Obligatorio] | Nombre de la aplicación. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-app-list"></a>sfctl mesh app list
Enumera todos los recursos de aplicación.

Obtiene la información sobre todos los recursos de aplicación de un grupo de recursos determinado. La información incluye la descripción y otras propiedades de la aplicación.

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-app-show"></a>sfctl mesh app show
Obtiene el recurso de aplicación con el nombre especificado.

Obtiene la información sobre el recurso de aplicación con el nombre especificado. La información incluye la descripción y otras propiedades de la aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name -n [Obligatorio] | Nombre de la aplicación. |

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