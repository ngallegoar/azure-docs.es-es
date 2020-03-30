---
title: Creación de funciones definidas por el usuario en Azure Digital Twins | Microsoft Docs
description: Cómo crear funciones definidas por el usuario, buscadores de coincidencias y asignaciones de roles con Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: 232d85789c25e905873286eba6fda32c327a6e25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276934"
---
# <a name="how-to-create-user-defined-functions-in-azure-digital-twins"></a>Creación de funciones definidas por el usuario en Azure Digital Twins

[Las funciones definidas por el usuario](./concepts-user-defined-functions.md) permiten a los usuarios configurar una lógica personalizada para que se ejecute en los mensajes de telemetría entrantes y los metadatos de grafos espaciales. Los usuarios pueden enviar eventos a [puntos de conexión](./how-to-egress-endpoints.md) predefinidos.

En esta guía se muestra un ejemplo que demuestra cómo detectar y notificar sobre cualquier lectura que supere una temperatura determinada en los eventos de temperatura recibidos.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Referencia de la biblioteca cliente

Las funciones que están disponibles como métodos auxiliares en el tiempo de ejecución de las funciones definidas por el usuario se enumeran en el documento [referencia de la biblioteca cliente](./reference-user-defined-functions-client-library.md).

## <a name="create-a-matcher"></a>Creación de un buscador de coincidencias

Los buscadores de coincidencias son objetos gráficos que determinan qué funciones definidas por el usuario se ejecutan para un mensaje de telemetría determinado.

- Comparaciones de condiciones válidas para el buscador de coincidencias:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Destinos de condiciones válidas para el buscador de coincidencias:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

El buscador de coincidencias del ejemplo siguiente evalúa como true a cualquier evento de telemetría de sensores con `"Temperature"` como su valor de tipo de datos. Para crear varios buscadores de coincidencias en una función definida por el usuario, realice una solicitud HTTP POST autenticada a:

```URL
YOUR_MANAGEMENT_API_URL/matchers
```

Con el cuerpo JSON:

```JSON
{
  "id": "3626464-f39b-46c0-d9b0c-436aysj55",
  "name": "Temperature Matcher",
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "conditions": [
    {
      "id": "ag7gq35cfu3-e15a-4e9c-6437-sj6w68sy44s",
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ]
}
```

| Value | Reemplazar por |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | La región de servidor en la que está hospedada la instancia |

## <a name="create-a-user-defined-function"></a>Creación de una función definida por el usuario

La creación de una función definida por el usuario implica la realización de una solicitud HTTP de varias partes a las API de Administración de Azure Digital Twins.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

Tras la creación de los buscadores de coincidencias, cargue el fragmento de código de la función con la siguiente solicitud autenticada HTTP POST de varias partes a:

```URL
YOUR_MANAGEMENT_API_URL/userdefinedfunctions
```

Use el siguiente cuerpo:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "name": "User Defined Function",
  "description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Value | Reemplazar por |
| --- | --- |
| USER_DEFINED_BOUNDARY | Nombre de un límite de contenido de varias partes |
| YOUR_SPACE_IDENTIFIER | El identificador de espacio  |
| YOUR_MATCHER_IDENTIFIER | El identificador del buscador de coincidencias que quiere usar |

1. Compruebe que los encabezados incluyan: `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
1. Compruebe que el cuerpo esté formado por varias partes:

   - La primera parte contiene los metadatos necesarios de función definida por el usuario.
   - La segunda contiene la lógica de ejecución de JavaScript.

1. En la sección **USER_DEFINED_BOUNDARY**, reemplace los valores de **spaceId** (`YOUR_SPACE_IDENTIFIER`) y **matchers** (`YOUR_MATCHER_IDENTIFIER`).
1. Compruebe que la función definida por el usuario de JavaScript se proporcione como `Content-Type: text/javascript`.

### <a name="example-functions"></a>Funciones de ejemplo

Se establece la lectura de telemetría del sensor directamente con el tipo de datos **Temperature**, que es `sensor.DataType`:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

El parámetro **telemetry** expone los atributos **SensorId** y **Message**, que corresponden a un mensaje enviado por un sensor. El parámetro **executionContext** expone los siguientes atributos:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

En el ejemplo siguiente, registramos un mensaje si la lectura de telemetría del sensor supera un umbral definido previamente. Si la configuración de diagnóstico está habilitada en la instancia de Azure Digital Twins, también se reenvían los registros de las funciones definidas por el usuario:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

El código siguiente desencadena una notificación si el nivel de temperatura supera la constante predefinida:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Para obtener un ejemplo más complejo de código de función definida por el usuario, consulte la [Guía de inicio rápido sobre ocupación](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Crear una asignación de rol

Cree una asignación de roles para que se ejecute la función definida por el usuario. Si no existe ninguna asignación de roles para la función definida por el usuario, no tendrá los permisos adecuados para interactuar con la API de Administración ni tendrá acceso para realizar acciones en objetos gráficos. Las acciones que puede realizar la función definida por el usuario se especifican y definen a través del control de acceso basado en roles dentro de las API de Administración de Azure Digital Twins. Por ejemplo, las funciones definidas por el usuario pueden limitar su ámbito mediante la especificación de determinados roles o rutas de control de acceso. Para obtener más información, lea la documentación de [Control de acceso basado en rol](./security-role-based-access-control.md).

1. [Consulte la API del sistema](./security-create-manage-role-assignments.md#retrieve-all-roles) para que todos los roles obtengan el identificador de rol que desea asignar a la función definida por el usuario. Para ello, realice una solicitud HTTP GET autenticada a:

    ```URL
    YOUR_MANAGEMENT_API_URL/system/roles
    ```
   Mantenga el identificador de rol que desee. Se pasará como el atributo de cuerpo JSON **roleId** (`YOUR_DESIRED_ROLE_IDENTIFIER`) a continuación.

1. **objectId** (`YOUR_USER_DEFINED_FUNCTION_ID`) será el identificador de la función definida por el usuario que se creó anteriormente.
1. Busque el valor de **path** (`YOUR_ACCESS_CONTROL_PATH`) mediante la realización de una consulta de los espacios con `fullpath`.
1. Copie el valor de `spacePaths` devuelto, ya que lo utilizará a continuación. Realice una solicitud HTTP GET autenticada a:

    ```URL
    YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Value | Reemplazar por |
    | --- | --- |
    | YOUR_SPACE_NAME | El nombre del espacio que quiere usar |

1. Pegue el valor `spacePaths` devuelto en **path** para crear una asignación de roles de función definida por el usuario mediante una solicitud HTTP POST autenticada a:

    ```URL
    YOUR_MANAGEMENT_API_URL/roleassignments
    ```
    Con el cuerpo JSON:

    ```JSON
    {
      "roleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "objectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "objectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Value | Reemplazar por |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | El identificador para el rol deseado |
    | YOUR_USER_DEFINED_FUNCTION_ID | El identificador de la función definida por el usuario que desea usar |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | El identificador que especifica el tipo de función definida por el usuario (`UserDefinedFunctionId`). |
    | YOUR_ACCESS_CONTROL_PATH | La ruta del control de acceso |

>[!TIP]
> Lea el artículo [Creación y administración de asignación de roles](./security-create-manage-role-assignments.md) para obtener más información sobre las operaciones de API de Administración de la función definida por el usuario y los puntos de conexión.

## <a name="send-telemetry-to-be-processed"></a>Envío de la telemetría a procesarse

El sensor definido en el gráfico de inteligencia espacial envía telemetría. A su vez, la telemetría desencadena la ejecución de la función definida por el usuario que se cargó. El procesador de datos recoge los datos de telemetría. A continuación, se crea un plan de ejecución para invocar la función definida por el usuario.

1. Recuperar los buscadores de coincidencias del sensor del que se generó la lectura.
1. En función de qué buscadores de coincidencia se evalúen correctamente, recuperar las funciones definidas por el usuario asociadas.
1. Ejecutar cada función definida por el usuario.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear puntos de conexión de Azure Digital Twins](./how-to-egress-endpoints.md) a los que enviar eventos.

- Para obtener más información sobre el enrutamiento en Azure Digital Twins, consulte [Enrutamiento de eventos y mensajes](./concepts-events-routing.md).

- Revise la [documentación de referencia de biblioteca de cliente](./reference-user-defined-functions-client-library.md).