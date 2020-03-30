---
title: Modificación de datos (LUIS)
description: Obtenga información sobre cómo se pueden modificar los datos antes de las predicciones en Language Understanding (LUIS)
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: b3b36351a64a4e1a0bd13d5785a4e0609a80901d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292075"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Modificación de datos de expresiones antes o durante la predicción
LUIS proporciona distintos métodos para manipular la expresión antes o durante la predicción. Entre ellos se incluyen la [corrección de la ortografía](luis-tutorial-bing-spellcheck.md) y la solución de los problemas de la zona horaria para elementos [datetimeV2](luis-reference-prebuilt-datetimev2.md) creados previamente.

## <a name="correct-spelling-errors-in-utterance"></a>Corregir errores de ortografía de una expresión


### <a name="v3-runtime"></a>Runtime V3

Preprocese el texto por correcciones ortográficas antes de enviar la expresión a LUIS. Use expresiones de ejemplo con la ortografía correcta para asegurarse de obtener las predicciones correctas.

Utilice [Bing Spell Check](../bing-spell-check/overview.md) para corregir el texto antes de enviarlo a LUIS.

### <a name="prior-to-v3-runtime"></a>Antes de runtime V3

LUIS usa [Bing Spell Check API V7](../Bing-Spell-Check/overview.md) para corregir los errores de ortografía de la expresión. LUIS necesita la clave asociada a ese servicio. Cree la clave y agréguela como parámetro de cadena de consulta en el [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356).

El punto de conexión requiere dos parámetros para que las correcciones ortográficas funcionen:

|Parámetro|Value|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|Clave de punto de conexión de [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/)|

Cuando [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecta un error, se devuelven la expresión original y la expresión corregida junto con las predicciones del punto de conexión.

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

```JSON
{
    "query": "Book a flite to London?",
    "prediction": {
        "normalizedQuery": "book a flight to london?",
        "topIntent": "BookFlight",
        "intents": {
            "BookFlight": {
                "score": 0.780123
            }
        },
        "entities": {},
    }
}
```

* * *

### <a name="list-of-allowed-words"></a>Lista de palabras permitidas
Bing Spell Check API que se utiliza en LUIS no admite una lista de palabras para omitir durante las alteraciones de la corrección ortográfica. Si necesita admitir una lista de palabras o acrónimos, procese la expresión en la aplicación cliente antes de enviar la expresión a LUIS para la predicción de intenciones.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Cambiar la zona horaria de la entidad datetimeV2 creada previamente
Cuando una aplicación de LUIS usa la entidad [datetimeV2](luis-reference-prebuilt-datetimev2.md) creada previamente, se puede devolver un valor de fecha y hora en la respuesta de la predicción. La zona horaria de la solicitud se usa para determinar la fecha y hora correctas que se van a devolver. Si la solicitud procede de un bot o de otra aplicación centralizada antes de llegar a LUIS, corrija la zona horaria que usa LUIS.

### <a name="endpoint-querystring-parameter"></a>Parámetro de cadena de consulta de punto de conexión
La zona horaria se puede corregir agregando la zona horaria del usuario al [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356) mediante el parámetro `timezoneOffset`. El valor de `timezoneOffset` debe ser un número positivo o negativo (en minutos) para modificar la hora.

|Parámetro|Value|
|--|--|
|`timezoneOffset`|número positivo o negativo (en minutos)|

### <a name="daylight-savings-example"></a>Ejemplo de horario de verano
Si necesita que la entidad datetimeV2 creada previamente que se ha devuelto se ajuste al horario de verano, debe usar el parámetro de cadena de consulta `timezoneOffset` con un valor +/- en minutos para la consulta del [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356).

#### <a name="v2-prediction-endpoint-request"></a>[Solicitud de punto de conexión de predicción de V2](#tab/V2)

Agregar 60 minutos:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

Quitar 60 minutos:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=-60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

#### <a name="v3-prediction-endpoint-request"></a>[Solicitud de punto de conexión de predicción de V3](#tab/V3)

Agregar 60 minutos:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

Quitar 60 minutos:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=-60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>El código de C# determina el valor correcto de timezoneOffset
El siguiente código de C# usa el método [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) de la clase [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) para determinar el `timezoneOffset` correcto según la hora del sistema:

```csharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Corregir errores de ortografía con este tutorial](luis-tutorial-bing-spellcheck.md)
