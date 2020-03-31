---
title: 'Moderación del texto mediante Text Moderation API: Content Moderator'
titleSuffix: Azure Cognitive Services
description: Pruebe la moderación de texto mediante Text Moderation API en la consola en línea.
services: cognitive-services
author: PatrickFarley
ms.author: pafarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: e0930558f31b27a77fa2cd6b44fcea2fe9091086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74538823"
---
# <a name="moderate-text-from-the-api-console"></a>Moderación de texto desde la consola de API

Use la [API de moderación de texto](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) de Azure Content Moderator para examinar el texto, detectar las blasfemias y compararlo con listas personalizadas y compartidas.

## <a name="get-your-api-key"></a>Obtener la clave de API

Antes de probar la API en la consola en línea, necesita la clave de suscripción. Se encuentra en la pestaña **Configuración**, en el cuadro **Ocp-Apim-Subscription-Key**. Para más información, consulte [Información general](overview.md).

## <a name="navigate-to-the-api-reference"></a>Navegar hasta la referencia de API

Vaya a la [referencia de Text Moderation API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  Se abre la página **Text - Screen** (Texto: filtrar).

## <a name="open-the-api-console"></a>Abrir la consola de API

En **Open API testing console** (Abrir consola de pruebas de API), seleccione la región que mejor describa su ubicación. 

  ![Selección de la región en la página Text - Screen (Texto: filtrar)](images/test-drive-region.png)

  Se abre la consola de API de **Text - Screen** (Texto: filtrar).

## <a name="select-the-inputs"></a>Seleccionar las entradas

### <a name="parameters"></a>Parámetros

Seleccione los parámetros de consulta que quiere usar en la pantalla de texto. En este ejemplo, utilice el valor predeterminado de **language** (idioma). También puede dejarlo en blanco, ya que la operación detectará automáticamente el idioma probable como parte de su ejecución.

> [!NOTE]
> Para el parámetro **language** (idioma), asigne `eng` o déjelo en blanco para ver la respuesta **classification** (clasificación) asistida por máquina. **Esta característica solo admite inglés**.
>
> Para la detección de **palabras soeces**, use el [código ISO 639-3](http://www-01.sil.org/iso639-3/codes.asp) de los idiomas admitidos que se enumeran en este artículo, o deje el campo en blanco.

Para **autocorrect** (autocorrección), **PII** (DCP) y **classify (preview)** [clasificar (versión preliminar)], seleccione **true**. Deje el campo **ListId** vacío.

  ![Parámetros de la consulta de la consola Text - Screen (Texto: filtrar)](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>Tipo de contenido

Para **Content-Type**, seleccione el tipo de contenido que quiera filtrar. Para este ejemplo, use el tipo predeterminado **text/plain** (texto/sin formato). En el cuadro **Ocp-Apim-Subscription-Key**, especifique la clave de suscripción.

### <a name="sample-text-to-scan"></a>Texto de ejemplo que se va a examinar

En el cuadro **Request body** (Cuerpo de la solicitud), escriba algo de texto. En el ejemplo siguiente se muestra un error intencionado en el texto.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="analyze-the-response"></a>Análisis de la respuesta

La respuesta siguiente muestra las distintas conclusiones desde la API. Contiene posibles palabras soeces, datos personales, clasificación (versión preliminar) y la versión corregida automáticamente.

> [!NOTE]
> La característica "Classification" (Clasificación) asistida por máquina está en versión preliminar y solo admite inglés.

```json
{"OriginalText":"Is this a grabage or crap email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555.\r\nThese are all UK phone numbers: +44 123 456 7890 or 0234 567 8901 or 0456 789 0123.\r\nAlso, 999-99-9999 looks like a social security number (SSN).",
"NormalizedText":"Is this a grabage or crap email abcdef@ abcd. com, phone: 4255550111, IP: 255. 255. 255. 255, 1234 Main Boulevard, Panapolis WA 96555. \r\nThese are all UK phone numbers: +44 123 456 7890 or 0234 567 8901 or 0456 789 0123. \r\nAlso, 999- 99- 9999 looks like a social security number ( SSN) .",
"Misrepresentation":null,
"PII":{  
  "Email":[  
    {  
      "Detected":"abcdef@abcd.com",
      "SubType":"Regular",
      "Text":"abcdef@abcd.com",
      "Index":32
    }
  ],
  "IPA":[  
    {  
      "SubType":"IPV4",
      "Text":"255.255.255.255",
      "Index":72
    }
  ],
  "Phone":[  
    {  
      "CountryCode":"US",
      "Text":"4255550111",
      "Index":56
    },
    {  
      "CountryCode":"US",
      "Text":"425 555 0111",
      "Index":211
    },
    {  
      "CountryCode":"UK",
      "Text":"+44 123 456 7890",
      "Index":207
    },
    {  
      "CountryCode":"UK",
      "Text":"0234 567 8901",
      "Index":227
    },
    {  
      "CountryCode":"UK",
      "Text":"0456 789 0123",
      "Index":244
    }
  ],
  "Address":[  
    {  
      "Text":"1234 Main Boulevard, Panapolis WA 96555",
      "Index":89
    }
  ],
  "SSN":[  
    {  
      "Text":"999999999",
      "Index":56
    },
    {  
      "Text":"999-99-9999",
      "Index":266
    }
  ]
},
"Classification":{  
  "ReviewRecommended":true,
  "Category1":{  
    "Score":1.5113095059859916E-06
  },
  "Category2":{  
    "Score":0.12747249007225037
  },
  "Category3":{  
    "Score":0.98799997568130493
  }
},
"Language":"eng",
"Terms":[  
  {  
    "Index":21,
    "OriginalIndex":21,
    "ListId":0,
    "Term":"crap"
  }
],
"Status":{  
  "Code":3000,
  "Description":"OK",
  "Exception":null
},
"TrackingId":"2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

Para una explicación detallada de todas las secciones de la respuesta JSON, consulte la guía conceptual de [moderación de texto](text-moderation-api.md).

## <a name="next-steps"></a>Pasos siguientes

Use la API REST del código o siga el [inicio rápido del SDK de .NET](dotnet-sdk-quickstart.md) para realizar la integración con la aplicación.
