---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: fdcb5bb57afe145b57cb1223242d4d5625a2dfb7
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400739"
---
Se necesita la clave y la región de la suscripción del recurso del servicio de voz para crear un objeto de configuración de voz. El objeto de configuración es necesario para crear una instancia de un objeto del reconocedor de voz.

La instancia del reconocedor ofrece varias formas de reconocer la voz. En este ejemplo, la voz se reconoce una vez. Esta funcionalidad permite que el servicio de voz sepa que solo va a enviar una frase para el reconocimiento y que, una vez que se identifica la frase, se detendrá el reconocimiento de voz. Una vez que se produce el resultado, el código escribirá el motivo del reconocimiento en la consola.

> [!TIP]
> El SDK de voz se utilizará de forma predeterminada para reconocer el uso de `en-us` como idioma. Para más información sobre cómo elegir el idioma de origen, consulte [Especificación del idioma de origen para la conversión de voz a texto](../../../how-to-specify-source-language.md).