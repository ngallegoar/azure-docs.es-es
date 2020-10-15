---
title: Preguntas más frecuentes sobre los asistentes de voz
titleSuffix: Azure Cognitive Services
description: Obtenga respuestas a las preguntas más habituales sobre los asistentes de voz que usan comandos personalizados (versión preliminar) o el canal de Direct Line Speech.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 8480299c2c889a243150028ac9651f4b62656aec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74110344"
---
# <a name="voice-assistants-frequently-asked-questions"></a>Preguntas más frecuentes sobre los asistentes de voz

Si no encuentra respuestas a sus preguntas en este documento, [consulte otras opciones de soporte técnico](support.md).

## <a name="general"></a>General

**P: ¿Qué es un asistente de voz?**

**R:** Como Cortana, un asistente de voz es una solución que escucha las expresiones habladas de un usuario, analiza el contenido de esas expresiones en busca de significado, realiza una o varias acciones en respuesta a la intención de la expresión y, a continuación, proporciona una respuesta al usuario que a menudo incluye un componente hablado. Se trata de una experiencia de "conversación" para interactuar con un sistema. Los autores del asistente de voz crean una aplicación en el dispositivo mediante `DialogServiceConnector` en el SDK de Voz para comunicarse con un asistente creado mediante [comandos personalizados (versión preliminar)](custom-commands.md) o el canal de [Direct Line Speech](direct-line-speech.md) de Bot Framework. Estos asistentes pueden usar palabras clave personalizadas, conversaciones personalizadas y habla personalizada para proporcionar una experiencia adaptada a su marca o producto.

**P: ¿Debo usar comandos personalizados (versión preliminar) o Direct Line Speech? ¿Cuál es la diferencia?**

**R:** Los [comandos personalizados (versión preliminar)](custom-commands.md) son un conjunto de herramientas de menor complejidad para crear y hospedar fácilmente un asistente adecuado para escenarios de finalización de tareas. [Direct Line Speech](direct-line-speech.md) proporciona capacidades más enriquecidas y sofisticadas para permitir escenarios de conversación sólidos. Para obtener más información, consulte la [comparación de soluciones para asistentes](voice-assistants.md#choosing-an-assistant-solution).

**P: ¿Cómo empiezo?**

**R:** La mejor manera de empezar es la creación de una aplicación de comandos personalizados (versión preliminar) o un bot de Bot Framework.

- [Creación de una aplicación de comandos personalizados (versión preliminar)](quickstart-custom-speech-commands-create-new.md)
- [Creación de un bot de Bot Framework básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
- [Conexión de un bot al canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)

## <a name="debugging"></a>Depuración

**P: ¿Dónde está mi secreto de canal?**

**R:** Si ha usado la versión preliminar de Direct Line Speech o está leyendo la documentación relacionada, puede encontrar una clave secreta en la página de registro del canal Direct Line Speech. `FromBotSecret` de Factory Method `DialogServiceConfig` v1.7 en el SDK de Voz también espera este valor.

La versión más reciente de Direct Line Speech simplifica el proceso de comunicación con el bot desde un dispositivo. En la página de registro del canal, la lista desplegable de la parte superior asocia el registro del canal Direct Line Speech con un recurso de voz. Una vez asociado, el SDK de Voz v 1.8 incluye Factory Method `BotFrameworkConfig::FromSubscription` que configurará un `DialogServiceConnector` para que se comunique con el bot que ha asociado a la suscripción.

Si todavía está migrando la aplicación cliente de v1.7 a v1.8, `DialogServiceConfig::FromBotSecret` puede seguir trabajando con un valor no vacío y no nulo para su parámetro de secreto de canal, por ejemplo, el secreto anterior que utilizó. Simplemente se omitirá cuando se use una suscripción de voz asociada a un registro de canal más reciente. Tenga en cuenta que el valor _debe_ ser no nulo y no estar vacío, ya que se comprueban en el dispositivo antes de que la asociación del lado de servicio sea apropiada.

Para obtener una guía más detallada, consulte la [sección del tutorial](tutorial-voice-enable-your-bot-speech-sdk.md#register-the-direct-line-speech-channel) que le guía por el registro del canal.

**P: Se genera un error 401 durante la conexión y nada funciona. Sé que mi clave de suscripción de voz es válida. ¿Qué está ocurriendo?**

**R:** Cuando administre la suscripción en Azure Portal, asegúrese de que usa el recurso **Voz** (Microsoft.CognitiveServicesSpeechServices, "Voz") y _no_ el recurso **Cognitive Services** (Microsoft.CognitiveServicesAllInOne, "Todos los servicios de Cognitive Services"). Consulte también [regiones admitidas del servicio de voz para los asistentes de voz](regions.md#voice-assistants).

![Suscripción correcta para Direct Line Speech](media/voice-assistants/faq-supported-subscription.png "ejemplo de una suscripción de voz compatible")

**P: Obtengo el texto de reconocimiento de `DialogServiceConnector`, pero veo un error "1011" y no recibo nada de mi bot. ¿Por qué?**

**R:** Este error indica un problema de comunicación entre su asistente y el servicio del asistente de voz.

- En el caso de los comandos personalizados (versión preliminar), asegúrese de que la aplicación de comandos personalizados (versión preliminar) esté publicada.
- En el caso de Direct Line Speech, asegúrese de que ha [conectado su bot al canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech), [ha agregado compatibilidad con el protocolo de transmisión](https://aka.ms/botframework/addstreamingprotocolsupport) al bot (con la compatibilidad relacionada con socket web) y, luego, compruebe que su bot responde a las solicitudes entrantes del canal.

**P: Este código todavía no funciona o recibo un error diferente cuando uso `DialogServiceConnector`. ¿qué debo hacer?**

**R:** El registro basado en archivos proporciona bastantes más detalles y puede ayudarle a acelerar las solicitudes de soporte técnico. Para habilitar esta funcionalidad, consulte el [uso del registro de archivos](how-to-use-logging.md).

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas](troubleshooting.md)
- [Notas de la versión](releasenotes.md)
