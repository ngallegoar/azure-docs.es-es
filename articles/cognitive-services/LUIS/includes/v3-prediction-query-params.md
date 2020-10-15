---
title: Parámetros de cadena de consulta de API v3
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 06/30/2020
ms.openlocfilehash: 14380dd781fc1eebfe7edb0a816ff8af0f2f17b4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91309442"
---
Los parámetros de cadena de consulta de API v3 incluyen:

|Parámetro de consulta|Nombre del portal de LUIS|Tipo|Versión|Valor predeterminado|Propósito|
|--|--|--|--|--|--|
|`log`|Guardar registros|boolean|v2 y v3|false|Almacena la consulta en el archivo de registro. El valor predeterminado es False.|
|`query`|-|string|Solo v3|No tiene valor predeterminado: es obligatorio en la solicitud GET.|**En la versión 2**, la expresión que va a predecirse se almacena en el parámetro `q`. <br><br>**En la versión 3**, la funcionalidad se pasa en el parámetro `query`.|
|`show-all-intents`|Incluir puntuaciones para todas las intenciones|boolean|Solo v3|false|Devuelve todas las intenciones con la puntuación correspondiente en el objeto **prediction.intents**. Las intenciones se devuelven como objetos en un objeto `intents` primario. Esto habilita el acceso mediante programación sin necesidad de encontrar la intención en una matriz: `prediction.intents.give`. En la versión 2, se devuelven en una matriz. |
|`verbose`|Incluir más detalles de entidades|boolean|v2 y v3|false|**En la versión 2**, cuando se establece en true, se devuelven todas las intenciones previstas. Si necesita todas las intenciones previstas, use el parámetro v3 de `show-all-intents`.<br><br>**En la versión 3**, este parámetro solo proporciona detalles de los metadatos de la entidad para la predicción de entidad.  |
|`timezoneOffset`|-|string|V2|-|Zona horaria aplicada a las entidades datetimeV2.|
|`datetimeReference`|-|string|V3|-|[Zona horaria](../luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) aplicada a las entidades datetimeV2. Reemplaza `timezoneOffset` de la versión 2.|