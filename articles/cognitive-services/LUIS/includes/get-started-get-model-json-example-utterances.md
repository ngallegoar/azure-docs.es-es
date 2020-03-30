---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: e879afdbd4c34e9d74405644de86421fb2cbab46
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77279727"
---
Las expresiones del ejemplo siguen un formato específico.

El campo `text` contiene el texto de la expresión de ejemplo. El campo `intentName` debe corresponder al nombre de una intención existente en la aplicación de LUIS. El campo `entityLabels` es obligatorio. Si no desea etiquetar ninguna entidad, proporcione una matriz vacía.

Si la matriz entityLabels no está vacía, `startCharIndex` y `endCharIndex` tienen que marcar la entidad a la que se hace referencia en el campo `entityName`. El índice está basado en cero, lo que significa que 6 en el ejemplo superior hace referencia a la "S" de Seattle y no al espacio antes de la S mayúscula. Si comienza o termina la etiqueta en un espacio en el texto, se produce un error en la llamada de API para agregar las expresiones.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```
