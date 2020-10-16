---
title: 'CLI de Azure Service Fabric: réplica sfctl'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar réplicas.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 3ed1ecca686471e052f07e945738ad1a49e68464
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86260298"
---
# <a name="sfctl-replica"></a>réplica de sfctl
Administre las réplicas que pertenecen a las particiones del servicio.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| deployed | Obtiene los detalles de la réplica implementada en un nodo de Service Fabric. |
| deployed-list | Obtiene la lista de réplicas implementadas en un nodo de Service Fabric. |
| health | Obtiene el mantenimiento de una réplica de servicio con estado o de una instancia de servicio sin estado de Service Fabric. |
| info | Obtiene la información sobre una réplica de una partición de Service Fabric. |
| list | Obtiene la información sobre las réplicas de una partición de servicio de Service Fabric. |
| remove | Quita una réplica de servicio que se ejecuta en un nodo. |
| report-health | Envía un informe de mantenimiento sobre la réplica de Service Fabric. |
| restart | Reinicia una réplica de servicio de un servicio persistente que se ejecuta en un nodo. |

## <a name="sfctl-replica-deployed"></a>sfctl replica deployed
Obtiene los detalles de la réplica implementada en un nodo de Service Fabric.

Obtiene los detalles de la réplica implementada en un nodo de Service Fabric. La información incluye el tipo de servicio, el nombre del servicio, la operación de servicio actual, la fecha y hora de inicio de la operación de servicio actual, el identificador de partición, el identificador de réplica/instancia, la carga notificada y otra información.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --replica-id [Obligatorio] | El identificador de la réplica. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-deployed-list"></a>sfctl replica deployed-list
Obtiene la lista de réplicas implementadas en un nodo de Service Fabric.

Obtiene la lista que contiene la información sobre las réplicas implementadas implementada en un nodo de Service Fabric. La información incluye el identificador de la partición, el identificador de la réplica, el estado de la réplica, el nombre del servicio, el nombre del tipo de servicio y otra información. Utilice los parámetros de consulta PartitionId o ServiceManifestName para obtener información sobre las réplicas implementadas que coincidan con los valores especificados para esos parámetros.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --partition-id | La identidad de la partición. |
| --service-manifest-name | El nombre de un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-health"></a>sfctl replica health
Obtiene el mantenimiento de una réplica de servicio con estado o de una instancia de servicio sin estado de Service Fabric.

Obtiene el estado de una réplica de Service Fabric. Use EventsHealthStateFilter para filtrar la colección de eventos de estado notificados en la réplica en función del estado de mantenimiento.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --replica-id [Obligatorio] | El identificador de la réplica. |
| --events-health-state-filter | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-info"></a>sfctl replica info
Obtiene la información sobre una réplica de una partición de Service Fabric.

La respuesta incluye el identificador, el rol, el estado, el estado, el nombre del nodo, el tiempo de actividad y otros detalles acerca de la réplica.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --replica-id [Obligatorio] | El identificador de la réplica. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-list"></a>sfctl replica list
Obtiene la información sobre las réplicas de una partición de servicio de Service Fabric.

El punto de conexión de GetReplicas devuelve información sobre las réplicas de la partición especificada. La respuesta incluye el identificador, el rol, el estado, el estado, el nombre del nodo, el tiempo de actividad y otros detalles acerca de la réplica.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-remove"></a>sfctl replica remove
Quita una réplica de servicio que se ejecuta en un nodo.

Esta API simula un error de réplica de Service Fabric mediante la eliminación de una réplica de un clúster de Service Fabric. La eliminación cierra la réplica, realiza su transición al rol None y elimina del clúster toda la información sobre la réplica. Esta API comprueba la ruta de acceso de eliminación del estado de la réplica y simula la ruta de acceso permanente del error notificado a través de las API de cliente. Advertencia: No se realizan comprobaciones de seguridad cuando se utiliza esta API. El uso incorrecto de esta API puede conllevar la pérdida de datos de los servicios con estado. Además, la marca forceRemove afecta a todas las demás réplicas hospedadas en el mismo proceso.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --replica-id [Obligatorio] | El identificador de la réplica. |
| --force-remove | Elimina una aplicación o un servicio de Service Fabric de manera forzada sin pasar por la secuencia de apagado correcta. Este parámetro puede usarse para forzar la eliminación de una aplicación o un servicio cuya eliminación normal requiere un tiempo de espera a causa de problemas del código del servicio que impiden el cierre correcto de las réplicas. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-report-health"></a>sfctl replica report-health
Envía un informe de mantenimiento sobre la réplica de Service Fabric.

Informa del estado de mantenimiento de la réplica de Service Fabric especificada. El informe debe contener la información sobre el origen del informe de mantenimiento y la propiedad sobre la cual se informa. El informe se envía a una réplica de puerta de enlace de Service Fabric, que lo reenvía al almacén de estado. La puerta de enlace puede aceptar el informe, pero el almacén de estado puede rechazarlo después de una validación adicional. Por ejemplo, el almacén de estado puede rechazar el informe debido a un parámetro no válido, como un número de secuencia obsoleto. Para ver si el informe se ha aplicado en el almacén de estado, ejecute get replica health y compruebe que aparece en la sección HealthEvents.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --health-property [Obligatorio] | La propiedad de la información de mantenimiento. <br><br> Una entidad puede tener informes de mantenimiento para diferentes propiedades. La propiedad es una cadena y no una enumeración fija para permitir al informador la flexibilidad de categorizar la condición de estado que desencadena el informe. Por ejemplo, un informador con SourceId "LocalWatchdog" puede supervisar el estado del disco disponible en un nodo, por lo que puede informar de la propiedad "AvailableDisk" en ese nodo. El mismo informador puede supervisar la conectividad del nodo, por lo que puede informar de una propiedad "Connectivity" en el mismo nodo. En el almacén de estado, estos informes se tratan como eventos de mantenimiento independientes del nodo especificado. Junto con SourceId, la propiedad identifica la información de mantenimiento de manera única. |
| --health-state    [Obligatorio] | Los valores posibles incluyen\: 'Invalid', 'Ok', 'Warning', 'Error', 'Unknown'. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --replica-id [Obligatorio] | La identidad de la partición. |
| --source-id       [Obligatorio] | El nombre de origen que identifica el componente de cliente/guardián/sistema que genera la información de estado. |
| --description | La descripción de la información de mantenimiento. <br><br> Representa el texto libre utilizado para agregar información legible por el ser humano sobre el informe. La longitud máxima de la cadena de la descripción es de 4096 caracteres. Si la cadena proporcionada es más larga, se truncará automáticamente. Cuando está truncado, los últimos caracteres de la descripción contienen un marcador "[Truncado]" y el tamaño total de la cadena es de 4096 caracteres. La presencia del marcador indica a los usuarios que el truncamiento se ha producido. Tenga en cuenta que, cuando se trunca, la descripción tiene menos de 4096 caracteres de la cadena original. |
| --immediate | Una marca que indica si el informe se debe enviar inmediatamente. <br><br> Se envía un informe de mantenimiento a una aplicación de puerta de enlace de Service Fabric, que la reenvía al almacén de estado. Si Immediate se establece en true, el informe se envía inmediatamente de la puerta de enlace HTTP al almacén de estado, independientemente de la configuración de cliente de Fabric que esté usando la aplicación de la puerta de enlace HTTP. Esto es útil para los informes críticos que deben enviarse tan pronto como sea posible. En función del tiempo y otras condiciones, el envío del informe puede aún generar un error, por ejemplo, si se cierra la puerta de enlace HTTP o el mensaje no llega a la puerta de enlace. Si Immediate se establece en false, el informe se envía en función de la configuración de cliente de mantenimiento de la puerta de enlace HTTP. Por lo tanto, se procesará por lotes de acuerdo con la configuración de HealthReportSendInterval. Esta es la configuración recomendada porque permite que el cliente de mantenimiento optimice los mensajes de notificación de estado destinados al almacén de estado, así como el procesamiento de informes de mantenimiento. De forma predeterminada, los informes no se envían inmediatamente. |
| --remove-when-expired | Valor que indica si el informe se quita del almacén de estado cuando expire. <br><br> Si se establece en true, el informe se quita del almacén de estado una vez que expire. Si se establece en false, el informe se trata como un error cuando expire. El valor de esta propiedad es false de forma predeterminada. Cuando los clientes notifican periódicamente, deben establecer RemoveWhenExpired en false (valor predeterminado). De esta manera, si el notificador tiene problemas (por ejemplo, un interbloqueo) y no puede informar, la entidad se evalúa al llegar el error cuando expira el informe de mantenimiento. De este modo, se marca que la entidad está en estado de mantenimiento Error. |
| --sequence-number | El número de secuencia para este informe de estado como una cadena numérica. <br><br> El número de secuencia del informe se usa por el almacén de estado para detectar informes obsoletos. Si no se especifica, se genera automáticamente un número de secuencia por el cliente de estado cuando se agrega un informe. |
| --service-kind | El tipo de réplica de servicio (con o sin estado) para el que se va a notificar el mantenimiento. Los valores posibles son los siguientes\: "Stateless", "Stateful".  Valor predeterminado\: Stateful. |
| --timeout -t | Valor predeterminado\: 60. |
| --ttl | La duración durante la cual este informe de mantenimiento es válido. Este campo usa el formato ISO8601 para especificar la duración. <br><br> Cuando los clientes notifican periódicamente, deben enviar informes con una frecuencia mayor que el período de vida. Si los clientes notifican en transición, pueden establecer el período de vida en Infinito. Cuando expira el período de vida, el evento de estado que contiene la información de estado se quita del almacén de estado, si RemoveWhenExpired es true, o se evalúa en el error, si RemoveWhenExpired es false. Si no se especifica, el período de vida se establece de forma predeterminada en un valor infinito. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-replica-restart"></a>sfctl replica restart
Reinicia una réplica de servicio de un servicio persistente que se ejecuta en un nodo.

Reinicia una réplica de servicio de un servicio persistente que se ejecuta en un nodo. Advertencia: No se realizan comprobaciones de seguridad cuando se utiliza esta API. El uso incorrecto de esta API puede conllevar la pérdida de disponibilidad de los servicios con estado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --replica-id [Obligatorio] | El identificador de la réplica. |
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
