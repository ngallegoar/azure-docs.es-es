---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: dapine
ms.openlocfilehash: 3125b6203f04b4cabd8cd0822a97317185cbf194
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80638134"
---
Se necesita la clave y la región de la suscripción del recurso del servicio de voz para crear un objeto de configuración de voz. El objeto de configuración es necesario para crear una instancia de un objeto del reconocedor de voz.

La instancia del reconocedor ofrece varias formas de reconocer la voz. En este ejemplo, la voz se reconoce una vez. Esta funcionalidad permite que el servicio de voz sepa que solo va a enviar una frase para el reconocimiento y que, una vez que se identifica la frase, se detendrá el reconocimiento de voz. Una vez que se produce el resultado, el código escribirá el motivo del reconocimiento en la consola.

> [!TIP]
> El SDK de voz se utilizará de forma predeterminada para reconocer el uso de `en-us` como idioma. Para más información sobre cómo elegir el idioma de origen, consulte [Especificación del idioma de origen para la conversión de voz a texto](../../../how-to-specify-source-language.md).