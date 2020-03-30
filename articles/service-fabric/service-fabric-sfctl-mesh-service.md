---
title: 'CLI de Azure Service Fabric: sfctl mesh service'
description: Obtenga información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para obtener los detalles de servicio de los recursos de una aplicación.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 2b9f16062b02d7a1a3403d396643a7989b8976b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905930"
---
# <a name="sfctl-mesh-service"></a>sfctl mesh service
Obtiene los detalles del servicio y muestra los servicios de un recurso de aplicación.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| list | Enumera todos los recursos de servicio. |
| show | Obtiene el recurso de servicio con el nombre especificado. |

## <a name="sfctl-mesh-service-list"></a>sfctl mesh service list
Enumera todos los recursos de servicio.

Obtiene la información sobre todos los servicios de un recurso de aplicación. La información incluye la descripción y otras propiedades del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --app-name: nombre de la aplicación [obligatorio] | Nombre de la aplicación. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-mesh-service-show"></a>sfctl mesh service show
Obtiene el recurso de servicio con el nombre especificado.

Obtiene la información sobre el recurso de servicio con el nombre especificado. La información incluye la descripción y otras propiedades del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --app-name: nombre de la aplicación [obligatorio] | Nombre de la aplicación. |
| --name -n [obligatorio] | El nombre del servicio. |

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