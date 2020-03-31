---
title: 'CLI de Azure Service Fabric: Implementación de sfctl mesh'
description: Obtenga más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para crear recursos de Service Fabric Mesh.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 108389407221779ed20e81310f084b7b5c23b8c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906024"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Crea recursos de Service Fabric Mesh.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| create | Crea una implementación de recursos de Service Fabric Mesh. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl mesh deployment create
Crea una implementación de recursos de Service Fabric Mesh.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --input-yaml-files [obligatorio] | Rutas de acceso relativas o absolutas separadas por comas de todos los archivos yaml o ruta de acceso relativa o absoluta del directorio (recursivo) que contiene los archivos yaml. |
| --parameters | Ruta de acceso relativa o absoluta de un archivo yaml o un objeto json que contiene los parámetros que deben invalidarse. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

### <a name="examples"></a>Ejemplos

Consolida e implementa todos los recursos en el clúster mediante la invalidación de los parámetros mencionados en el archivo yaml.
``` 
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters  
./param.yaml    
```

Consolida e implementa todos los recursos en un directorio mediante la invalidación de los parámetros mencionados en el archivo yaml.

``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Consolida e implementa todos los recursos de un directorio en el clúster mediante la invalidación de los parámetros que se pasan directamente como objeto json.
``` 
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :    
{'value' : 'my_value'} }"   
```

## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).
