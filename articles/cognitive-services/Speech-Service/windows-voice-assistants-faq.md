---
title: 'Asistentes de voz en Windows: preguntas más frecuentes'
titleSuffix: Azure Cognitive Services
description: Preguntas comunes que suelen aparecer durante el desarrollo del agente de voz de Windows.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: aa71057d3819acb335153ee5b4b65960320405be
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997291"
---
# <a name="samples-and-faqs"></a>Ejemplos y preguntas más frecuentes

## <a name="the-uwp-voice-assistant-sample"></a>Ejemplo del asistente de voz de UWP

El ejemplo del asistente de voz de UWP es un asistente de voz en Windows que sirve para:

- demostrar el uso de las API ConversationalAgent de Windows.
- Mostrar los procedimientos recomendados para un agente de voz en Windows.
- Proporcionar una aplicación adaptable y componentes reutilizables para su aplicación de agente MVP.
- Mostrar cómo se puede usar Direct Line Speech, junto con Bot Framework o comandos personalizados, con las API ConversationalAgent de Windows para disfrutar de una experiencia de agente de voz completa.

La documentación que se proporciona con la aplicación de ejemplo le guía a través de la ruta de acceso del código de activación por voz y administración del agente, desde los requisitos previos de inicio hasta la limpieza adecuada.

> [!div class="nextstepaction"]
> [Visitar el repositorio de GitHub para el ejemplo de UWP](https://aka.ms/MVA/sample)

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-do-i-contact-microsoft-for-resources-like-limited-access-feature-tokens-and-keyword-model-files"></a>¿Cómo puedo ponerme en contacto con Microsoft para obtener recursos como tokens de características de acceso limitado y archivos de modelo de palabra clave?

Póngase en contacto con winvoiceassistants@microsoft.com para solicitar estos recursos.

### <a name="my-app-is-showing-in-a-small-window-when-i-activate-it-by-voice-how-can-i-transition-from-the-compact-view-to-a-full-application-window"></a>Mi aplicación se muestra en una ventana pequeña cuando se activa mediante la voz. ¿Cómo puedo pasar de la vista compacta a una ventana de la aplicación completa?

Cuando la aplicación se activa por primera vez mediante la voz, se inicia en una vista compacta. Lea la [guía de diseño de la versión preliminar de activación de voz](windows-voice-assistants-best-practices.md#design-guidance-for-voice-activation-preview) para obtener instrucciones sobre las diferentes vistas y las transiciones entre estas para los asistentes de voz en Windows.

Para realizar la transición de la vista compacta a la vista de la aplicación completa, use la API de appView `TryEnterViewModeAsync`:

`var appView = ApplicationView.GetForCurrentView();
 await appView.TryEnterViewModeAsync(ApplicationViewMode.Default);`

### <a name="why-are-voice-assistant-features-on-windows-only-enabled-for-uwp-applications"></a>¿Por qué las características del asistente de voz en Windows solo están habilitadas para las aplicaciones para UWP?

Teniendo en cuenta los riesgos de privacidad asociados a características como la activación de voz, las características de la plataforma UWP son necesarias y permiten que las características del asistente de voz en Windows sean lo suficientemente seguras.

### <a name="the-uwp-voice-assistant-sample-uses-direct-line-speech-do-i-have-to-use-direct-line-speech-for-my-voice-assistant-on-windows"></a>El ejemplo del asistente de voz de UWP usa Direct Line Speech. ¿Tengo que usar Direct Line Speech para mi asistente de voz en Windows?

La aplicación para UWP de ejemplo se desarrolló con Direct Line Speech y el SDK de Speech Services como una demostración del uso de un servicio de diálogo con la funcionalidad del agente de conversación de Windows. Sin embargo, puede usar cualquier servicio para la comprobación de palabras clave local y en la nube, la conversión de voz a texto, el cuadro de diálogo de bot y la conversión de texto a voz.