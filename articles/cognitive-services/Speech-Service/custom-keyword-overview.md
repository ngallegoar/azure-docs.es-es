---
title: Palabras clave personalizadas - Servicio de Voz
titleSuffix: Azure Cognitive Services
description: Introducción a las características, las funcionalidades y las restricciones de las palabras clave mediante el kit de desarrollo de software (SDK) de Voz.
services: cognitive-services
author: hasyashah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: hasshah
ms.openlocfilehash: 0a6df626f9567b51a8371c17004b454f9c5b4597
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202044"
---
# <a name="what-is-a-keyword"></a>¿Qué es una palabra clave?

Una palabra clave es una palabra o frase corta que permite activar por voz el producto. Por ejemplo, "Hola Cortana" es la palabra clave para el asistente Cortana. La activación por voz permite que los usuarios empiecen a interactuar con su producto sin tener que usar las manos, simplemente diciendo la palabra clave. Como el producto siempre espera escuchar la palabra clave, todo el audio se procesa de manera local en el dispositivo del usuario hasta que se produce una detección para asegurarse de que sus datos sigan siendo lo más privados posible.

## <a name="core-features-of-custom-keyword"></a>Características principales de la Palabra clave personalizada

Con las características de personalización, rendimiento e integración de la Palabra clave personalizada, puede adecuar la activación por voz para que se adapte mejor a las necesidades de los usuarios y a la visión del producto.

| Característica | Descripción |
|----------|----------|
| Personalización de palabras clave | Como extensión de su marca, una palabra clave reafirma el patrimonio que ha creado con sus clientes. El portal de Palabra clave personalizada en Speech Studio le permite especificar cualquier palabra o frase corta que mejor represente su marca. Para personalizar aún más la palabra clave, elija las pronunciaciones correctas, que el modelo de palabra clave generado respetará.
| Comprobación de la palabra clave | Cuando se confía sin problemas en que la palabra clave se detectará de manera local, el audio se envía a la nube para comprobar aún más que un usuario dijo la palabra clave. Comprobar la palabra clave brinda una capa de seguridad adicional al reducir el impacto de una detección local incorrecta y proteger la privacidad de los usuarios.
| Integración del asistente de voz y el SDK de Voz | Las palabras clave generadas en la Palabra clave personalizada en Speech Studio se pueden integrar fácilmente en el dispositivo o la aplicación a través del SDK de Voz. Solo tiene que apuntar el SDK al modelo de palabras clave proporcionado por Speech Studio y el producto se activará por voz, con el respaldo de la comprobación de la palabra clave. Para completar las experiencias de voz del producto, puede crear su propio [Asistente de voz](voice-assistants.md).

## <a name="get-started-with-custom-keywords"></a>Introducción a las palabras clave personalizadas

* Tutorial: Creación de una [palabra clave personalizada mediante Speech Studio](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws)
* Tutorial: Cómo [activar el producto con la voz mediante el SDK de voz, C#](tutorial-voice-enable-your-bot-speech-sdk.md)
* Inicio rápido: [Reconocimiento de palabras clave con el SDK de voz, en la Plataforma universal de Windows mediante C#](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer)
* Inicio rápido: [Reconocimiento de palabras clave con el SDK de voz, en Android mediante Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer)

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
* [Obtención del SDK de voz](speech-sdk.md)
* [Más información sobre los Asistentes de voz](voice-assistants.md)
