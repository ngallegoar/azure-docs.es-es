---
title: 'CLI de Azure Service Fabric: aplicación de sfctl'
description: Más información sobre sfctl, la interfaz de la línea de comandos de Azure Service Fabric. Incluye una lista de comandos para administrar aplicaciones.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 880770345eb7d65850db322bd97d64c60b6681ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86260875"
---
# <a name="sfctl-application"></a>aplicación de sfctl
Cree, elimine y administre aplicaciones y tipos de aplicaciones.

## <a name="commands"></a>Comandos:

|Get-Help|Descripción|
| --- | --- |
| create | Crea una aplicación de Service Fabric con la descripción especificada. |
| delete | Elimina una aplicación existente de Service Fabric. |
| deployed | Obtiene la información sobre una aplicación implementada en un nodo de Service Fabric. |
| deployed-health | Obtiene la información sobre el mantenimiento de una aplicación implementada en un nodo de Service Fabric. |
| deployed-list | Obtiene la lista de las aplicaciones implementadas en un nodo de Service Fabric. |
| health | Obtiene el estado de la aplicación de Service Fabric. |
| info | Obtiene la información sobre una aplicación de Service Fabric. |
| list | Obtiene la lista de aplicaciones creadas en el clúster de Service Fabric que coinciden con los filtros especificados. |
| load | Obtiene la información de carga sobre una aplicación de Service Fabric. |
| manifest | Obtiene el manifiesto que describe un tipo de aplicación. |
| provision | Aprovisiona o registra un tipo de aplicación de Service Fabric con el clúster mediante el paquete ".sfpkg" en el almacén externo o mediante el paquete de aplicación en el almacén de imágenes. |
| report-health | Envía un informe de estado sobre la aplicación de Service Fabric. |
| type | Obtiene la lista de tipos de aplicaciones del clúster de Service Fabric que coinciden exactamente con el nombre especificado. |
| type-list | Obtiene la lista de tipos de aplicaciones del clúster de Service Fabric. |
| unprovision | Elimina un tipo de aplicación de Service Fabric del clúster o anula su registro. |
| upgrade | Inicia la actualización de una aplicación en el clúster de Service Fabric. |
| upgrade-resume | Reanuda la actualización de una aplicación en el clúster de Service Fabric. |
| upgrade-rollback | Inicia la reversión de la actualización en curso de una aplicación en el clúster de Service Fabric. |
| upgrade-status | Obtiene los detalles de la última actualización de esta aplicación. |
| upload | Copia un paquete de aplicación de Service Fabric en el almacén de imágenes. |

## <a name="sfctl-application-create"></a>sfctl application create
Crea una aplicación de Service Fabric con la descripción especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --app-name [Obligatorio] | El nombre de la aplicación, incluido el esquema URI 'fabric\:'. |
| --app-type [Obligatorio] | El nombre del tipo de aplicación que se encuentra en el manifiesto de aplicación. |
| --app-version [Obligatorio] | La versión del tipo de aplicación, tal como se define en el manifiesto de aplicación. |
| --max-node-count | El número máximo de nodos que Service Fabric reserva como capacidad para esta aplicación. Tenga en cuenta que esto no significa que los servicios de esta aplicación estarán colocados en todos estos nodos. |
| --metrics | Una lista codificada de JSON con las descripciones de las métricas de capacidad de la aplicación. Una métrica se define como un nombre, asociado con un conjunto de funcionalidades de cada nodo en que se encuentra la aplicación. |
| --min-node-count | El número mínimo de nodos que Service Fabric reserva como capacidad para esta aplicación. Tenga en cuenta que esto no significa que los servicios de esta aplicación estarán colocados en todos estos nodos. |
| --parameters | Una lista codificada en JSON con los reemplazos de los parámetros de la aplicación que se aplicarán al crear la aplicación. |
| --timeout -t | Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-delete"></a>sfctl application delete
Elimina una aplicación existente de Service Fabric.

Una aplicación debe crearse para poder eliminarla. Al eliminar una aplicación, se eliminan todos los servicios que forman parte de ella. De forma predeterminada, Service Fabric intenta cerrar las réplicas del servicio de forma adecuada y después lo elimina. Sin embargo, si el servicio tiene problemas para cerrar la réplica correctamente, la operación de eliminación puede tardar mucho tiempo o quedarse bloqueada. Use la marca ForceRemove opcional para omitir la secuencia de cierre correcto y forzar la eliminación de la aplicación y de todos sus servicios.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
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

## <a name="sfctl-application-deployed"></a>sfctl application deployed
Obtiene la información sobre una aplicación implementada en un nodo de Service Fabric.

Esta consulta devuelve información de la aplicación del sistema si el identificador de aplicación proporcionado es para la aplicación del sistema. Los resultados abarcan las aplicaciones implementadas en los estados activo, activando y descargando. Esta consulta requiere que el nombre del nodo corresponda a un nodo en el clúster. La consulta produce un error si el nombre de nodo proporcionado no apunta a ningún nodo de Service Fabric activo en el clúster.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name [Obligatorio] | El nombre del nodo. |
| --include-health-state | Incluir el estado de mantenimiento de una entidad. Si este parámetro es falso o no se especifica, el estado de mantenimiento devuelto es "Unknown". Cuando se establece en true, la consulta va en paralelo al nodo y al servicio del sistema de mantenimiento antes de que los resultados se combinen. Como resultado, la consulta es más costosa y puede tardar más tiempo. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-deployed-health"></a>sfctl application deployed-health
Obtiene la información sobre el mantenimiento de una aplicación implementada en un nodo de Service Fabric.

Obtiene la información sobre el mantenimiento de una aplicación implementada en un nodo de Service Fabric. Utilice el parámetro EventsHealthStateFilter para filtrar opcionalmente para la colección de objetos HealthEvent notificados en la aplicación implementada según el estado de mantenimiento. Utilice el parámetro DeployedServicePackagesHealthStateFilter para filtrar opcionalmente para los elementos secundarios de DeployedServicePackageHealth según el estado de mantenimiento.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id                     [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --node-name                          [Obligatorio] | El nombre del nodo. |
| --deployed-service-packages-health-state-filter | Permite filtrar los objetos de estado de mantenimiento del paquete de servicio implementado devueltos en el resultado de la consulta de mantenimiento de la aplicación implementada según su estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los paquetes de servicio implementados que coinciden con el filtro. Todos los paquetes de servicio implementados se utilizan para evaluar el estado de mantenimiento agregado de la aplicación implementada. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelve el estado de mantenimiento de los paquetes de servicio con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --events-health-state-filter | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --exclude-health-statistics | Indica si las estadísticas de mantenimiento se deben devolver como parte del resultado de la consulta. El valor predeterminado es false. Las estadísticas muestran el número de entidades secundarias en estado de mantenimiento Ok, Warning y Error. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-deployed-list"></a>sfctl application deployed-list
Obtiene la lista de las aplicaciones implementadas en un nodo de Service Fabric.

Obtiene la lista de las aplicaciones implementadas en un nodo de Service Fabric. Los resultados no incluyen información sobre las aplicaciones de sistema implementadas a menos que el ID lo solicite explícitamente. Los resultados abarcan las aplicaciones implementadas en los estados activo, activando y descargando. Esta consulta requiere que el nombre del nodo corresponda a un nodo en el clúster. La consulta produce un error si el nombre de nodo proporcionado no apunta a ningún nodo de Service Fabric activo en el clúster.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --node-name [Obligatorio] | El nombre del nodo. |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --include-health-state | Incluir el estado de mantenimiento de una entidad. Si este parámetro es falso o no se especifica, el estado de mantenimiento devuelto es "Unknown". Cuando se establece en true, la consulta va en paralelo al nodo y al servicio del sistema de mantenimiento antes de que los resultados se combinen. Como resultado, la consulta es más costosa y puede tardar más tiempo. |
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

## <a name="sfctl-application-health"></a>sfctl application health
Obtiene el estado de la aplicación de Service Fabric.

Devuelve el estado de mantenimiento de la aplicación de Service Fabric. La respuesta notifica un estado de mantenimiento correcto, con error o con una advertencia. Si la entidad no se encuentra en el almacén de estado, devuelve un error.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --deployed-applications-health-state-filter | Permite filtrar los objetos de estado de mantenimiento de las aplicaciones implementadas devueltos en el resultado de la consulta de mantenimiento de las aplicaciones según su estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Solo se devolverán las aplicaciones implementadas que coincidan con el filtro. Todas las aplicaciones implementadas se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelve el estado de mantenimiento de las aplicaciones implementadas con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --events-health-state-filter | Permite filtrar la colección de objetos HealthEvent devueltos según el estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los eventos que coinciden con el filtro. Todos los eventos se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelven todos los eventos con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --exclude-health-statistics | Indica si las estadísticas de mantenimiento se deben devolver como parte del resultado de la consulta. El valor predeterminado es false. Las estadísticas muestran el número de entidades secundarias en estado de mantenimiento Ok, Warning y Error. |
| --services-health-state-filter | Permite filtrar los objetos de estado de mantenimiento de los servicios devueltos en el resultado del la consulta de mantenimiento de los servicios en función de su estado de mantenimiento. Los valores posibles para este parámetro incluyen el valor entero de uno de los siguientes estados de mantenimiento. Se devuelven únicamente los servicios que coinciden con el filtro. Todos los servicios se utilizan para evaluar el estado de mantenimiento agregado. Si no se especifica, se devuelven todas las entradas. Los valores de estado se marcan según la enumeración, por lo que el valor puede ser una combinación de estos valores obtenidos mediante el operador bit a bit "OR". Por ejemplo, si el valor proporcionado es 6, se devuelve el estado de mantenimiento de los servicios con el valor HealthState de Ok (2) y Warning (4).  <br> - Default: valor predeterminado. Coincide con cualquier HealthState. El valor predeterminado es cero.  <br> - None: filtro que no coincide con ningún valor de HealthState. Se utiliza para no devolver ningún resultado en una determinada colección de estados. El valor es 1.  <br> - Ok: filtro que asocia la entrada con el valor de HealthState de Ok. El valor es 2.  <br> - Warning: filtro que asocia la entrada con el valor de HealthState de Warning. El valor es 4.  <br> - Error: filtro que asocia la entrada con el valor de HealthState de Error. El valor es 8.  <br> - All: filtro que asocia la entrada con cualquier valor de HealthState. El valor es 65535. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-info"></a>sfctl application info
Obtiene la información sobre una aplicación de Service Fabric.

Devuelve la información sobre la aplicación que se creó o que está en proceso de creación en el clúster de Service Fabric y cuyo nombre coincide con el especificado como parámetro. La respuesta incluye el nombre, el tipo, el estado, los parámetros y otros detalles sobre la aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --exclude-application-parameters | La marca que especifica si los parámetros de la aplicación se excluirán del resultado. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-list"></a>sfctl application list
Obtiene la lista de aplicaciones creadas en el clúster de Service Fabric que coinciden con los filtros especificados.

Obtiene la información sobre las aplicaciones que se crearon o que están en proceso de creación en el clúster de Service Fabric y que coinciden con los filtros especificados. La respuesta incluye el nombre, el tipo, el estado, los parámetros y otros detalles sobre la aplicación. Si las aplicaciones no caben en una página, se devuelve una página de resultados, así como un token de continuación que puede usarse para obtener la página siguiente. No se pueden especificar los filtros ApplicationTypeName y ApplicationDefinitionKindFilter a la vez.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-definition-kind-filter | Se usa para filtrar según ApplicationDefinitionKind que es el mecanismo utilizado para definir una aplicación de Service Fabric.  <br> - Default: valor predeterminado que realiza la misma función que la selección de "All". El valor es 0.  <br> - All: filtro que asocia la entrada con cualquier valor de ApplicationDefinitionKind. El valor es 65535.  <br> -ServiceFabricApplicationDescription: filtro que asocia la entrada con el valor ServiceFabricApplicationDescription de ApplicationDefinitionKind. El valor es 1.  <br> - All: filtro que asocia la entrada con el valor Compose de ApplicationDefinitionKind. El valor es 2. |
| --application-type-name | El nombre del tipo de aplicación utilizado para filtrar las aplicaciones que se van a consultar. Este valor no debe contener la versión del tipo de aplicación. |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --exclude-application-parameters | La marca que especifica si los parámetros de la aplicación se excluirán del resultado. |
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

## <a name="sfctl-application-load"></a>sfctl application load
Obtiene la información de carga sobre una aplicación de Service Fabric.

Devuelve la información de carga sobre la aplicación que se creó o que está en proceso de crearse en el clúster de Service Fabric y cuyo nombre coincide con el especificado como parámetro. La respuesta incluye el nombre, los nodos mínimos y máximos, el número de nodos de los que la aplicación se ocupa actualmente y la información de métrica de carga sobre la aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-manifest"></a>sfctl application manifest
Obtiene el manifiesto que describe un tipo de aplicación.

La respuesta contiene el manifiesto de aplicación XML como una cadena.

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

## <a name="sfctl-application-provision"></a>sfctl application provision
Aprovisiona o registra un tipo de aplicación de Service Fabric con el clúster mediante el paquete ".sfpkg" en el almacén externo o mediante el paquete de aplicación en el almacén de imágenes.

Aprovisiona un tipo de aplicación de Service Fabric con el clúster. El aprovisionamiento es necesario para poder crear instancias de las aplicaciones nuevas. La operación de aprovisionamiento se puede realizar en el paquete de aplicación especificado por relativePathInImageStore o mediante el identificador URI del ".sfpkg" externo. A menos que se establezca --external-provision, este comando espera un aprovisionamiento del almacén de imágenes.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-package-download-uri | La ruta al paquete de aplicación '.sfpkg' desde la que puede descargar el paquete de aplicación mediante los protocolos HTTP o HTTPS. <br><br> Solo para aprovisionamiento de un almacén externo de tipo. El paquete de aplicación puede almacenarse en un almacén externo que proporcione la operación GET para descargar el archivo. Los protocolos compatibles son HTTP y HTTPS, y la ruta de acceso debe permitir un acceso de lectura. |
| --application-type-build-path | Solo para aprovisionamiento de un almacén de imágenes de tipo. La ruta de acceso relativa para el paquete de aplicación en el almacén de imágenes especificado durante la operación de carga anterior. |
| --application-type-name | Solo para aprovisionamiento de un almacén externo de tipo. El nombre del tipo de aplicación representa el nombre del tipo de aplicación que se encuentra en el manifiesto de aplicación. |
| --application-type-version | Solo para aprovisionamiento de un almacén externo de tipo. La versión del tipo de aplicación representa la versión del tipo de aplicación que se encuentra en el manifiesto de aplicación. |
| --external-provision | La ubicación desde donde se puede registrar o aprovisionar un paquete de aplicación. Indica que el aprovisionamiento es para un paquete de aplicación que se ha cargado previamente en un almacén externo. El paquete de aplicación termina con la extensión *.sfpkg. |
| --no-wait | Indica si el aprovisionamiento se debe realizar o no de forma asincrónica. <br><br> Cuando se establece en true, la operación de aprovisionamiento se devuelve cuando el sistema acepta la solicitud y la operación de aprovisionamiento continúa sin ningún límite de tiempo de espera. El valor predeterminado es false. Para paquetes de aplicación de gran tamaño, se recomienda establecer el valor en true. |
| --timeout -t | Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-report-health"></a>sfctl application report-health
Envía un informe de estado sobre la aplicación de Service Fabric.

Informa del estado de mantenimiento de la aplicación de Service Fabric especificada. El informe debe contener la información sobre el origen del informe de mantenimiento y la propiedad sobre la cual se informa. Se envía el informe a una aplicación de puerta de enlace de Service Fabric, que la reenvía al almacén de estado. La puerta de enlace puede aceptar el informe, pero el almacén de estado puede rechazarlo después de una validación adicional. Por ejemplo, el almacén de estado puede rechazar el informe debido a un parámetro no válido, como un número de secuencia obsoleto. Para ver si el informe se ha aplicado en el almacén de estado, obtenga el mantenimiento de la aplicación y compruebe que aparece el informe.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. <br><br> Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --health-property [Obligatorio] | La propiedad de la información de mantenimiento. <br><br> Una entidad puede tener informes de mantenimiento para diferentes propiedades. La propiedad es una cadena y no una enumeración fija para permitir al informador la flexibilidad de categorizar la condición de estado que desencadena el informe. Por ejemplo, un informador con SourceId "LocalWatchdog" puede supervisar el estado del disco disponible en un nodo, por lo que puede informar de la propiedad "AvailableDisk" en ese nodo. El mismo informador puede supervisar la conectividad del nodo, por lo que puede informar de una propiedad "Connectivity" en el mismo nodo. En el almacén de estado, estos informes se tratan como eventos de mantenimiento independientes del nodo especificado. Junto con SourceId, la propiedad identifica la información de mantenimiento de manera única. |
| --health-state    [Obligatorio] | Los valores posibles incluyen\: 'Invalid', 'Ok', 'Warning', 'Error', 'Unknown'. |
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

## <a name="sfctl-application-type"></a>sfctl application type
Obtiene la lista de tipos de aplicaciones del clúster de Service Fabric que coinciden exactamente con el nombre especificado.

Devuelve la información sobre los tipos de aplicaciones aprovisionados o en proceso de aprovisionamiento en el clúster de Service Fabric. Estos resultados son los tipos de aplicaciones cuyo nombre coincide exactamente con el especificado como parámetro y que coinciden con los parámetros de consulta establecidos. Se devuelven todas las versiones del tipo de aplicación que coinciden con el nombre del tipo de aplicación, y cada versión se devuelve como un tipo de aplicación. La respuesta incluye el nombre, la versión, el estado y otros detalles sobre el tipo de la aplicación. Se trata de una consulta paginada, lo que significa que, si no caben todos los tipos de aplicaciones en una página, se devuelve una página de resultados, así como un token de continuación que puede usarse para obtener la página siguiente. Por ejemplo, si hay diez tipos de aplicaciones, pero en una página solo caben tres o si el número máximo de resultados está establecido en tres, entonces solo se devuelven tres. Para acceder a los demás resultados, recupere las páginas siguientes con el token de continuación devuelto en la siguiente consulta. Si no existen páginas sucesivas, se devuelve un token de continuación vacío.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-type-name [Obligatorio] | Nombre del tipo de aplicación. |
| --application-type-version | La versión del tipo de aplicación. |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --exclude-application-parameters | La marca que especifica si los parámetros de la aplicación se excluirán del resultado. |
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

## <a name="sfctl-application-type-list"></a>sfctl application type-list
Obtiene la lista de tipos de aplicaciones del clúster de Service Fabric.

Devuelve la información sobre los tipos de aplicaciones aprovisionados o en proceso de aprovisionamiento en el clúster de Service Fabric. Cada versión de un tipo de aplicación se devuelve como tipo de aplicación. La respuesta incluye el nombre, la versión, el estado y otros detalles sobre el tipo de la aplicación. Se trata de una consulta paginada, lo que significa que, si no caben todos los tipos de aplicaciones en una página, se devuelve una página de resultados, así como un token de continuación que puede usarse para obtener la página siguiente. Por ejemplo, si hay diez tipos de aplicaciones, pero en una página solo caben tres o si el número máximo de resultados está establecido en tres, entonces solo se devuelven tres. Para acceder a los demás resultados, recupere las páginas siguientes con el token de continuación devuelto en la siguiente consulta. Si no existen páginas sucesivas, se devuelve un token de continuación vacío.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-type-definition-kind-filter | Se usa para filtrar según ApplicationTypeDefinitionKind, que es el mecanismo utilizado para definir un tipo de aplicación de Service Fabric.  <br> - Default: valor predeterminado que realiza la misma función que la selección de "All". El valor es 0.  <br> - All: filtro que asocia la entrada con cualquier valor de ApplicationTypeDefinitionKind. El valor es 65535.  <br> - ServiceFabricApplicationPackage: filtro que asocia la entrada con el valor ApplicationTypeDefinitionKind de ServiceFabricApplicationPackage. El valor es 1.  <br> - Compose: filtro que asocia la entrada con el valor Compose de ApplicationTypeDefinitionKind. El valor es 2. |
| --continuation-token | El parámetro continuation token se utiliza para obtener el siguiente conjunto de resultados. Un token de continuación con un valor no vacío se incluye en la respuesta de la API cuando los resultados del sistema no caben en una única respuesta. Cuando este valor se pasa a la siguiente llamada de la API, la API devuelve el siguiente conjunto de resultados. Si no hay más resultados, el token de continuación no contiene ningún valor. El valor de este parámetro no debe ser la dirección URL codificada. |
| --exclude-application-parameters | La marca que especifica si los parámetros de la aplicación se excluirán del resultado. |
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

## <a name="sfctl-application-unprovision"></a>sfctl application unprovision
Elimina un tipo de aplicación de Service Fabric del clúster o anula su registro.

Esta operación solo puede realizarse si se han eliminado todas las instancias de la aplicación del tipo de aplicación. Una vez anulado el registro del tipo de aplicación, no se pueden crear instancias de la aplicación para este tipo de aplicación concreto.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-type-name [Obligatorio] | Nombre del tipo de aplicación. |
| --application-type-version [Obligatorio] | La versión del tipo de aplicación, tal como se define en el manifiesto de aplicación. |
| --async-parameter | La marca indica si el desaprovisionamiento se debe realizar o no de forma asincrónica. Cuando se establece en true, la operación de desaprovisionamiento se devuelve cuando el sistema acepta la solicitud y la operación de desaprovisionamiento continúa sin ningún límite de tiempo de espera. El valor predeterminado es false. Sin embargo, se recomienda que lo establezca en true para los paquetes de aplicación de gran tamaño que se han aprovisionado. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-upgrade"></a>sfctl application upgrade
Inicia la actualización de una aplicación en el clúster de Service Fabric.

Valida los parámetros de actualización de la aplicación proporcionados e inicia la actualización de la aplicación si los parámetros son válidos. Tenga en cuenta que la descripción de la actualización reemplaza la descripción existente. Esto significa que, si no se especifican los parámetros, los que ya existen en la aplicación se sobrescribirán con la lista de parámetros vacía. Esto podría dar lugar a que la aplicación use el valor predeterminado de los parámetros del manifiesto de aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. <br><br> Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --application-version [Obligatorio] | La versión del tipo de aplicación de destino (que se encuentra en el manifiesto de aplicación) para la actualización de la aplicación. |
| --parameters          [Obligatorio] | Una lista codificada en JSON de reemplazos de parámetros de aplicación que se aplicarán al actualizar la aplicación. |
| --default-service-health-policy | Especificación codificada en JSON de la directiva de mantenimiento que se usa de forma predeterminada para evaluar el estado de un tipo de servicio. |
| --failure-action | La acción para realizar cuando una actualización supervisada detecta infracciones de directivas de mantenimiento o supervisión. |
| --force-restart | Reinicia de forma forzosa los procesos durante la actualización incluso si no ha cambiado la versión de código. |
| --health-check-retry-timeout | El período de tiempo entre intentos para realizar comprobaciones de mantenimiento si la aplicación o el clúster no funcionan correctamente.  Valor predeterminado\: PT0H10M0S. |
| --health-check-stable-duration | La cantidad de tiempo que la aplicación o el clúster deben tener un estado correcto antes de que la actualización continúe con el siguiente dominio de actualización.  Valor predeterminado\: PT0H2M0S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --health-check-wait-duration | El período de tiempo de espera después de completar un dominio de actualización antes de iniciar el proceso de comprobaciones de mantenimiento.  Valor predeterminado\: 0. |
| --max-unhealthy-apps | El porcentaje máximo permitido de aplicaciones implementadas con un estado incorrecto. Se representa como un número entre 0 y 100. |
| --mode | El modo usado para supervisar el mantenimiento durante una actualización gradual.  Valor predeterminado\: UnmonitoredAuto. |
| --replica-set-check-timeout | El período de tiempo máximo para bloquear el procesamiento de un dominio de actualización y evitar la pérdida de disponibilidad cuando hay problemas inesperados. Se mide en segundos. |
| --service-health-policy | Mapa codificado en JSON con una directiva de mantenimiento de tipo de servicio por cada nombre de tipo de servicio. El mapa está vacío de forma predeterminada. |
| --timeout -t | Valor predeterminado\: 60. |
| --upgrade-domain-timeout | El período de tiempo del que dispone cada dominio de actualización para completarse antes de la ejecución de FailureAction.  Valor predeterminado\: P10675199DT02H48M05.4775807S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --upgrade-timeout | El período de tiempo en el que se debe completar la actualización general antes de que se ejecute FailureAction.  Valor predeterminado\: P10675199DT02H48M05.4775807S. <br><br> En primer lugar se interpreta como una cadena que representa una duración ISO 8601. Si se produce un error, se interpreta como un número que representa el total de milisegundos. |
| --warning-as-error | Indica si las advertencias se tratan con el mismo nivel de gravedad que los errores. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-upgrade-resume"></a>sfctl application upgrade-resume
Reanuda la actualización de una aplicación en el clúster de Service Fabric.

Reanuda la actualización de una aplicación de Service Fabric manual no supervisada. Service Fabric actualiza un dominio de actualización cada vez. Para las actualizaciones manuales no supervisadas, después de que Service Fabric finaliza un dominio de actualización, espera a que se llame a esta API antes de proceder al siguiente dominio de actualización.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --upgrade-domain-name [Obligatorio] | El nombre de dominio de actualización en la que se va a reanudar la actualización. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-upgrade-rollback"></a>sfctl application upgrade-rollback
Inicia la reversión de la actualización en curso de una aplicación en el clúster de Service Fabric.

Comienza a revertir la actualización de la aplicación actual a la versión anterior. Esta API solo se puede utilizar para revertir la actual actualización en curso que se está poniendo al día a la nueva versión. Si la aplicación no se está actualizando, utilice StartApplicationUpgrade API para actualizarla a la versión deseada, lo que incluye la reversión a una versión anterior.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-upgrade-status"></a>sfctl application upgrade-status
Obtiene los detalles de la última actualización de esta aplicación.

Devuelve información sobre el estado de la última actualización de la aplicación junto con detalles para ayudar a depurar los problemas de mantenimiento de la aplicación.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --application-id [Obligatorio] | La identidad de la aplicación. Este suele ser el nombre completo de la aplicación sin el esquema de URI "fabric\:". A partir de la versión 6.0, los nombres jerárquicos se delimitan con el carácter "\~". Por ejemplo, si el nombre de la aplicación es "fabric\:/myapp/app1", la identidad de la aplicación sería "myapp\~app1" en 6.0+ y "myapp/app1" en las versiones anteriores. |
| --timeout -t | Tiempo de espera del servidor para realizar la operación en segundos. Este tiempo de espera especifica el tiempo que el cliente está dispuesto a esperar a que se complete la operación solicitada. El valor predeterminado para este parámetro es 60 segundos.  Valor predeterminado\: 60. |

### <a name="global-arguments"></a>Argumentos globales

|Argumento|Descripción|
| --- | --- |
| --debug | Aumente el nivel de detalle de registro para mostrar todos los registros de depuración. |
| --help -h | Muestre este mensaje de ayuda y salga. |
| --output -o | Formato de salida.  Valores permitidos\: json, jsonc, table y tsv.  Valor predeterminado\: json. |
| --query | Cadena de consulta de JMESPath. Consulte http\://jmespath.org/ para obtener más información y ejemplos. |
| --verbose | Aumente el nivel de detalle de registro. Use --debug para obtener registros de depuración completos. |

## <a name="sfctl-application-upload"></a>sfctl application upload
Copia un paquete de aplicación de Service Fabric en el almacén de imágenes.

Opcionalmente, se muestra el progreso de carga de cada archivo en el paquete. El progreso de carga se envía a `stderr`.

### <a name="arguments"></a>Argumentos

|Argumento|Descripción|
| --- | --- |
| --path     [obligatorio] | Ruta de acceso al paquete de aplicación local. |
| --compress | Solo está disponible para paquetes de aplicación de Service Fabric. Cree una carpeta que contenga el paquete de aplicación comprimido en la ubicación predeterminada o en la ubicación especificada por el parámetro de ubicación comprimido y, luego, cargue la carpeta recién creada. <br><br> Si ya hay un archivo comprimido generado mediante sfctl, se sobrescribirá si se establece esta marca. Si el directorio no es un paquete de aplicación, se devolverá un error. Si ya es un paquete de aplicación comprimido, la carpeta se copiará tal cual. De forma predeterminada, el paquete de aplicación comprimido recién creado se eliminará después de una carga correcta. Si la carga no se realiza correctamente, limpie manualmente el paquete comprimido según sea necesario. La eliminación no quita los directorios vacíos que puedan haberse creado si el parámetro de ubicación comprimida hace referencia a directorios que no existen. |
| --compressed-location | Ubicación en la que se va a colocar el paquete de aplicación comprimido. <br><br> Si no se proporciona ninguna ubicación, el paquete comprimido se colocará en una carpeta recién creada llamada sfctl_compressed_temp en el directorio principal especificado en el argumento de ruta de acceso. Por ejemplo, si el argumento de ruta de acceso tiene el valor C\:/FolderA/AppPkg, el paquete comprimido se agregará a C\:/FolderA/sfctl_compressed_temp/AppPkg. |
| --imagestore-string | Almacén de imágenes de destino en el que cargar el paquete de aplicación.  Predeterminado\: fabric\:ImageStore. <br><br> Para la carga en una ubicación de archivo, inicie este parámetro con "file\:". De lo contrario, el valor debe ser la cadena de conexión del almacén de imágenes, como el valor predeterminado. |
| --keep-compressed | Indica si se debe mantener el paquete comprimido generado tras la finalización correcta de la carga. <br><br> Si no se establece, cuando la carga finalice correctamente, se eliminarán los paquetes de aplicación comprimidos. Si la carga no se realiza correctamente, el paquete de aplicación siempre se conservará en el directorio de salida para volver a cargarlo. |
| --show-progress | Muestra el progreso de la carga de archivos para paquetes grandes. |
| --timeout -t | Tiempo de espera total en segundos. La carga no se realizará correctamente y devolverá un error una vez transcurrido el tiempo de espera de carga. Este tiempo de espera se aplica a todo el paquete de la aplicación y los tiempos de espera de archivos individuales serán iguales a la duración del tiempo de espera restante. El tiempo de espera no incluye el tiempo necesario para comprimir el paquete de aplicación.  Valor predeterminado\: 300 |

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
