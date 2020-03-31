---
title: 'Patrones de consulta comunes: Azure Digital Twins | Microsoft Docs'
description: Conozca varios patrones comunes de consulta de API para las API de administración de Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 133c0e0dcc07afb85a0f3af9ae51d2207abac293
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589120"
---
# <a name="how-to-query-azure-digital-twins-apis-for-common-tasks"></a>Cómo consultar las API de Azure Digital Twins para tareas comunes

En este artículo se muestran los patrones de consulta que le ayudan a ejecutar escenarios comunes para la instancia de Azure Digital Twins. Se da por hecho que su instancia de Digital Twins ya está en ejecución. Puede usar a cualquier cliente de REST, como Postman. 

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]


## <a name="queries-for-spaces-and-types"></a>Consultas de espacios y tipos

En esta sección se muestran consultas de ejemplo para obtener más información sobre los espacios aprovisionados. Realice solicitudes HTTP GET autenticadas con las consultas de ejemplo, y reemplace los marcadores de posición por valores de su configuración. 

- Obtenga espacios que son nodos raíz.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?$filter=ParentSpaceId eq null
    ```

- Obtenga un espacio por su nombre e incluya dispositivos, sensores, valores calculados y valores de sensor. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=Focus Room A1&includes=fullpath,devices,sensors,values,sensorsvalues
    ```

- Obtenga espacios y su información de dispositivo/sensor, cuyo elemento principal es el identificador de espacio especificado, y que se encuentran en los niveles del dos al cinco [en relación con el espacio especificado](how-to-navigate-apis.md#api-navigation). 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?spaceId=YOUR_SPACE_ID&includes=fullpath,devices,sensors,values,sensorsvalues&traverse=Down&minLevel=1&minRelative=true&maxLevel=5&maxRelative=true
    ```

- Obtenga el espacio con el identificador dado e incluya valores de sensor y calculados.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?ids=YOUR_SPACE_ID&includes=Values,sensors,SensorsValues
    ```

- Obtenga las claves de propiedad de un determinado espacio.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/propertykeys?spaceId=YOUR_SPACE_ID
    ```

- Obtenga espacios con la clave de propiedad denominada *AreaInSqMeters* y cuyo valor sea 30. También puede realizar operaciones de cadena, por ejemplo, obtener espacios que contengan la clave de propiedad con `name = X contains Y`.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?propertyKey=AreaInSqMeters&propertyValue=30
    ```

- Obtenga todos los nombres con el nombre *Temperature* y las dependencias y ontologías asociadas.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/types?names=Temperature&includes=space,ontologies,description,fullpath
    ```


## <a name="queries-for-roles-and-role-assignments"></a>Consulta de roles y asignaciones de roles

En esta sección se muestran algunas consultas para obtener más información sobre los roles y sus asignaciones. 

- Obtenga todos los roles compatibles con Azure Digital Twins.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```

- Obtenga todas las asignaciones de roles de su instancia de Digital Twins. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=down
    ```

- Obtenga las asignaciones de roles en una ruta de acceso determinada.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments?path=/A_SPATIAL_PATH
    ```

## <a name="queries-for-devices"></a>Consultas de dispositivos

En esta sección se muestran algunos ejemplos de cómo puede usar las API de administración para obtener información específica sobre sus dispositivos. Todas las llamadas de API deben ser solicitudes HTTP GET autenticadas.

- Obtenga todos los dispositivos.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices
    ```

- Busque todos los estados de dispositivo.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/devices/statuses
    ```

- Obtenga un dispositivo específico.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID
    ```

- Obtenga todos los dispositivos asociados al espacio raíz.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?maxLevel=1
    ```

- Obtenga todos los dispositivos asociados a espacios en los niveles de 2 a 4.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4
    ```

- Obtenga todos los dispositivos asociados directamente a un identificador de espacio determinado.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID
    ```

- Obtenga todos los dispositivos asociados a un espacio concreto y sus descendientes.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down
    ```

- Obtenga todos los dispositivos asociados a los descendientes de un espacio, excepto ese espacio.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true
    ```

- Obtenga todos los dispositivos asociados a elementos secundarios directos de un espacio.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true
    ```

- Obtenga todos los dispositivos asociados a uno de los antecesores de un espacio.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Up&maxLevel=-1&maxRelative=true
    ```

- Obtenga todos los dispositivos asociados a los descendientes de un espacio que sean de nivel inferior o igual a 5.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Down&maxLevel=5
    ```

- Obtenga todos los dispositivos asociados a espacios que se encuentran en el mismo nivel que el espacio con el identificador *YOUR_SPACE_ID*.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?spaceId=YOUR_SPACE_ID&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true
    ```

- Obtenga la cadena de conexión de dispositivos de IoT Hub para su dispositivo.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_ID?includes=ConnectionString
    ```

- Obtenga el dispositivo con el identificador de hardware dado, incluidos los sensores asociados.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/devices?hardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=sensors
    ```

- Obtenga los sensores de tipos de datos determinados, en este caso *Motion* y *Temperature*.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/sensors?dataTypes=Motion,Temperature
    ```

## <a name="queries-for-matchers-and-user-defined-functions"></a>Consultas de buscadores de coincidencias y funciones definidas por el usuario 

- Obtenga todos los buscadores de coincidencias aprovisionados y sus identificadores.

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers
    ```

- Obtenga detalles sobre un buscador de coincidencias determinado, incluidos los espacios y la función definida por el usuario asociada a él.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID?includes=description, conditions, fullpath, userdefinedfunctions, space
    ```

- Evalúe un buscador de coincidencias con un sensor y habilite el registro con fines de depuración. La devolución de este mensaje GET HTTP indica si el buscador de coincidencias y el sensor pertenecen al tipo de datos. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_ID/evaluate/YOUR_SENSOR_ID?enableLogging=true
    ```

- Obtenga el identificador de las funciones definidas por el usuario. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions
    ```

- Obtenga el contenido de una determinada función definida por el usuario. 

   ```plaintext
    YOUR_MANAGEMENT_API_URL/userdefinedfunctions/YOUR_USER_DEFINED_FUNCTION_ID/contents
    ```


## <a name="queries-for-users"></a>Consultas de usuarios

En esta sección se muestran algunas consultas de API de ejemplo para administrar usuarios en Azure Digital Twins. Realice una solicitud GET HTTP y reemplace los marcadores de posición por valores de su configuración. 

- Obtenga todos los usuarios. 

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users
    ```

- Obtenga un usuario específico.

    ```plaintext
    YOUR_MANAGEMENT_API_URL/users/ANY_USER_ID
    ```

## <a name="next-steps"></a>Pasos siguientes

Para aprender cómo autenticar con la API de administración, lea [Autenticación con las API](./security-authenticating-apis.md).

Para más información sobre los puntos de conexión de API, lea [Uso de Digital Twins Swagger](./how-to-use-swagger.md).
