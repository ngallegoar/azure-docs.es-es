---
title: 'Navegación por las API: Azure Digital Twins | Microsoft Docs'
description: Conozca los patrones comunes de consultar las API de administración de Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: e9cdfd40a9672d19ef32dede0baadcdd56266bab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231376"
---
# <a name="how-to-use-azure-digital-twins-management-apis"></a>Cómo usar las API de administración de Azure Digital Twins

Las API de administración de Azure Digital Twins proporcionan funcionalidades eficaces para las aplicaciones de IoT. En este artículo se muestra cómo desplazarse por la estructura de API.  

## <a name="api-summary"></a>API summary

En la lista siguiente se muestran los componentes de las API de Digital Twins.

* [/spaces](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces): estas API interactúan con las ubicaciones físicas de su instalación. Ayudan a crear, eliminar y administrar las asignaciones digitales de sus ubicaciones físicas en forma de un [grafo espacial](concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph).

* [/devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices): estas API interactúan con los dispositivos de su instalación. Estos dispositivos pueden administrar uno o varios sensores. Por ejemplo, un dispositivo podría ser su teléfono, un pod de sensor Raspberry Pi, una puerta de enlace Lora, etc.

* [/sensors](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Sensors): estas API le ayudan a comunicarse con los sensores asociados con los dispositivos y sus ubicaciones físicas. Los sensores registran y envían valores ambientales que luego se pueden usar para manipular el entorno espacial.  

* [/resources](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Resources): estas API ayudan a configurar los recursos, como un centro de IoT, para su instancia de Digital Twins.

* [/types](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Types): estas API permiten asociar tipos extendidos a objetos de Digital Twins, para agregar características específicas a esos objetos. Estos tipos permiten filtrar y agrupar de forma fácil los objetos de la interfaz de usuario y las funciones personalizadas que procesan los datos de telemetría. Ejemplos de tipos extendidos son *DeviceType*, *SensorType*, *SensorDataType*, *SpaceType*, *SpaceSubType* , *SpaceBlobType*, *SpaceResourceType*, etc.

* [/ontologies](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Ontologies): estas API le ayudan a administrar ontologías, que son colecciones de tipos extendidos. Las ontologías proporcionan nombres para tipos de objetos según el espacio físico que representan. Así por ejemplo, la ontología *BACnet* proporciona nombres específicos para *sensor types*, *datatypes*, *datasubtypes* y *dataunittypes*. Las ontologías se administran y crean por el servicio. Los usuarios pueden cargar y descargar ontologías. Cuando se carga una ontología, todos sus nombres de tipo asociados se habilitan y están preparados para aprovisionarse en el grafo espacial. 

* [/propertyKeys](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/PropertyKeys): puede usar estas API para crear propiedades personalizadas para sus *espacios*, *dispositivos*, *usuarios* y *sensores*. Estas propiedades se crean como pares clave-valor. Para definir el tipo de datos para estas propiedades, puede establecer su valor de *PrimitiveDataType*. Por ejemplo, puede definir una propiedad llamada *BasicTemperatureDeltaProcessingRefreshTime* de tipo *uint* para los sensores y, luego, asignar un valor a esta propiedad para cada uno de los sensores. También puede agregar restricciones para estos valores durante la creación de la propiedad, como los intervalos *Min* (Mín) y *Max* (Máx), así como valores permitidos como *ValidationData*.

* [/matchers](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Matchers): estas API le permiten especificar las condiciones que se van a evaluar desde los datos del dispositivo de entrada. Para más información, consulte [este artículo](concepts-user-defined-functions.md#matchers). 

* [/userDefinedFunctions](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/UserDefinedFunctions): estas API permiten crear, eliminar o actualizar una función personalizada que se ejecutará cuando se produzcan las condiciones definidas por los *buscadores de coincidencias*, para procesar los datos procedentes de la instalación. Consulte [este artículo](concepts-user-defined-functions.md#user-defined-functions) para más información sobre estas funciones personalizadas, también llamadas *funciones definidas por el usuario*. 

* [/endpoints](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Endpoints): estas API permiten crear puntos de conexión de modo que la solución de Digital Twins se pueda comunicar con otros servicios de Azure para almacenar y analizar datos. Lea [este artículo](concepts-events-routing.md) para más información. 

* [/keyStores](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/KeyStores): estas API permiten administrar los almacenes de claves de seguridad para los espacios. Estos almacenes pueden contener una colección de claves de seguridad y le permiten recuperar fácilmente las claves válidas más recientes.

* [/users](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Users): estas API permiten asociar usuarios a los espacios, para localizar a estas personas cuando sea necesario. 

* [/system](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/System): estas API permiten administrar la configuración del sistema, como los tipos predeterminados de espacios y sensores. 

* [/roleAssignments](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/RoleAssignments): estas API permiten asociar roles a entidades, como el identificador de usuario, el identificador de función definida por el usuario, etc. Cada asignación de roles incluye el identificador de la entidad para asociar, el tipo de entidad, el identificador del rol para asociar, el identificador del inquilino y una ruta de acceso que define el límite superior del recurso al que puede acceder la entidad con esa asociación. Lea [este artículo](security-role-based-access-control.md) para más información.


## <a name="api-navigation"></a>Desplazamiento por las API

Las API de Digital Twins admiten el filtrado y la navegación por el grafo espacial con los parámetros siguientes:

- **spaceId**: la API filtrará los resultados por el identificador de espacio especificado. Además, la marca booleana **useParentSpace** es aplicable a las API [/espacios](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces), lo que indica que el identificador de espacio especificado hace referencia al espacio primario en lugar de al espacio actual. 

- **minLevel** y **maxLevel**: los espacios raíz se consideran en el nivel 1. Los espacios con el espacio primario en el nivel *n* están en el nivel *n+1*. Con estos valores establecidos, puede filtrar los resultados en niveles específicos. Estos valores son inclusivos cuando se establecen. Los dispositivos, sensores y otros objetos se consideran del mismo nivel que su espacio más cercano. Para obtener todos los objetos de un nivel determinado, establezca **minLevel** y **maxLevel** en el mismo valor.

- **minRelative** y **maxRelative**: cuando se proporcionan estos filtros, el nivel correspondiente es relativo al nivel del identificador de espacio especificado:
   - El nivel relativo *0* es el mismo nivel que el identificador de espacio especificado.
   - El nivel relativo *1* representa los espacios en el mismo nivel que los elementos secundarios del identificador de espacio especificado. El nivel relativo *n* representa espacios menores que el espacio especificado por *n* niveles.
   - El nivel relativo *-1* representa espacios en el mismo nivel que el espacio primario del espacio especificado.

- **traverse**: le permite desplazarse en cualquier dirección desde un identificador de espacio dado, según lo especificado por los siguientes valores.
   - **Ninguna**: este valor predeterminado filtra hasta el identificador de espacio especificado.
   - **Down** (Abajo): este valor filtra por el identificador de espacio especificado y sus descendientes. 
   - **Up** (Arriba): este valor filtra por el identificador de espacio especificado y sus antecesores. 
   - **Span** (Intervalo): este valor filtra una parte horizontal del grafo espacial, en el mismo nivel que el identificador de espacio especificado. Para ello, es necesario establecer **minRelative** o **maxRelative** en true. 


### <a name="examples"></a>Ejemplos

En la lista siguiente se muestran algunos ejemplos de desplazamiento por las API [/devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices). Tenga en cuenta que el marcador de posición `YOUR_MANAGEMENT_API_URL` hace referencia al identificador URI de las API de Digital Twins en el formato `https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/api/v1.0/`, donde `YOUR_INSTANCE_NAME` es el nombre de la instancia de Azure Digital Twins y `YOUR_LOCATION` es la región donde se hospeda la instancia.

- `YOUR_MANAGEMENT_API_URL/devices?maxLevel=1` devuelve todos los dispositivos asociados a espacios raíz.
- `YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4` devuelve todos los dispositivos asociados a espacios de los niveles 2, 3 o 4.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId` devuelve todos los dispositivos asociados directamente a mySpaceId.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down` devuelve todos los dispositivos asociados a mySpaceId o uno de sus descendientes.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true` devuelve todos los dispositivos asociados a descendientes de mySpaceId, excepto mySpaceId.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true` devuelve todos los dispositivos asociados a elementos secundarios inmediatos de mySpaceId.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Up&maxLevel=-1&maxRelative=true` devuelve todos los dispositivos asociados a uno de los antecesores de mySpaceId.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&maxLevel=5` devuelve todos los dispositivos asociados a descendientes de mySpaceId que se encuentran en un nivel menor o igual a 5.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true` devuelve todos los dispositivos asociados a espacios que se encuentran en el mismo nivel que mySpaceId.


## <a name="odata-support"></a>Compatibilidad con OData

La mayoría de las API que devuelven colecciones, como una llamada GET en /spaces, admiten el subconjunto siguiente de las opciones de consulta genéricas del sistema de [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData):  

* **$filter**
* **$orderby** 
* **$top**
* **$skip**: si quiere mostrar toda la colección, debe solicitarla como un conjunto completo en una sola llamada y, luego, realizar la paginación en la aplicación. 

> [!NOTE]
> Algunas opciones de OData (como las opciones de consulta **$count**, **$expand** y **$search**) no se admiten actualmente.

### <a name="examples"></a>Ejemplos

En la lista siguiente se muestran varias consultas con sintaxis válida de OData:

- `YOUR_MANAGEMENT_API_URL/devices?$top=3&$orderby=Name desc`
- `YOUR_MANAGEMENT_API_URL/keystores?$filter=endswith(Description,'space')`
- `YOUR_MANAGEMENT_API_URL/devices?$filter=TypeId eq 2`
- `YOUR_MANAGEMENT_API_URL/resources?$filter=StatusId ne 1`
- `YOUR_MANAGEMENT_API_URL/users?$top=4&$filter=endswith(LastName,'k')&$orderby=LastName`
- `YOUR_MANAGEMENT_API_URL/spaces?$orderby=Name desc&$top=3&$filter=substringof('Floor',Name)`
 
## <a name="next-steps"></a>Pasos siguientes

Para conocer algunos patrones comunes de consulta de API, lea [How to query Azure Digital Twins APIs for common tasks](./how-to-query-common-apis.md) (Consulta de las API de Azure Digital Twins para tareas comunes).

Para más información sobre los puntos de conexión de API, lea [Uso de Digital Twins Swagger](./how-to-use-swagger.md).

Para revisar la sintaxis de OData y los operadores de comparación disponibles, lea [Operadores de comparación de OData en Azure Cognitive Search](../search/search-query-odata-comparison-operators.md).
