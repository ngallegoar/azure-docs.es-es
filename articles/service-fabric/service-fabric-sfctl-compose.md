---
title: 'CLI de Azure Service Fabric: compose de sfctl'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para aplicaciones Docker Compose.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: e84b35ff3dfa7be0c848a4d7b906c622d9b88914
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86257348"
---
# <a name="sfctl-compose"></a>sfctl compose
Cree, elimine y administre aplicaciones de Docker Compose.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| create | Crea una implementación de Compose de Service Fabric. |
| list | Obtiene la lista de implementaciones de Compose creadas en el clúster de Service Fabric. |
| remove | Elimina una implementación existente de Compose de Service Fabric del clúster. |
| status | Obtiene información sobre una implementación de Compose de Service Fabric. |
| upgrade | Inicia la actualización de una implementación de Compose en el clúster de Service Fabric. |
| upgrade-rollback | Inicia el proceso de reversión la actualización de una implementación de Compose en el clúster de Service Fabric. |
| upgrade-status | Obtiene detalles de la actualización más reciente realizada en esta implementación de Compose de Service Fabric. |

## <a name="sfctl-compose-create"></a>sfctl compose create
Crea una implementación de Compose de Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --deployment-name [Obligatorio] | Nombre de la implementación. |
| --file-path       [obligatorio] | Ruta de acceso al archivo de destino de Docker Compose. |
| --encrypted-pass | En lugar de solicitar una contraseña de registro de contenedor, utilice una frase de contraseña ya cifrada. |
| --has-pass | Solicitará una contraseña para el registro de contenedor. |
| --timeout -t | Valor predeterminado\: 60. |
| --user | Nombre de usuario para conectarse al registro de contenedor. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-compose-list"></a>sfctl compose list
Obtiene la lista de implementaciones de Compose creadas en el clúster de Service Fabric.

Obtiene el estado sobre las implementaciones de Compose que se crearon o están en proceso de creación en el clúster de Service Fabric. La respuesta incluye el nombre, el estado y otros detalles acerca de la implementación de Compose. Si la lista de implementaciones no cabe en una página, se devuelve una página de resultados, así como un token de continuación que puede usarse para obtener la página siguiente.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --max-results | El número máximo de resultados que se devuelven como parte de las consultas paginadas. Este parámetro define el límite superior en el número de resultados devueltos. Los resultados devueltos pueden ser menos que el número máximo de resultados especificado si no caben en el mensaje según las restricciones del tamaño máximo del mensaje definidas en la configuración. Si este parámetro es cero o no se especifica, la consulta paginada incluye tantos resultados como quepan en el mensaje devuelto. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-compose-remove"></a>sfctl compose remove
Elimina una implementación existente de Compose de Service Fabric del clúster.

Elimina una implementación existente de Compose de Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --deployment-name [Obligatorio] | La identidad de la implementación. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-compose-status"></a>sfctl compose status
Obtiene información sobre una implementación de Compose de Service Fabric.

Devuelve el estado de la implementación de Compose que se creó o está en proceso de creación en el clúster de Service Fabric y cuyo nombre coincide con el especificado como parámetro. La respuesta incluye el nombre, el estado y otros detalles acerca de la implementación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --deployment-name [Obligatorio] | La identidad de la implementación. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-compose-upgrade"></a>sfctl compose upgrade
Inicia la actualización de una implementación de Compose en el clúster de Service Fabric.

Valida los parámetros de actualización proporcionados e inicia la actualización de la implementación si los parámetros son válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --deployment-name [Obligatorio] | Nombre de la implementación. |
| --file-path [Obligatorio] | Ruta de acceso al archivo de destino de Docker Compose. |
| --default-svc-type-health-map | Diccionario JSON codificado que describe la directiva de estado que se usa para evaluar el estado de los servicios. |
| --encrypted-pass | En lugar de solicitar una contraseña de registro de contenedor, utilice una frase de contraseña ya cifrada. |
| --failure-action | Los valores posibles son\: "Invalid", "Rollback", "Manual". |
| --force-restart | Los procesos se reinician de forma forzosa durante la actualización incluso si no ha cambiado la versión del código. <br><br> La actualización solo cambia la configuración o los datos. |
| --has-pass | Solicitará una contraseña para el registro de contenedor. |
| --health-check-retry | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no funcionan correctamente. |
| --health-check-stable | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --health-check-wait | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento. |
| --replica-set-check | El período de tiempo máximo para bloquear el procesamiento de un dominio de actualización y evitar la pérdida de disponibilidad cuando hay problemas inesperados. <br><br> Cuando este tiempo de espera expire, el procesamiento del dominio de actualización se llevará a cabo independientemente de los problemas de pérdida de disponibilidad. El tiempo de espera se restablece al principio de cada dominio de actualización. Los valores válidos oscilan entre 0 y 42949672925, ambos inclusive. |
| --svc-type-health-map | Lista codificada en JSON de objetos que describen las directivas de estado que se usan para evaluar el estado de los distintos tipos de servicio. |
| --timeout -t | Valor predeterminado\: 60. |
| --unhealthy-app | El porcentaje máximo permitido de aplicaciones en mal estado antes de informar de un error. <br><br> Por ejemplo, para permitir el 10 % de las aplicaciones en mal estado, este valor sería 10. El valor representa el porcentaje máximo tolerado de aplicaciones que pueden ser incorrectas antes de que el clúster se considere erróneo. Si se respeta el porcentaje, pero hay al menos una aplicación en mal estado, el estado se evalúa como Warning. Se calcula dividiendo el número de aplicaciones en mal estado sobre el número total de instancias de aplicación en el clúster. |
| --upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de FailureAction. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --upgrade-kind | Valor predeterminado\: Rolling. |
| --upgrade-mode | Los valores posibles son\: "Invalid", "UnmonitoredAuto", "UnmonitoredManual", "Monitored".  Valor predeterminado\: UnmonitoredAuto. |
| --upgrade-timeout | El período de tiempo en el que se debe completar la actualización general antes de que se ejecute FailureAction. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --user | Nombre de usuario para conectarse al registro de contenedor. |
| --warning-as-error | Indica si las advertencias se tratan con el mismo nivel de gravedad que los errores. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-compose-upgrade-rollback"></a>sfctl compose upgrade-rollback
Inicia el proceso de reversión la actualización de una implementación de Compose en el clúster de Service Fabric.

Revierte una actualización de la implementación de Compose de Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --deployment-name [Obligatorio] | La identidad de la implementación. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl compose upgrade-status
Obtiene detalles de la actualización más reciente realizada en esta implementación de Compose de Service Fabric.

Devuelve la información sobre el estado de la actualización de la implementación de Compose junto con detalles para ayudar a depurar los problemas de mantenimiento de la aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --deployment-name [Obligatorio] | La identidad de la implementación. |
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
