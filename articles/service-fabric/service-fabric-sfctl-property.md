---
title: 'CLI de Azure Service Fabric: sfctl property'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para almacenar y consultar propiedades.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 0a5ebd4822c5f0ff1735464bb4d5b42c436ee529
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86260326"
---
# <a name="sfctl-property"></a>sfctl property
Almacene y consulte las propiedades con nombres de Service Fabric.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| delete | Elimina la propiedad de Service Fabric especificada. |
| get | Obtiene la propiedad de Service Fabric especificada. |
| list | Obtiene información sobre todas las propiedades de Service Fabric con un nombre concreto. |
| put | Crea o actualiza una propiedad de Service Fabric. |

## <a name="sfctl-property-delete"></a>sfctl property delete
Elimina la propiedad de Service Fabric especificada.

Elimina la propiedad de Service Fabric especificada con un nombre concreto. Es necesario haber creado una propiedad para eliminarla.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name-id       [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --property-name [obligatorio] | Especifica el nombre de la propiedad que se va a obtener. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-property-get"></a>sfctl property get
Obtiene la propiedad de Service Fabric especificada.

Obtiene la propiedad de Service Fabric especificada con un nombre concreto. Esto devolverá siempre valor y metadatos.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name-id       [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --property-name [obligatorio] | Especifica el nombre de la propiedad que se va a obtener. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-property-list"></a>sfctl property list
Obtiene información sobre todas las propiedades de Service Fabric con un nombre concreto.

Un nombre de Service Fabric puede tener una o varias propiedades con nombre que almacenan información personalizada. Esta operación obtiene la información acerca de estas propiedades en una lista paginada. La información incluye nombre, valor y metadatos sobre cada una de las propiedades.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name-id [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --include-values | Permite especificar si se deben incluir los valores de las propiedades devueltas. True si se deben devolver valores con los metadatos; False para devolver solo los metadatos de propiedad. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-property-put"></a>sfctl property put
Crea o actualiza una propiedad de Service Fabric.

Crea o actualiza la propiedad de Service Fabric especificada con un nombre concreto.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --name-id       [obligatorio] | Nombre de Service Fabric, sin el esquema de URI "fabric\:". |
| --property-name [obligatorio] | El nombre de la propiedad de Service Fabric. |
| --value         [obligatorio] | Describe un valor de propiedad de Service Fabric. Se trata de una cadena JSON. <br><br> La cadena JSON tiene dos campos: "Kind", el tipo de los datos y "Data", el valor de los datos. El valor de "tipo" debe ser el primer elemento en aparecer en la cadena JSON, y puede ser "Binary", "Int64", "Double", "String" o "Guid". El valor debe poder compatible con la serialización para los tipos especificados. Los valores de "tipo" y "datos" deben proporcionarse como cadenas. |
| --custom-id-type | El identificador del tipo personalizado de la propiedad. Con esta propiedad, el usuario es capaz de etiquetar el tipo del valor de la propiedad. |
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
