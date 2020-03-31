---
title: 'CLI de Azure Service Fabric: sfctl node'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar nodos de clúster.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 5881e6485003abd4fd23a7f6d06a428e768c00fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905880"
---
# <a name="sfctl-node"></a>sfctl node
Administre los nodos que forman un clúster.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| add-configuration-parameter-overrides | Agrega la lista de invalidaciones de configuración en el nodo especificado. |
| disable | Desactive un nodo de clúster de Service Fabric con la intención de desactivación especificada. |
| enable | Active un nodo de clúster de Service Fabric, que está actualmente desactivado. |
| get-configuration-overrides | Obtiene la lista de invalidaciones de configuración en el nodo especificado. |
| health | Obtiene el estado de un nodo de Service Fabric. |
| info | Obtiene la información sobre un nodo específico en el clúster de Service Fabric. |
| list | Obtiene la lista de nodos del clúster de Service Fabric. |
| load | Obtiene la información de carga de un nodo de Service Fabric. |
| remove-configuration-overrides | Quita las invalidaciones de configuración en el nodo especificado. |
| remove-state | Notifica a Service Fabric que el estado persistente en un nodo se quitó o perdió de forma permanente. |
| report-health | Envía un informe de estado sobre el nodo de Service Fabric. |
| restart | Reinicia un nodo de clúster de Service Fabric. |
| transition | Inicia o detiene un nodo de clúster. |
| transition-status | Obtiene el progreso de una operación iniciada mediante StartNodeTransition. |

## <a name="sfctl-node-add-configuration-parameter-overrides"></a>sfctl node add-configuration-parameter-overrides
Agrega la lista de invalidaciones de configuración en el nodo especificado.

Esta API permite agregar todas las invalidaciones de configuración existentes en el nodo especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --config-parameter-override-list [obligatorio] | Descripción para agregar una lista de invalidaciones de configuración. |
| --node-name                      [obligatorio] | El nombre del nodo. |
| --force | Fuerza la adición de invalidaciones de configuración en nodos especificados. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-disable"></a>sfctl node disable
Desactive un nodo de clúster de Service Fabric con la intención de desactivación especificada.

Desactive un nodo de clúster de Service Fabric con la intención de desactivación especificada. Una vez que la desactivación está en curso, la intención de desactivación puede aumentar, pero no disminuir (por ejemplo, un nodo que está desactivado con la intención Pause puede desactivarse aún más con Restart, pero no al revés). Los nodos deben reactivarse mediante la operación Activate a node en cualquier momento después de la desactivación. Si la desactivación no se completa, esto cancelará la desactivación. Un nodo que deja de funcionar y vuelve a activarse mientras está desactivado todavía deberá reactivarse antes de que puedan colocarse servicios en ese nodo.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --deactivation-intent | Describe la intención o el motivo para desactivar el nodo. Los valores posibles son los siguientes. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-enable"></a>sfctl node enable
Active un nodo de clúster de Service Fabric, que está actualmente desactivado.

Active un nodo de clúster de Service Fabric, que está actualmente desactivado. Una vez activado, el nodo volverá a convertirse en un destino viable para colocar las nuevas réplicas, y se volverán a activar las réplicas desactivadas restantes en el nodo.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-get-configuration-overrides"></a>sfctl node get-configuration-overrides
Obtiene la lista de invalidaciones de configuración en el nodo especificado.

Esta API permite obtener todas las invalidaciones de configuración existentes en el nodo especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-health"></a>sfctl node health
Obtiene el estado de un nodo de Service Fabric.

Obtiene el estado de un nodo de Service Fabric. Use EventsHealthStateFilter para filtrar la colección de eventos de estado notificados en el nodo en función del estado de mantenimiento. Si el nodo que se especifica por nombre no existe en el almacén de estado, se devuelve un error.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
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

## <a name="sfctl-node-info"></a>sfctl node info
Obtiene la información sobre un nodo específico en el clúster de Service Fabric.

La respuesta incluye el nombre, el estado, el id., el mantenimiento, el tiempo de actividad y otros detalles acerca del nodo.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-list"></a>sfctl node list
Obtiene la lista de nodos del clúster de Service Fabric.

La respuesta incluye el nombre, el estado, el identificador, el mantenimiento, el tiempo de actividad y otros detalles acerca del nodo.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --max-results | El número máximo de resultados que se devuelven como parte de las consultas paginadas. Este parámetro define el límite superior en el número de resultados devueltos. Los resultados devueltos pueden ser menos que el número máximo de resultados especificado si no caben en el mensaje según las restricciones del tamaño máximo del mensaje definidas en la configuración. Si este parámetro es cero o no se especifica, la consulta paginada incluye tantos resultados como quepan en el mensaje devuelto. |
| --node-status-filter | Permite filtrar los nodos según NodeStatus. Se devolverán únicamente los nodos que coinciden con el valor de filtro especificado. El valor de filtro puede ser uno de los siguientes.  Valor predeterminado\: predeterminado. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-load"></a>sfctl node load
Obtiene la información de carga de un nodo de Service Fabric.

Recupera la información de carga de un nodo de Service Fabric para todas las métricas que tienen una carga o una capacidad definida.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-remove-configuration-overrides"></a>sfctl node remove-configuration-overrides
Quita las invalidaciones de configuración en el nodo especificado.

Esta API permite quitar todas las invalidaciones de configuración existentes en el nodo especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-remove-state"></a>sfctl node remove-state
Notifica a Service Fabric que el estado persistente en un nodo se quitó o perdió de forma permanente.

Esto implica que no es posible recuperar el estado persistente de ese nodo. Por lo general, esto ocurre si se borró un disco duro, o si se bloquea un disco duro. El nodo debe estar inactivo para que esta operación se realice correctamente. Esta operación permite a Service Fabric saber que las réplicas en ese nodo ya no existen, y que Service Fabric debe dejar de esperar a que esas réplicas vuelvan a activarse. No ejecute este cmdlet si no se ha quitado el estado en el nodo y el nodo puede recuperarse con su estado intacto. A partir de Service Fabric 6.5, para poder usar esta API para los nodos de inicialización, cambie los nodos de inicialización a nodos regulares (no de inicialización) y, después, invoque esta API para quitar el estado del nodo. Si el clúster se está ejecutando en Azure, después de que el nodo de inicialización deje de funcionar, Service Fabric intentará cambiarlo automáticamente a un nodo que no sea de inicialización. Para conseguirlo, asegúrese de que el número de nodos que no son de inicialización en el tipo de nodo principal no sea inferior al número de nodos de inicialización inactivos. Si es necesario, agregue más nodos al tipo de nodo principal. En clústeres independientes, si no se espera que el nodo de inicialización inactivo vuelva con su estado intacto, quite el nodo del clúster. Consulte https\://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-windows-server-add-remove-nodes.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-report-health"></a>sfctl node report-health
Envía un informe de estado sobre el nodo de Service Fabric.

Informa del estado de mantenimiento del nodo de Service Fabric especificado. El informe debe contener la información sobre el origen del informe de mantenimiento y la propiedad sobre la cual se informa. El informe se envía a un nodo de puerta de enlace de Service Fabric, que lo reenvía al almacén de estado. La puerta de enlace puede aceptar el informe, pero el almacén de estado puede rechazarlo después de una validación adicional. Por ejemplo, el almacén de estado puede rechazar el informe debido a un parámetro no válido, como un número de secuencia obsoleto. Para ver si el informe se ha aplicado en el almacén de estado, compruebe que aparece en la sección HealthEvents.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --health-property [Obligatorio] | La propiedad de la información de mantenimiento. <br><br> Una entidad puede tener informes de mantenimiento para diferentes propiedades. La propiedad es una cadena y no una enumeración fija para permitir al informador la flexibilidad de categorizar la condición de estado que desencadena el informe. Por ejemplo, un informador con SourceId "LocalWatchdog" puede supervisar el estado del disco disponible en un nodo, por lo que puede informar de la propiedad "AvailableDisk" en ese nodo. El mismo informador puede supervisar la conectividad del nodo, por lo que puede informar de una propiedad "Connectivity" en el mismo nodo. En el almacén de estado, estos informes se tratan como eventos de mantenimiento independientes del nodo especificado. Junto con SourceId, la propiedad identifica la información de mantenimiento de manera única. |
| --health-state    [Obligatorio] | Los valores posibles incluyen\: 'Invalid', 'Ok', 'Warning', 'Error', 'Unknown'. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --source-id       [Obligatorio] | El nombre de origen que identifica el componente de cliente/guardián/sistema que genera la información de estado. |
| --description | La descripción de la información de mantenimiento. <br><br> Representa el texto libre utilizado para agregar información legible por el ser humano sobre el informe. La longitud máxima de la cadena de la descripción es de 4096 caracteres. Si la cadena proporcionada es más larga, se truncará automáticamente. Cuando está truncado, los últimos caracteres de la descripción contienen un marcador "[Truncado]" y el tamaño total de la cadena es de 4096 caracteres. La presencia del marcador indica a los usuarios que el truncamiento se ha producido. Tenga en cuenta que, cuando se trunca, la descripción tiene menos de 4096 caracteres de la cadena original. |
| --immediate | Una marca que indica si el informe se debe enviar inmediatamente. <br><br> Se envía un informe de mantenimiento a una aplicación de puerta de enlace de Service Fabric, que la reenvía al almacén de estado. Si Immediate se establece en true, el informe se envía inmediatamente de la puerta de enlace HTTP al almacén de estado, independientemente de la configuración de cliente de Fabric que esté usando la aplicación de la puerta de enlace HTTP. Esto es útil para los informes críticos que deben enviarse tan pronto como sea posible. En función del tiempo y otras condiciones, el envío del informe puede aún generar un error, por ejemplo, si se cierra la puerta de enlace HTTP o el mensaje no llega a la puerta de enlace. Si Immediate se establece en false, el informe se envía en función de la configuración de cliente de mantenimiento de la puerta de enlace HTTP. Por lo tanto, se procesará por lotes de acuerdo con la configuración de HealthReportSendInterval. Esta es la configuración recomendada porque permite que el cliente de mantenimiento optimice los mensajes de notificación de estado destinados al almacén de estado, así como el procesamiento de informes de mantenimiento. De forma predeterminada, los informes no se envían inmediatamente. |
| --remove-when-expired | Valor que indica si el informe se quita del almacén de estado cuando expire. <br><br> Si se establece en true, el informe se quita del almacén de estado una vez que expire. Si se establece en false, el informe se trata como un error cuando expire. El valor de esta propiedad es false de forma predeterminada. Cuando los clientes notifican periódicamente, deben establecer RemoveWhenExpired en false (valor predeterminado). De esta manera, si el notificador tiene problemas (por ejemplo, un interbloqueo) y no puede informar, la entidad se evalúa al llegar el error cuando expira el informe de mantenimiento. De este modo, se marca que la entidad está en estado de mantenimiento Error. |
| --sequence-number | El número de secuencia para este informe de estado como una cadena numérica. <br><br> El número de secuencia del informe se usa por el almacén de estado para detectar informes obsoletos. Si no se especifica, se genera automáticamente un número de secuencia por el cliente de estado cuando se agrega un informe. |
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

## <a name="sfctl-node-restart"></a>sfctl node restart
Reinicia un nodo de clúster de Service Fabric.

Reinicia un nodo de clúster de Service Fabric que ya se inició.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --create-fabric-dump | Especifique en True para crear un volcado de memoria del proceso de nodo de tejido. Distingue mayúsculas de minúsculas.  Valor predeterminado\: false. |
| --node-instance-id | El identificador de instancia del nodo de destino. Si se especificó el identificador de instancia, se reinicia el nodo solo si coincide con la instancia actual del nodo. Un valor predeterminado de "0" coincidiría con cualquier identificador de instancia. El identificador de instancia puede obtenerse mediante la consulta get node.  Valor predeterminado\: 0. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-transition"></a>sfctl node transition
Inicia o detiene un nodo de clúster.

Inicia o detiene un nodo de clúster.  Un nodo de clúster es un proceso, no la propia instancia del sistema operativo.  Para iniciar un nodo, pase "Start" para el parámetro NodeTransitionType. Para detener un nodo, pase "Stop" para el parámetro NodeTransitionType. Esta API inicia la operación: cuando la API devuelve el nodo, puede no haya terminado de realizar la transición aún. Llame a GetNodeTransitionProgress con el mismo OperationId para obtener el progreso de la operación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-instance-id [Obligatorio] | El identificador de instancia del nodo de destino. Puede determinarse a través de la API GetNodeInfo. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --node-transition-type [Obligatorio] | Indica el tipo de transición que se va a realizar.  NodeTransitionType.Start iniciará un nodo detenido. NodeTransitionType.Stop detendrá un nodo que está en funcionamiento. |
| --operation-id [Obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --stop-duration-in-seconds [Obligatorio] | La duración, en segundos, para conservar el nodo detenido.  El valor mínimo es 600; y el máximo, 14400.  Después de que expire este tiempo, el nodo automáticamente volverá a estar en funcionamiento. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-node-transition-status"></a>sfctl node transition-status
Obtiene el progreso de una operación iniciada mediante StartNodeTransition.

Obtiene el progreso de una operación iniciada mediante StartNodeTransition, con el OperationId proporcionado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --operation-id [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
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
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).