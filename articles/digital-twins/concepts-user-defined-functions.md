---
title: 'Procesamiento de datos y funciones definidas por el usuario: Azure Digital Twins | Microsoft Docs'
description: Introducción al procesamiento de datos, los buscadores de coincidencias y las funciones definidas por el usuario con Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 75ed2029582438ede43687addfd54c0a187e0120
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231380"
---
# <a name="data-processing-and-user-defined-functions"></a>Procesamiento de datos y funciones definidas por el usuario

Azure Digital Twins ofrece funcionalidades avanzadas de proceso. Los desarrolladores pueden definir y ejecutar funciones personalizadas en los mensajes de telemetría entrantes para enviar eventos a puntos de conexión predefinidos.

## <a name="data-processing-flow"></a>Flujo de procesamiento de datos

Una vez que los dispositivos envían datos de telemetría a Azure Digital Twins, los desarrolladores pueden procesar los datos en cuatro fases: *validación*, *coincidencia*, *proceso* y *distribución*.

[![Flujo de procesamiento de datos de Azure Digital Twins](media/concepts/digital-twins-data-processing-flow.png)](media/concepts/digital-twins-data-processing-flow.png#lightbox)

1. La fase de validación transforma el mensaje de telemetría entrante en un formato de [objeto de transferencia de datos](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) reconocido. Esta fase también ejecuta la validación de dispositivos y sensores.
1. La fase coincidencia busca las funciones definidas por el usuario adecuadas para ejecutarse. Los buscadores de coincidencias predefinidos buscan las funciones definidas por el usuario según la información del dispositivo, del sensor y del espacio a partir del mensaje entrante de telemetría.
1. La fase proceso ejecuta a las funciones definidas por el usuario coincidentes en la fase anterior. Estas funciones pueden leer y actualizar los valores calculados en los nodos de un grafo espacial y pueden emitir notificaciones personalizadas.
1. La fase de envío enruta las notificaciones personalizadas desde la fase de proceso a los puntos de conexión definidos en el grafo.

## <a name="data-processing-objects"></a>Objetos de procesamiento de datos

El procesamiento de datos en Azure Digital Twins consiste en definir tres objetos: *buscadores de coincidencias*, *funciones definidas por el usuario* y *asignaciones de roles*.

[![Objetos de procesamiento de datos de Azure Digital Twins](media/concepts/digital-twins-user-defined-functions.png)](media/concepts/digital-twins-user-defined-functions.png#lightbox)

### <a name="matchers"></a>Buscadores de coincidencias

Los buscadores de coincidencias definen un conjunto de condiciones que evalúan las acciones que se llevarán a cabo según la telemetría entrante del sensor. Las condiciones para determinar la coincidencia podrían incluir propiedades del sensor, el dispositivo primario del sensor y el espacio primario del sensor. Las condiciones se expresan como comparaciones frente a una [ruta de acceso JSON](https://jsonpath.com/), tal como se describe en este ejemplo:

- Todos los sensores de tipo de datos **Temperatura** representados por el valor de cadena de escape `\"Temperature\"`.
- Que tienen `01` en su puerto.
- Que pertenecen a dispositivos con la clave de propiedad extendida **Manufacturer** establecida en el valor de cadena de escape `\"Contoso\"`.
- Que pertenecen a los espacios del tipo especificado por la cadena de escape `\"Venue\"`.
- Que son descendientes del **primario SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

```JSON
{
  "id": "23535afafd-f39b-46c0-9b0c-0dd3892a1c30",
  "name": "My custom matcher",
  "spaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value Contoso and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "conditions": [
    {
      "id": "43898sg43-e15a-4e9c-abb8-2gw464364",
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "id": "wt3th44-e15a-35sg-seg3-235wf3ga463",
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "id": "735hs33-e15a-37jj-23532-db901d550af5",
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"Contoso\"",
      "comparison": "Equals"
    },
    {
      "id": "222325-e15a-49fg-5744-463643644",
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - Las rutas de acceso JSON distinguen entre mayúsculas y minúsculas.
> - La carga de JSON es la misma que la carga que devolvería:
>   - `/sensors/{id}?includes=properties,types` para el sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` para el dispositivo primario del sensor.
>   - `/spaces/{id}?includes=properties,types,location,timezone` para el espacio primario del sensor.
> - Las comparaciones distinguen entre mayúsculas y minúsculas.

### <a name="user-defined-functions"></a>Funciones definidas por el usuario

Una función definida por el usuario es una función personalizada que se ejecuta en un entorno aislado en Azure Digital Twins. Las funciones definidas por el usuario tienen acceso al mensaje de telemetría sin formato del sensor, que se ha recibido. También tienen acceso al grafo espacial y al servicio de distribuidor. Una vez que se registra la función definida por el usuario dentro del grafo, debe crearse un buscador de coincidencias (detallado [anteriormente](#matchers)) para especificar cuándo se debe ejecutar dicha función. Por ejemplo, cuando Azure Digital Twins recibe nuevos datos de telemetría de un sensor determinado, la función definida por el usuario coincidente puede calcular una media móvil de las últimas lecturas del sensor.

Las funciones definidas por el usuario se pueden escribir en JavaScript. Los métodos auxiliares interactúan con el grafo en el entorno de ejecución definido por el usuario. Los desarrolladores pueden ejecutar fragmentos de código personalizados en relación con los mensajes de telemetría de los sensores. Algunos ejemplos son:

- Establecer la lectura del sensor directamente en el objeto de sensor dentro del grafo.
- Realizar una acción basada en las lecturas de distintos sensores dentro de un espacio en el grafo.
- Crear una notificación cuando se cumplen ciertas condiciones para un lectura entrante del sensor.
- Asociar metadatos del grafo a la lectura del sensor antes de enviar una notificación.

Para obtener más información, consulte [Uso de funciones definidas por el usuario](./how-to-user-defined-functions.md).

#### <a name="examples"></a>Ejemplos

El [repositorio de GitHub para el ejemplo de C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp/) contiene algunos ejemplos de las funciones definidas por el usuario:
- [Esta función](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) busca dióxido de carbono, movimiento y valores de temperatura para determinar si una sala está disponible con estos valores del intervalo. En los [tutoriales para Digital Twins](tutorial-facilities-udf.md) se explora esta función de forma más detallada. 
- [Esta función](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/multiplemotionsensors.js) busca datos de varios sensores de movimiento y determina la disponibilidad del espacio si ninguno de ellos detecta movimiento alguno. Puede reemplazar con facilidad la función definida por el usuario usada en la [guía de inicio rápido](quickstart-view-occupancy-dotnet.md) o en los [tutoriales](tutorial-facilities-setup.md), realizando los cambios mencionados en la sección de comentarios del archivo. 

### <a name="role-assignment"></a>Asignación de roles

Las acciones de una función definida por el usuario están sujetas al [control de acceso basado en rol](./security-role-based-access-control.md) de Azure Digital Twins para proteger los datos dentro del servicio. Las asignaciones de roles definen las funciones definidas por el usuario que tienen los permisos adecuados para interactuar con el grafo espacial y sus entidades. Por ejemplo, una función definida por el usuario podría tener la capacidad y el permiso para *CREAR*, *LEER*, *ACTUALIZAR* o *ELIMINAR* datos del grafo en un espacio determinado. El nivel de acceso de una función definida por el usuario se comprueba cuando esta solicita datos al grafo o intenta una acción. Para obtener más información, consulte [Control de acceso basado en rol](./security-create-manage-role-assignments.md).

Es posible que un buscador de coincidencias desencadene una función definida por el usuario que no tiene ninguna asignación de roles. En este caso, la función definida por el usuario no podría leer datos del grafo.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo enrutar eventos y mensajes de telemetría a otros servicios de Azure, lea [Enrutamiento de eventos y mensajes](./concepts-events-routing.md).

- Para más información sobre cómo crear buscadores de coincidencias, funciones definidas por el usuario y asignaciones de roles, lea la [guía para la creación de funciones definidas por el usuario](./how-to-user-defined-functions.md).

- Revise la [documentación de referencia de biblioteca de cliente de funciones definidas por el usuario](./reference-user-defined-functions-client-library.md).
