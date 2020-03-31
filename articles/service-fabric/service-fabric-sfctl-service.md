---
title: 'CLI de Azure Service Fabric: servicio sfctl'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar servicios, tipos de servicio y paquetes de servicio.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 696de713129ca71dd7f2451501a7cc9eca0ee9b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76906242"
---
# <a name="sfctl-service"></a>servicio de sfctl
Cree, elimine y administre servicios, tipos de servicio y paquetes de servicio.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| app-name | Obtiene el nombre de la aplicación de Service Fabric para un servicio. |
| code-package-list | Obtiene la lista de paquetes de código implementados en un nodo de Service Fabric. |
| create | Crea el servicio de Service Fabric especificado. |
| delete | Elimina un servicio existente de Service Fabric. |
| deployed-type | Obtiene la información sobre un tipo de servicio especificado de la aplicación implementada en un nodo en un clúster de Service Fabric. |
| deployed-type-list | Obtiene la lista que contiene información sobre los tipos de servicio desde las aplicaciones implementadas en un nodo en el clúster de Service Fabric. |
| description | Obtiene la descripción de un servicio de Service Fabric existente. |
| get-container-logs | Obtiene los registros de contenedor para el contenedor implementado en un nodo de Service Fabric. |
| health | Obtiene el estado del servicio de Service Fabric especificado. |
| info | Obtiene la información sobre el servicio específico que pertenece a la aplicación de Service Fabric. |
| list | Obtiene la información sobre todos los servicios que pertenecen a la aplicación especificados por el identificador de la aplicación. |
| manifest | Obtiene el manifiesto que describe un tipo de servicio. |
| package-deploy | Descarga los paquetes asociados con el manifiesto de servicio especificado en la memoria caché de imágenes en el nodo especificado. |
| package-health | Obtiene la información sobre el estado de un paquete de servicio para una aplicación específica implementada para una aplicación y nodo de Service Fabric. |
| package-info | Obtiene la lista de paquetes de servicio implementados en un nodo de Service Fabric que coincide exactamente con el nombre especificado. |
| package-list | Obtiene la lista de paquetes de servicio implementados en un nodo de Service Fabric. |
| recover | Indica al clúster de Service Fabric que debería intentar recuperar el servicio especificado, que está actualmente está atascado en pérdida de cuórum. |
| report-health | Envía un informe de mantenimiento sobre el servicio de Service Fabric. |
| resolve | Resuelve una partición de Service Fabric. |
| type-list | Obtiene la lista que contiene la información sobre los tipos de servicio que son compatibles con el tipo de aplicación aprovisionada en un clúster de Service Fabric. |
| update | Actualiza el servicio especificado con la descripción de actualización determinada. |

## <a name="sfctl-service-app-name"></a>sfctl service app-name
Obtiene el nombre de la aplicación de Service Fabric para un servicio.

Obtiene el nombre de la aplicación para el servicio especificado. Si un servicio con el identificador de servicio proporcionado no existe, se devuelve un error 404 FABRIC_E_SERVICE_DOES_NOT_EXIST.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
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

## <a name="sfctl-service-code-package-list"></a>sfctl service code-package-list
Obtiene la lista de paquetes de código implementados en un nodo de Service Fabric.

Obtiene la lista de paquetes de código implementados en un nodo de Service Fabric para la aplicación especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --code-package-name | El nombre del paquete de código especificado en un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
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

## <a name="sfctl-service-create"></a>sfctl service create
Crea el servicio de Service Fabric especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --app-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --name [Obligatorio] | Nombre del servicio. Debe ser un elemento secundario del identificador de la aplicación. Se trata del nombre completo, incluido el URI `fabric\:`. Por ejemplo, el servicio `fabric\:/A/B` es un elemento secundario de la aplicación `fabric\:/A`. |
| --service-type [Obligatorio] | Nombre del tipo de servicio. |
| --activation-mode | El modo de activación para el paquete de servicio. |
| --constraints | Las restricciones de colocación como una cadena. Las restricciones de colocación son expresiones booleanas en las propiedades del nodo y permiten restringir un servicio en nodos concretos según los requisitos de servicio. Por ejemplo, para colocar un servicio en los nodos en los que NodeType está en azul, especifique lo siguiente\: "NodeColor == blue". |
| --correlated-service | Nombre del servicio de destino con el que realizar la correlación. |
| --correlation | Correlaciona el servicio con un servicio existente con una afinidad de alineación. |
| --dns-name | El nombre DNS del servicio que se va a crear. El servicio del sistema DNS de Service Fabric debe activarse para esta configuración. |
| --instance-count | El recuento de instancias. Esto se aplica solo a servicios sin estado. |
| --int-scheme | Indica que el servicio debe particionarse de forma uniforme en un rango de enteros sin signo. |
| --int-scheme-count | El número de particiones dentro del rango de clave de tipo entero que crear, si se utiliza un esquema de partición de entero uniforme. |
| --int-scheme-high | El final del rango de entero de clave, si se usa un esquema de partición de entero uniforme. |
| --int-scheme-low | El inicio del rango de entero de clave, si se usa un esquema de partición de entero uniforme. |
| --load-metrics | Lista codificada en JSON de métricas usadas al realizar servicios de equilibrio de carga en los nodos. |
| --min-replica-set-size | El tamaño del conjunto de réplica mínimo como número. Esto se aplica solo a servicios con estado. |
| --move-cost | Especifica el costo de movimiento de un servicio. Los valores posibles son\: "Zero", "Low", "Medium", "High" y "VeryHigh". |
| --named-scheme | Indica que el servicio debe tener varias particiones con nombre. |
| --named-scheme-list | Lista codificada en JSON de nombres para la partición del servicio, si se usa el esquema de partición con nombre. |
| --no-persisted-state | Si es true, esto indica que el servicio no tiene un estado persistente almacenado en el disco local o solo almacena el estado en la memoria. |
| --placement-policy-list | Lista con codificación JSON de directivas de colocación para el servicio y cualquier nombre de dominio asociado. Las directivas pueden ser una o varias de\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain``RequireDomainDistribution`. |
| --quorum-loss-wait | La duración máxima, en segundos, que se permite que la partición esté en un estado de pérdida de cuórum. Esto se aplica solo a servicios con estado. |
| --replica-restart-wait | La duración, en segundos, entre que una réplica deja de funcionar y se crea una nueva. Esto se aplica solo a servicios con estado. |
| --scaling-policies | Lista codificada de JSON de directivas de escalado para este servicio. |
| --service-placement-time | Tiempo durante el que las réplicas pueden permanecer en compilación antes de que se notifique que la compilación está bloqueada. Esto se aplica solo a servicios con estado. |
| --singleton-scheme | Indica que el servicio debe tener una sola partición o ser un servicio sin particiones. |
| --stand-by-replica-keep | La duración máxima, en segundos, para las que las réplicas de StandBy se mantienen antes de eliminarse. Esto se aplica solo a servicios con estado. |
| --stateful | Indica que el servicio es un servicio con estado. |
| --stateless | Indica que el servicio es un servicio sin estado. |
| --target-replica-set-size | El tamaño del conjunto de réplica de destino como número. Esto se aplica solo a servicios con estado. |
| --timeout -t | Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-delete"></a>sfctl service create
Elimina un servicio existente de Service Fabric.

Una servicio debe crearse para poder eliminarlo. De forma predeterminada, Service Fabric intenta cerrar las réplicas del servicio de forma adecuada y después lo elimina. Sin embargo, si el servicio tiene problemas para cerrar la réplica correctamente, la operación de eliminación puede tardar mucho tiempo o quedarse bloqueada. Use la marca ForceRemove opcional para omitir la secuencia de cierre correcto y forzar la eliminación del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
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

## <a name="sfctl-service-deployed-type"></a>sfctl service deployed-type
Obtiene la información sobre un tipo de servicio especificado de la aplicación implementada en un nodo en un clúster de Service Fabric.

Obtiene la lista que contiene información sobre los tipos de servicio específicos desde las aplicaciones implementadas en un nodo en el clúster de Service Fabric. La respuesta incluye el nombre del tipo de servicio, su estado de registro, el paquete de código que lo ha registrado y el identificador de activación del paquete del servicio. Cada entrada representa una activación de un tipo de servicio, diferenciada por el identificador de activación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id    [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name         [Obligatorio] | El nombre del nodo. |
| --service-type-name [Obligatorio] | Especifica el nombre de un tipo de servicio de Service Fabric. |
| --service-manifest-name | El nombre del manifiesto de servicio para filtrar la lista de información de tipo de servicio implementado. Si se especifica, la respuesta solo contendrá la información sobre los tipos de servicio que se definen en este manifiesto de servicio. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl service deployed-type-list
Obtiene la lista que contiene información sobre los tipos de servicio desde las aplicaciones implementadas en un nodo en el clúster de Service Fabric.

Obtiene la lista que contiene información sobre los tipos de servicio desde las aplicaciones implementadas en un nodo en el clúster de Service Fabric. La respuesta incluye el nombre del tipo de servicio, su estado de registro, el paquete de código que lo ha registrado y el identificador de activación del paquete del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --service-manifest-name | El nombre del manifiesto de servicio para filtrar la lista de información de tipo de servicio implementado. Si se especifica, la respuesta solo contendrá la información sobre los tipos de servicio que se definen en este manifiesto de servicio. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-description"></a>Descripción del servicio sfctl
Obtiene la descripción de un servicio de Service Fabric existente.

Obtiene la descripción de un servicio de Service Fabric existente. Debe crearse un servicio para poder obtener su descripción.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
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

## <a name="sfctl-service-get-container-logs"></a>sfctl service get-container-logs
Obtiene los registros de contenedor para el contenedor implementado en un nodo de Service Fabric.

Obtiene los registros de contenedor para el contenedor implementado en un nodo de Service Fabric para el paquete de código especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id        [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --code-package-name     [Obligatorio] | El nombre del paquete de código especificado en un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --node-name             [Obligatorio] | El nombre del nodo. |
| --service-manifest-name [Obligatorio] | El nombre de un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --previous | Especifica si se deben obtener los registros de contenedor de los contenedores cerrados/fallidos de la instancia de paquete de código. |
| --tail | Número de líneas para mostrar desde el final de los registros. El valor predeterminado es 100. "all" para mostrar los registros completos. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-health"></a>sfctl service health
Obtiene el estado del servicio de Service Fabric especificado.

Obtiene la información de estado del servicio especificado. Use EventsHealthStateFilter para filtrar la colección de eventos de estado notificados en el servicio en función del estado de mantenimiento. Use el elemento PartitionsHealthStateFilter para filtrar la colección de particiones devueltas. Si especifica un servicio que no existe en el almacén de estado, esta solicitud devuelve un error.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --events-health-state-filter | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --exclude-health-statistics | Indica si las estadísticas de mantenimiento se deben devolver como parte del resultado de la consulta. El valor predeterminado es false. Las estadísticas muestran el número de entidades secundarias en estado de mantenimiento Ok, Warning y Error. |
| --partitions-health-state-filter | Permite el filtrado de los objetos de estado de mantenimiento de particiones devueltos en el resultado del la consulta de estado de servicio en función de su estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente las particiones que coinciden con el filtro. Todas las particiones se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit 'OR'. Por ejemplo, si el valor proporcionado es 6, se devolverá el estado de mantenimiento de las particiones con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-info"></a>sfctl service info
Obtiene la información sobre el servicio específico que pertenece a la aplicación de Service Fabric.

Devuelve la información sobre el servicio específico que pertenece a una aplicación de Service Fabric específica.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
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

## <a name="sfctl-service-list"></a>sfctl service list
Obtiene la información sobre todos los servicios que pertenecen a la aplicación especificados por el identificador de la aplicación.

Devuelve la información sobre todos los servicios que pertenecen a la aplicación especificados por el identificador de la aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --service-type-name | El nombre de tipo de servicio usado para filtrar los servicios que se van a consultar. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-manifest"></a>sfctl service manifest
Obtiene el manifiesto que describe un tipo de servicio.

Obtiene el manifiesto que describe un tipo de servicio. La respuesta contiene el manifiesto de servicio XML como una cadena.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-type-name [Obligatorio] | Nombre del tipo de aplicación. |
| --application-type-version [Obligatorio] | La versión del tipo de aplicación. |
| --service-manifest-name [Obligatorio] | El nombre de un manifiesto de servicio registrado como parte de un tipo de aplicación en un clúster de Service Fabric. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-package-deploy"></a>sfctl service package-deploy
Descarga los paquetes asociados con el manifiesto de servicio especificado en la memoria caché de imágenes en el nodo especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --app-type-name         [Obligatorio] | El nombre del manifiesto de aplicación para el manifiesto de servicio solicitado correspondiente. |
| --app-type-version      [Obligatorio] | La versión del manifiesto de aplicación para el manifiesto de servicio solicitado correspondiente. |
| --node-name             [Obligatorio] | El nombre del nodo. |
| --service-manifest-name [Obligatorio] | El nombre de manifiesto de servicio asociado a los paquetes que se van a descargar. |
| --share-policy | Lista con codificación JSON de las directivas de uso compartido. Cada elemento de la directiva de uso compartido se compone de los elementos "name" y "scope". El nombre corresponde al nombre del código, configuración o paquete de datos que va a compartirse. El ámbito puede ser "None", "All", "Code", "Config" o "Data". |
| --timeout -t | Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-package-health"></a>sfctl service package-health
Obtiene la información sobre el estado de un paquete de servicio para una aplicación específica implementada para una aplicación y nodo de Service Fabric.

Obtiene la información sobre el estado de un paquete de servicio para una aplicación específica implementada en un nodo de Service Fabric. Utilice el parámetro EventsHealthStateFilter para filtrar opcionalmente para la colección de objetos HealthEvent notificados en el paquete de servicio implementado según el estado de mantenimiento.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id       [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name            [Obligatorio] | El nombre del nodo. |
| --service-package-name [Obligatorio] | El nombre del paquete de servicio. |
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

## <a name="sfctl-service-package-info"></a>sfctl service package-info
Obtiene la lista de paquetes de servicio implementados en un nodo de Service Fabric que coincide exactamente con el nombre especificado.

Devuelve la información sobre los paquetes de servicio implementados en un nodo de Service Fabric para la aplicación especificada. Estos resultados son los paquetes de servicio cuyo nombre coincide exactamente con el nombre del paquete de servicio como parámetro.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id       [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name            [Obligatorio] | El nombre del nodo. |
| --service-package-name [Obligatorio] | El nombre del paquete de servicio. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-package-list"></a>sfctl service package-list
Obtiene la lista de paquetes de servicio implementados en un nodo de Service Fabric.

Devuelve la información sobre los paquetes de servicio implementados en un nodo de Service Fabric para la aplicación especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
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

## <a name="sfctl-service-recover"></a>sfctl service recover
Indica al clúster de Service Fabric que debería intentar recuperar el servicio especificado, que está actualmente está atascado en pérdida de cuórum.

Indica al clúster de Service Fabric que debería intentar recuperar el servicio especificado, que está actualmente está atascado en pérdida de cuórum. Esta operación solo debe realizarse si se sabe que no pueden recuperarse las réplicas que están fuera de servicio. El uso incorrecto de esta API puede provocar la pérdida potencial de datos.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
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

## <a name="sfctl-service-report-health"></a>sfctl service report-health
Envía un informe de mantenimiento sobre el servicio de Service Fabric.

Informa del estado de mantenimiento del servicio de Service Fabric especificado. El informe debe contener la información sobre el origen del informe de mantenimiento y la propiedad sobre la cual se informa. El informe se envía a un servicio de puerta de enlace de Service Fabric, que lo reenvía al almacén de estado. La puerta de enlace puede aceptar el informe, pero el almacén de estado puede rechazarlo después de una validación adicional. Por ejemplo, el almacén de estado puede rechazar el informe debido a un parámetro no válido, como un número de secuencia obsoleto. Para ver si el informe se ha aplicado en el almacén de estado, compruebe que aparece en los eventos de mantenimiento del servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --health-property [Obligatorio] | La propiedad de la información de mantenimiento. <br><br> Una entidad puede tener informes de mantenimiento para diferentes propiedades. La propiedad es una cadena y no una enumeración fija para permitir al informador la flexibilidad de categorizar la condición de estado que desencadena el informe. Por ejemplo, un informador con SourceId "LocalWatchdog" puede supervisar el estado del disco disponible en un nodo, por lo que puede informar de la propiedad "AvailableDisk" en ese nodo. El mismo informador puede supervisar la conectividad del nodo, por lo que puede informar de una propiedad "Connectivity" en el mismo nodo. En el almacén de estado, estos informes se tratan como eventos de mantenimiento independientes del nodo especificado. Junto con SourceId, la propiedad identifica la información de mantenimiento de manera única. |
| --health-state    [Obligatorio] | Los valores posibles incluyen\: 'Invalid', 'Ok', 'Warning', 'Error', 'Unknown'. |
| --service-id      [Obligatorio] | La identidad del servicio. <br><br> Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" in 6.0+ and "myapp/app1/svc1" en las versiones anteriores. |
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

## <a name="sfctl-service-resolve"></a>sfctl service resolve
Resuelve una partición de Service Fabric.

Resuelve una partición de servicio de Service Fabric para obtener los puntos de conexión de las réplicas de servicio.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre del servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" en 6.0+ y "myapp/svc1/svc1" en las versiones anteriores. |
| --partition-key-type | Tipo de clave para la partición. Este parámetro es obligatorio si el esquema de partición para el servicio es Int64Range o Named. Los valores posibles son los siguientes. -None (1): indica que no se especifica el parámetro PartitionKeyValue. Esto es válido para las particiones con el esquema de partición como Singleton. Este es el valor predeterminado. El valor es 1. -Int64Range (2): indica que el parámetro PartitionKeyValue es una clave de partición int64. Esto es válido para las particiones con el esquema de partición como Int64Range. El valor es 2. - Named (3): indica que el parámetro PartitionKeyValue es un nombre de la partición. Esto es válido para las particiones con el esquema de partición como Named. El valor es 3. |
| --partition-key-value | Clave de partición. Es obligatorio si el esquema de partición para el servicio es Int64Range o Named. Esto no es el identificador de partición, sino el valor de clave entero o el nombre del identificador de partición. Por ejemplo, si el servicio usa particiones por rangos de 0 a 10, PartitionKeyValue sería un entero en ese rango. Consulte la descripción del servicio para ver el rango o el nombre. |
| --previous-rsp-version | El valor en el campo Versión de la respuesta que se recibió anteriormente. Es obligatorio si el usuario sabe que el resultado que se obtuvo anteriormente está obsoleto. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-type-list"></a>sfctl service type-list
Obtiene la lista que contiene la información sobre los tipos de servicio que son compatibles con el tipo de aplicación aprovisionada en un clúster de Service Fabric.

Obtiene la lista que contiene la información sobre los tipos de servicio que son compatibles con el tipo de aplicación aprovisionada en un clúster de Service Fabric. Debe existir el tipo de aplicación proporcionado. En caso contrario, se devuelve un estado 404.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-type-name [Obligatorio] | Nombre del tipo de aplicación. |
| --application-type-version [Obligatorio] | La versión del tipo de aplicación. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-service-update"></a>sfctl service update
Actualiza el servicio especificado con la descripción de actualización determinada.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --service-id [Obligatorio] | La identidad del servicio. Este suele ser el nombre completo del servicio sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de servicio es "fabric\:/myapp/app1/svc1", la identidad del servicio sería "myapp\~app1\~svc1" in 6.0+ and "myapp/app1/svc1" en las versiones anteriores. |
| --constraints | Las restricciones de colocación como una cadena. Las restricciones de colocación son expresiones booleanas en las propiedades del nodo y permiten restringir un servicio en nodos concretos según los requisitos de servicio. Por ejemplo, para colocar un servicio en los nodos en los que NodeType está en azul, especifique lo siguiente\: "NodeColor == blue". |
| --correlated-service | Nombre del servicio de destino con el que realizar la correlación. |
| --correlation | Correlaciona el servicio con un servicio existente con una afinidad de alineación. |
| --instance-count | El recuento de instancias. Esto se aplica solo a servicios sin estado. |
| --load-metrics | Lista con codificación JSON de métricas usadas al realizar el equilibrio de carga en los nodos. |
| --min-replica-set-size | El tamaño del conjunto de réplica mínimo como número. Esto se aplica solo a servicios con estado. |
| --move-cost | Especifica el costo de movimiento de un servicio. Los valores posibles son\: "Zero", "Low", "Medium", "High" y "VeryHigh". |
| --placement-policy-list | Lista con codificación JSON de directivas de colocación para el servicio y cualquier nombre de dominio asociado. Las directivas pueden ser una o varias de\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain``RequireDomainDistribution`. |
| --quorum-loss-wait | La duración máxima, en segundos, que se permite que la partición esté en un estado de pérdida de cuórum. Esto se aplica solo a servicios con estado. |
| --replica-restart-wait | La duración, en segundos, entre que una réplica deja de funcionar y se crea una nueva. Esto se aplica solo a servicios con estado. |
| --scaling-policies | Lista codificada de JSON de directivas de escalado para este servicio. |
| --service-placement-time | Tiempo durante el que las réplicas pueden permanecer en compilación antes de que se notifique que la compilación está bloqueada. Esto se aplica solo a servicios con estado. |
| --stand-by-replica-keep | La duración máxima, en segundos, para las que las réplicas de StandBy se mantienen antes de eliminarse. Esto se aplica solo a servicios con estado. |
| --stateful | Indica que el servicio de destino es un servicio con estado. |
| --stateless | Indica que el servicio de destino es un servicio sin estado. |
| --target-replica-set-size | El tamaño del conjunto de réplica de destino como número. Esto se aplica solo a servicios con estado. |
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
- Obtenga información sobre cómo utilizar la CLI de Service Fabric con los [scripts de ejemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).
