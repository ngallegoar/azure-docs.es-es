---
title: Directrices de privacidad para asistentes de voz en Windows
titleSuffix: Azure Cognitive Services
description: Instrucciones para habilitar la activación por voz de un agente de voz de forma predeterminada.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: 436367ede4f4be323b5334a201b1c9fb8f7f28e8
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997327"
---
# <a name="privacy-guidelines-for-voice-assistants-on-windows"></a>Directrices de privacidad para asistentes de voz en Windows

Es importante que los usuarios obtengan información clara acerca de cómo se recopilan y usan sus datos de voz, y también que controlen esta recopilación y el modo de realizarla. Estas facetas principales de la privacidad (*divulgación* y *consentimiento*) son especialmente importantes para los asistentes de voz integrados en Windows, dada la naturaleza de escucha siempre activa de su uso.

Los desarrolladores que crean asistentes de voz en Windows deben incluir elementos de interfaz de usuario claros dentro de sus aplicaciones que reflejen las funcionalidades de escucha del asistente.

> [!NOTE]
> Si no se proporciona la divulgación adecuada y el consentimiento de una aplicación de asistente, incluidas las actualizaciones de la aplicación, es posible que el ayudante deje de estar disponible para la activación por voz hasta que se resuelvan los problemas de privacidad. 

## <a name="minimum-requirements-for-feature-inclusion"></a>Requisitos mínimos para la inclusión de características

Los usuarios de Windows pueden ver y controlar la disponibilidad de sus aplicaciones de asistente en **`Settings > Privacy > Voice activation`** .

 > [!div class="mx-imgBorder"]
 > [![Lista de aplicaciones de privacidad](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png "Una entrada de configuración de privacidad de activación por voz de Windows para una aplicación de asistente")](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png#lightbox)

Para que se pueda incluir en esta lista, una aplicación debe:

1. Indicar de forma destacada a sus usuarios que escuchará una palabra clave, aunque la aplicación no se esté ejecutando, y la palabra clave de que se trata.
1. Incluir una descripción de cómo se utilizarán los datos de voz de un usuario, incluido un vínculo o una referencia a las directivas de privacidad pertinentes.
1. Informar a los usuarios de que, además de las opciones de configuración de la aplicación, los usuarios pueden ver y modificar sus opciones de privacidad en **`Settings > Privacy > Voice activation`** y, de manera opcional, incluir un vínculo de protocolo a `ms-settings:privacy-voiceactivation` para el acceso directo.

Después de cumplir estos requisitos y obtener la aprobación de Microsoft, una aplicación de asistente aparecerá en la lista de aplicaciones de activación por voz una vez que se haya registrado con las API `Windows.ApplicationModel.ConversationalAgent`, y los usuarios podrán proporcionar consentimiento a la aplicación para la activación por palabra clave. De forma predeterminada, el ajuste de ambas opciones es `Off` y es necesario que el usuario visite manualmente la página Configuración para habilitarlas.

> [!NOTE]
> En todos los casos, el permiso de activación por voz requiere el permiso micrófono. Si una aplicación de asistente no tiene acceso al micrófono, no será válida para la activación por voz y aparecerá en la configuración de privacidad de activación por voz en estado deshabilitado.

## <a name="additional-requirements-for-inclusion-in-microphone-consent"></a>Requisitos adicionales para la inclusión en el consentimiento del micrófono

Los autores del asistente que deseen facilitar y más fácilmente que sus usuarios puedan participar en la activación por voz pueden hacerlo si cumplen algunos requisitos adicionales para los anteriores. Una vez que se cumplan, la configuración de activación por voz estándar de dispositivo desbloqueado de la aplicación se establecerá de forma predeterminada en `On` cuando se conceda acceso al micrófono a la aplicación (y solo entonces). Esto elimina la necesidad de otra visita a Configuración antes de activar por voz un asistente.

Una aplicación de asistente debe cumplir los siguientes requisitos adicionales:

1. **Antes de** solicitar el consentimiento del micrófono (por ejemplo, mediante la API `AppCapability.RequestAccessAsync`), debe proporcionar una indicación destacada al usuario de que la aplicación del asistente desea escuchar la voz de un usuario para una palabra clave, aunque la aplicación no se esté ejecutando, y desea obtener el consentimiento del usuario.
2. Incluir toda la información importante sobre el uso de datos y las directivas de privacidad **antes** de solicitar acceso al micrófono o usar las API `Windows.ApplicationModel.ConversationalAgent`.
3. Evitar cualquier directiva o texto de instrucción (por ejemplo, "haga clic en sí en el siguiente símbolo del sistema") en el flujo de la experiencia que revele el comportamiento de la captura de audio y solicite permiso.

Una vez que se cumplan estos requisitos, aparecerá una aplicación de asistente válida en la lista de aplicaciones aptas para la activación por voz con el estado `enabled` una vez que se haya concedido el acceso al micrófono.

> [!NOTE]
> La activación por voz sobre el bloqueo no es apta para la habilitación automática con el acceso al micrófono y seguirá requiriendo que un usuario que visite la página privacidad de activación por voz para habilitar el acceso sobre el bloqueo para un asistente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre procedimientos recomendados de asistentes de voz en Windows](windows-voice-assistants-best-practices.md)