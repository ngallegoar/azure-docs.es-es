---
title: 'CLI de Azure Service Fabric: almacén de sfctl'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para realizar operaciones de nivel de archivo en el almacén de imágenes del clúster.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: af5399be44d2946775622598c5c1db3d8355f7cd
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86257077"
---
# <a name="sfctl-store"></a>almacén de sfctl
Realice operaciones básicas a nivel de archivo en el almacén de imágenes del clúster.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| delete | Elimina el contenido existente en el almacén de imágenes. |
| root-info | Obtiene la información de contenido en la raíz del almacén de imágenes. |
| stat | Obtiene la información de contenido del almacén de imágenes. |

## <a name="sfctl-store-delete"></a>sfctl store delete
Elimina el contenido existente en el almacén de imágenes.

Elimina el contenido existente del almacén de imágenes que se encuentra en la ruta de acceso relativa especificada. Este comando se puede usar para eliminar los paquetes de aplicación cargados una vez aprovisionados.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --content-path [Obligatorio] | Ruta de acceso relativa al archivo o la carpeta del almacén de imágenes desde su raíz. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-store-root-info"></a>sfctl store root-info
Obtiene la información de contenido en la raíz del almacén de imágenes.

Devuelve la información sobre el contenido del almacén de imágenes en la raíz de dicho almacén.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-store-stat"></a>sfctl store stat
Obtiene la información de contenido del almacén de imágenes.

Devuelve la información sobre el contenido del almacén de imágenes en la instancia de contentPath especificada. El valor de contentPath es relativo a la raíz del almacén de imágenes.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --content-path [Obligatorio] | Ruta de acceso relativa al archivo o la carpeta del almacén de imágenes desde su raíz. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

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
