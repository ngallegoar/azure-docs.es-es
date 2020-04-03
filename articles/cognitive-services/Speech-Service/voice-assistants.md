---
title: 'Asistentes de voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Introducción a las características, las funcionalidades y las restricciones de los asistentes de voz mediante el kit de desarrollo de software (SDK) de Voz.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 9219c9a72ce9e1cfba3504b0b8e16ade77f8a5e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369903"
---
# <a name="what-is-a-voice-assistant"></a>¿Qué es un asistente de voz?

Los asistentes de voz que utilizan el servicio de voz permiten a los desarrolladores crear interfaces de conversación naturales, similares a la humana, para sus aplicaciones y experiencias.

El servicio del asistente de voz proporciona una interacción rápida y confiable entre un dispositivo y una implementación de asistente que usa (1) el canal de voz de Direct Line Speech de Bot Framework o (2) el servicio integrado de comandos personalizados (versión preliminar) para la finalización de tareas.

Las aplicaciones se conectan al servicio del asistente de voz con el kit de desarrollo de software (SDK) de Voz.

   ![Diagrama conceptual del flujo de servicio de orquestación del asistente de voz](media/voice-assistants/overview.png "Flujo del asistente de voz")

## <a name="choosing-an-assistant-solution"></a>Elección de una solución de asistente

El primer paso para crear un asistente de voz es decidir qué se debe hacer. El servicio de voz proporciona varias soluciones complementarias para diseñar las interacciones de los asistentes. Si desea la flexibilidad y la versatilidad que proporciona el canal [Direct Line Speech](direct-line-speech.md) de Bot Framework o la simplicidad de [Custom Commands (versión preliminar)](custom-commands.md) para escenarios sencillos, la selección de las herramientas adecuadas le ayudará a comenzar.

| Si quiere... | Considere: | Por ejemplo... |
|-------------------|------------------|----------------|
|Conversación abierta con integración de sólidas aptitudes y control completo de la implementación | El canal [Direct Line Speech](direct-line-speech.md) de Bot Framework | <ul><li>"Tengo que ir a Seattle"</li><li>"¿Qué tipo de pizza puedo pedir?"</li></ul>
|Conversación orientada a tareas o controles y comandos con la creación y el hospedaje simplificados | [Comandos personalizados (versión preliminar)](custom-commands.md) | <ul><li>"Encender la luz superior"</li><li>"Subir la temperatura 5 grados"</ul>

Se recomienda [Direct Line Speech](direct-line-speech.md) como mejor opción predeterminada si aún no está seguro de lo que le gustaría que controlara el asistente. Ofrece integración con un completo conjunto de herramientas y ayudas de creación, como la [solución de asistente virtual y de plantilla de empresa](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) y el [servicio de QnA Maker](https://docs.microsoft.com/azure/cognitive-services/QnAMaker/Overview/overview) para crear patrones comunes y usar los orígenes de conocimiento existentes.

[Custom Commands (versión preliminar)](custom-commands.md) proporciona una experiencia de creación y hospedaje simplificada específicamente adaptada a escenarios de control y comandos de lenguaje natural.

   ![Comparación de soluciones de asistentes](media/voice-assistants/assistant-solution-comparison.png "Comparación de soluciones de asistentes")

## <a name="core-features"></a>Características principales

Tanto si elige [Direct Line Speech](direct-line-speech.md) o [Custom Commands (versión preliminar)](custom-commands.md) para crear sus interacciones con el asistente, puede usar un amplio conjunto de características de personalización para personalizar el asistente para la marca, el producto y la personalidad.

| Category | Características |
|----------|----------|
|[Palabra clave personalizada](speech-devices-sdk-create-kws.md) | Los usuarios pueden iniciar conversaciones con los asistentes mediante una palabra clave personalizada como "Hola, Contoso". Una aplicación lo lleva a cabo con un motor de palabras clave personalizadas en el SDK de Voz, que puede configurarse con una palabra clave [que puede generar aquí](speech-devices-sdk-create-kws.md). Los asistentes de voz pueden utilizar la comprobación de palabras clave del lado del servicio para mejorar la precisión de la activación de palabras clave (frente al dispositivo por sí solo).
|[Speech to Text](speech-to-text.md) | Los asistentes de voz convierten audio en tiempo real en texto reconocido mediante la [Conversión de voz en texto](speech-to-text.md) del servicio de voz. Este texto está disponible, a medida que se escribe, tanto para la implementación del asistente como para la aplicación cliente.
|[Texto a voz](text-to-speech.md) | Las respuestas textuales desde el asistente se sintetizan mediante [Texto a voz](text-to-speech.md) del servicio de voz. A continuación, esta síntesis se pone a disposición de la aplicación cliente como una secuencia de audio. Microsoft ofrece la posibilidad de crear su propia voz TTS neuronal personalizada de alta calidad que le pone voz a su marca. Para obtener más información, [póngase en contacto con nosotros](mailto:mstts@microsoft.com).

## <a name="getting-started-with-voice-assistants"></a>Introducción a los asistentes de voz

Le ofrecemos inicios rápidos diseñados para que ejecute el código en menos de 10 minutos. Esta tabla incluye una lista de inicios rápidos para asistente de voz ordenados por idioma.

| Guía de inicio rápido | Plataforma | Referencia de API |
|------------|----------|---------------|
| C#, UWP | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Browse](https://aka.ms/csspeech/javaref) |
| Java | Android | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Código de ejemplo

El código de ejemplo para crear un asistente de voz está disponible en GitHub. Estos ejemplos abarcan la aplicación cliente para conectarse al asistente en varios lenguajes de programación conocidos.

* [Ejemplos del asistente de voz (SDK)](https://aka.ms/csspeech/samples)
* [Tutorial: Habilitación del asistente de voz mediante el SDK de Voz, C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="tutorial"></a>Tutorial

Tutorial sobre cómo [habilitar la voz para el asistente mediante el SDK de Voz y el canal Direct Line Speech](tutorial-voice-enable-your-bot-speech-sdk.md).

## <a name="customization"></a>Personalización

Los asistentes de voz creados mediante el servicio de voz pueden usar la amplia variedad de opciones de personalización disponibles para la [conversión de voz en texto](speech-to-text.md), la [conversión de texto en voz](text-to-speech.md) y la [selección de palabras clave personalizadas](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Las opciones de personalización varían según el idioma o la configuración regional (consulte los [idiomas admitidos](supported-languages.md)).

## <a name="reference-docs"></a>Documentos de referencia

* [Acerca del SDK de Voz](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
* [Obtención del SDK de voz](speech-sdk.md)
* [Más información sobre los comandos personalizados (versión preliminar)](custom-commands.md)
* [Más información sobre Direct Line Speech](direct-line-speech.md)
