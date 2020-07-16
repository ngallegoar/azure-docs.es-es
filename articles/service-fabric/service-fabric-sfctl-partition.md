---
title: 'CLI de Azure Service Fabric: partición sfctl'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar las particiones de un servicio.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: fff569c586548d84ed55018764363ad7f05e526d
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86257094"
---
# <a name="sfctl-partition"></a>sfctl partition
Consulte y administre las particiones para cualquier servicio.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| data-loss | Esta API inducirá la pérdida de datos para la partición especificada. |
| data-loss-status | Obtiene el progreso de una operación de pérdida de datos de partición que se ha iniciado mediante la API StartDataLoss. |
| health | Obtiene el estado de la partición de Service Fabric especificada. |
| info | Obtiene la información sobre una partición de Service Fabric. |
| list | Obtiene la lista de particiones de un servicio de Service Fabric. |
| load | Obtiene la información de la carga de la partición de Service Fabric especificada. |
| load-reset | Restablece la carga actual de una partición de Service Fabric. |
| quorum-loss | Induce la pérdida de cuórum para una partición determinada del servicio con estado. |
| quorum-loss-status | Obtiene el progreso de una operación de pérdida de cuórum en una partición iniciada mediante la API StartQuorumLoss. |
| recover | Indica al clúster de Service Fabric que debería intentar recuperar una partición específica que actualmente está atascada en pérdida de cuórum. |
| recover-all | Indica al clúster de Service Fabric que debería intentar recuperar cualquier servicio (incluidos los servicios del sistema) que actualmente esté atascado en pérdida de cuórum. |
| report-health | Envía un informe de estado sobre la partición de Service Fabric. |
| restart | Esta API reiniciará algunas o todas las réplicas o instancias de la partición especificada. |
| restart-status | Obtiene el progreso de una operación PartitionRestart iniciada mediante StartPartitionRestart. |
| svc-name | Obtiene el nombre del servicio de Service Fabric para una partición. |

## <a name="sfctl-partition-data-loss"></a>sfctl partition data-loss
Esta API inducirá la pérdida de datos para la partición especificada.

Desencadenará una llamada a la API OnDataLossAsync de la partición.  Esta API inducirá la pérdida de datos para la partición especificada. Desencadenará una llamada a la API OnDataLoss de la partición. La pérdida de datos real dependerá del elemento DataLossMode especificado.
- PartialDataLoss: solo se quita un cuórum de réplicas y OnDataLoss se desencadena para la partición, pero la pérdida de datos real depende de la presencia de la replicación en proceso.  
- FullDataLoss: todas las réplicas se quitan y, por tanto, se pierden todos los datos y se desencadena OnDataLoss. Esta API solo debe llamarse con un servicio con estado como destino. No se recomienda llamar a esta API con un servicio de sistema como destino.

> [!NOTE]   
> Una vez que se ha llamado a esta API, la acción no se puede revertir. Una llamada a CancelOperation solo detiene la ejecución y limpia el estado interno del sistema. No restaurará datos si el comando ha progresado lo suficiente para provocar la pérdida de datos. Llame a la API GetDataLossProgress con el mismo OperationId para devolver información sobre la operación iniciada con esta API.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --data-loss-mode [obligatorio] | Esta enumeración se pasa a la API StartDataLoss para indicar qué tipo de pérdida de datos se inducirá. |
| --operation-id   [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --partition-id   [obligatorio] | La identidad de la partición. |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl partition data-loss-status
Obtiene el progreso de una operación de pérdida de datos de partición que se ha iniciado mediante la API StartDataLoss.

Obtiene el progreso de una operación de pérdida de datos que se ha iniciado con StartDataLoss, mediante el OperationId.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --operation-id [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-health"></a>sfctl partition health
Obtiene el estado de la partición de Service Fabric especificada.

Use EventsHealthStateFilter para filtrar la colección de eventos de estado notificados en el servicio en función del estado de mantenimiento. Use ReplicasHealthStateFilter para filtrar la colección de objetos ReplicaHealthState en la partición. Si especifica una partición que no existe en el almacén de estado, esta solicitud devuelve un error.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --events-health-state-filter | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --exclude-health-statistics | Indica si las estadísticas de mantenimiento se deben devolver como parte del resultado de la consulta. El valor predeterminado es false. Las estadísticas muestran el número de entidades secundarias en estado de mantenimiento Ok, Warning y Error. |
| --replicas-health-state-filter | Permite filtrar la colección de objetos ReplicaHealthState en la partición. El valor puede obtenerse de los miembros o de las operaciones bit a bit en miembros de HealthStateFilter. Se devolverán únicamente las réplicas que coinciden con el filtro. Todas las réplicas se utilizarán para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devolverán todos los eventos con el valor HealthState de OK (2) y Warning (4). Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento.  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-info"></a>sfctl partition info
Obtiene la información sobre una partición de Service Fabric.

Obtiene la información sobre la partición especificada. La respuesta incluye el identificador de partición, la información de esquemas de partición, las claves admitidas por la partición, el estado, el mantenimiento y otros detalles acerca de la partición.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-list"></a>sfctl partition list
Obtiene la lista de particiones de un servicio de Service Fabric.

La respuesta incluye el identificador de partición, la información de esquemas de partición, las claves admitidas por la partición, el estado, el mantenimiento y otros detalles acerca de la partición.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
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

## <a name="sfctl-partition-load"></a>sfctl partition load
Obtiene la información de la carga de la partición de Service Fabric especificada.

Devuelve información sobre la carga de una partición especificada. La respuesta incluye una lista de informes de carga para una partición de Service Fabric. Cada informe incluye el nombre de la métrica de carga, el valor y la última hora notificada en UTC.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-load-reset"></a>sfctl partition load-reset
Restablece la carga actual de una partición de Service Fabric.

Restablece la carga actual de una partición de Service Fabric a la carga de forma predeterminada para el servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl partition quorum-loss
Induce la pérdida de cuórum para una partición determinada del servicio con estado.

Esta API es útil en una situación de pérdida de cuórum temporal en el servicio. Llame a la API GetQuorumLossProgress con el mismo OperationId para devolver información sobre la operación iniciada con esta API. Esto solo se puede llamar en servicios de persistencia con estado (HasPersistedState==true).  No utilice esta API en servicios sin estado o en servicios con estado solo en memoria.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --operation-id         [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --partition-id         [obligatorio] | La identidad de la partición. |
| --quorum-loss-duration [obligatorio] | La cantidad de tiempo que la partición se mantendrá en pérdida de cuórum.  Debe especificarse en segundos. |
| --quorum-loss-mode     [obligatorio] | Esta enumeración se pasa a la API StartQuorumLoss para indicar qué tipo de pérdida de cuórum se inducirá. |
| --service-id           [obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-quorum-loss-status"></a>sfctl partition quorum-loss-status
Obtiene el progreso de una operación de pérdida de cuórum en una partición iniciada mediante la API StartQuorumLoss.

Obtiene el progreso de una operación de pérdida de cuórum iniciada mediante StartQuorumLoss, con el OperationId proporcionado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --operation-id [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-recover"></a>sfctl partition recover
Indica al clúster de Service Fabric que debería intentar recuperar una partición específica que actualmente está atascada en pérdida de cuórum.

Esta operación solo debe realizarse si se sabe que no pueden recuperarse las réplicas que están fuera de servicio. El uso incorrecto de esta API puede provocar la pérdida potencial de datos.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-recover-all"></a>sfctl partition recover-all
Indica al clúster de Service Fabric que debería intentar recuperar cualquier servicio (incluidos los servicios del sistema) que actualmente esté atascado en pérdida de cuórum.

Esta operación solo debe realizarse si se sabe que no pueden recuperarse las réplicas que están fuera de servicio. El uso incorrecto de esta API puede provocar la pérdida potencial de datos.

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

## <a name="sfctl-partition-report-health"></a>sfctl partition report-health
Envía un informe de estado sobre la partición de Service Fabric.

Informa del estado de mantenimiento de la partición de Service Fabric especificada. El informe debe contener la información sobre el origen del informe de mantenimiento y la propiedad sobre la cual se informa. El informe se envía a una partición de puerta de enlace de Service Fabric, que lo reenvía al almacén de estado. La puerta de enlace puede aceptar el informe, pero el almacén de estado puede rechazarlo después de una validación adicional. Por ejemplo, el almacén de estado puede rechazar el informe debido a un parámetro no válido, como un número de secuencia obsoleto. Para ver si el informe se ha aplicado en el almacén de estado, compruebe que aparece en la sección de eventos.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --health-property [Obligatorio] | La propiedad de la información de mantenimiento. <br><br> Una entidad puede tener informes de mantenimiento para diferentes propiedades. La propiedad es una cadena y no una enumeración fija para permitir al informador la flexibilidad de categorizar la condición de estado que desencadena el informe. Por ejemplo, un informador con SourceId "LocalWatchdog" puede supervisar el estado del disco disponible en un nodo, por lo que puede informar de la propiedad "AvailableDisk" en ese nodo. El mismo informador puede supervisar la conectividad del nodo, por lo que puede informar de una propiedad "Connectivity" en el mismo nodo. En el almacén de estado, estos informes se tratan como eventos de mantenimiento independientes del nodo especificado. Junto con SourceId, la propiedad identifica la información de mantenimiento de manera única. |
| --health-state    [Obligatorio] | Los valores posibles incluyen\: 'Invalid', 'Ok', 'Warning', 'Error', 'Unknown'. |
| --partition-id [Obligatorio] | La identidad de la partición. |
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

## <a name="sfctl-partition-restart"></a>sfctl partition restart
Esta API reiniciará algunas o todas las réplicas o instancias de la partición especificada.

Esta API es útil para probar la conmutación por error. Si se utilizan como destino de una partición de servicio sin estado, RestartPartitionMode debe ser AllReplicasOrInstances. Llame a la API GetPartitionRestartProgress con el mismo OperationId para obtener el progreso.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --operation-id [Obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --restart-partition-mode [Obligatorio] | Describe las particiones que se van a reiniciar. |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-restart-status"></a>sfctl partition restart-status
Obtiene el progreso de una operación PartitionRestart iniciada mediante StartPartitionRestart.

Obtiene el progreso de una operación PartitionRestart iniciada mediante StartPartitionRestart con el OperationId proporcionado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --operation-id [obligatorio] | Un GUID que identifica una llamada de esta API.  Se pasa a la API GetProgress correspondiente. |
| --partition-id [Obligatorio] | La identidad de la partición. |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-partition-svc-name"></a>sfctl partition svc-name
Obtiene el nombre del servicio de Service Fabric para una partición.

Obtiene el nombre del servicio para la partición especificada. Si el identificador de partición no existe en el clúster, se devuelve un error 404.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --partition-id [Obligatorio] | La identidad de la partición. |
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
