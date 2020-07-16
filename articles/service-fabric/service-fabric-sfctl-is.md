---
title: 'CLI de Service Fabric de Azure: sfctl is'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar la infraestructura.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 349f70c32ea4ebb4559f053d5ef05b4b37b6480f
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260379"
---
# <a name="sfctl-is"></a>sfctl is
Consulte y envíe comandos al servicio de infraestructura.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| command | Invoca un comando administrativo en la instancia del servicio de infraestructura determinada. |
| Query | Invoca una consulta de solo lectura en la instancia del servicio de infraestructura determinada. |

## <a name="sfctl-is-command"></a>sfctl is command
Invoca un comando administrativo en la instancia del servicio de infraestructura determinada.

Para los clústeres que tienen una o varias instancias del servicio de infraestructura configurado, esta API proporciona una manera de enviar comandos específicos de la infraestructura a una instancia determinada del servicio de infraestructura. Los comandos disponibles y sus formatos de respuesta correspondientes variarán en función de la infraestructura en la que se ejecute el clúster. Esta API es compatible con la plataforma Service Fabric; no está diseñada para utilizarse directamente desde el código.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --command [Obligatorio] | El texto del comando que se va a invocar. El contenido del comando es específico de la infraestructura. |
| --service-id | La identidad del servicio de infraestructura. <br><br> Este es el nombre completo del servicio de infraestructura sin el esquema de URI "fabric\:". Este parámetro solo es obligatorio para el clúster que tiene en ejecución más de una instancia del servicio de infraestructura. |
| --timeout -t | Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-is-query"></a>sfctl is query
Invoca una consulta de solo lectura en la instancia del servicio de infraestructura determinada.

Para los clústeres que tienen una o varias instancias del servicio de infraestructura configurado, esta API proporciona una manera de enviar consultas específicas de la infraestructura a una instancia determinada del servicio de infraestructura. Los comandos disponibles y sus formatos de respuesta correspondientes variarán en función de la infraestructura en la que se ejecute el clúster. Esta API es compatible con la plataforma Service Fabric; no está diseñada para utilizarse directamente desde el código.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --command [Obligatorio] | El texto del comando que se va a invocar. El contenido del comando es específico de la infraestructura. |
| --service-id | La identidad del servicio de infraestructura. <br><br> Este es el nombre completo del servicio de infraestructura sin el esquema de URI "fabric\:". Este parámetro solo es obligatorio para el clúster que tiene en ejecución más de una instancia del servicio de infraestructura. |
| --timeout -t | Valor predeterminado\: 60. |

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
