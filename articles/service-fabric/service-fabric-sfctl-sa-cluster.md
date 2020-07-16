---
title: 'CLI de Azure Service Fabric: sfctl sa-cluster'
description: Obtenga información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar clústeres independientes.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 7e9f4b55945afc04e5c826b26632d68cb75e502f
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260276"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Administre clústeres de Service Fabric independientes.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| config | Obtenga la configuración del clúster independiente de Service Fabric. |
| config-upgrade | Comience a actualizar la configuración de un clúster independiente de Service Fabric. |
| upgrade-status | Obtenga el estado de actualización de configuración de clúster de un clúster de Service Fabric independiente. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
Obtenga la configuración del clúster independiente de Service Fabric.

La configuración de clúster contiene propiedades del clúster que incluyen distintos tipos de nodos en el clúster, las configuraciones de seguridad, los errores y las topologías de dominio de actualización, etc.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --configuration-api-version [obligatorio] | La versión de API de la configuración de json de clúster independiente. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Comience a actualizar la configuración de un clúster independiente de Service Fabric.

Valide los parámetros de actualización de configuración suministrados e inicie la actualización de la configuración de un clúster si los parámetros son válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --cluster-config            [obligatorio] | La configuración de clúster. |
| --application-health-policies | Diccionario codificado en JSON de pares de nombres de tipo de aplicación y porcentaje máximo con estado incorrecto antes de producirse el error. |
| --delta-unhealthy-nodes | El porcentaje máximo permitido de degradación de mantenimiento delta durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --health-check-retry | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no funcionan correctamente.  Valor predeterminado\: PT0H0M0S. |
| --health-check-stable | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización.  Valor predeterminado\: PT0H0M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --health-check-wait | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento.  Valor predeterminado\: PT0H0M0S. |
| --timeout -t | Valor predeterminado\: 60. |
| --unhealthy-applications | El porcentaje máximo permitido de aplicaciones en mal estado durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --unhealthy-nodes | El porcentaje máximo permitido de nodos en mal estado durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --upgrade-domain-delta-unhealthy-nodes | El porcentaje máximo permitido de degradación de mantenimiento delta de dominio de actualización durante la actualización. Los valores permitidos son enteros entre 0 y 100. |
| --upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de FailureAction.  Valor predeterminado\: PT0H0M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --upgrade-timeout | El período de tiempo en el que se debe completar la actualización general antes de que se ejecute FailureAction.  Valor predeterminado\: PT0H0M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

### <a name="examples"></a>Ejemplos

Iniciar una actualización de la configuración de clúster
```
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Obtenga el estado de actualización de configuración de clúster de un clúster de Service Fabric independiente.

Obtenga los detalles del estado de actualización de configuración de clúster de un clúster de Service Fabric independiente.

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


## <a name="next-steps"></a>Pasos siguientes
- [Configuración](service-fabric-cli.md) de la CLI de Service Fabric.
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](./scripts/sfctl-upgrade-application.md).
