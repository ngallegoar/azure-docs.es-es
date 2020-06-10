---
title: 'Comandos personalizados (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Información general sobre las características, funcionalidades y restricciones de Custom Commands (versión preliminar), que es una solución para crear aplicaciones de voz.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 3c02e823202aa848a4de94885276835899562e4b
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266022"
---
# <a name="what-is-custom-commands-preview"></a>¿Qué es Comandos personalizados (versión preliminar)?

Las aplicaciones de voz, como los [asistentes de voz](voice-assistants.md), escuchan a los usuarios y realizan una acción en consecuencia, lo que a menudo implica una respuesta. Utilizan [conversión de voz en texto](speech-to-text.md) para transcribir la voz del usuario y, a continuación, realizan una acción en función de su comprensión del lenguaje natural del texto. Esta acción suele incluir una respuesta hablada del asistente generada con la [conversión de texto en voz](text-to-speech.md). Los dispositivos se conectan con los asistentes mediante el objeto `DialogServiceConnector` del SDK de voz.

**Custom Commands (versión preliminar)** es una solución simplificada para crear aplicaciones de voz. Proporciona una experiencia de creación unificada, un modelo de hospedaje automático y una complejidad relativamente inferior, en comparación con otras opciones como [Direct Line Speech](direct-line-speech.md). Sin embargo, esta simplificación conlleva una reducción en la flexibilidad. Por lo tanto, los comandos personalizados (versión preliminar) son más adecuados para escenarios de finalización de tareas o de comando y control. Es especialmente adecuado para los dispositivos de Internet de las cosas (IoT) y sin periféricos.

Para una interacción e integración de conversaciones complejas con otras soluciones como la [solución de Virtual Assistant y la plantilla empresarial](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview), se recomienda usar Direct Line Speech.

Los candidatos adecuados para los comandos personalizados (versión preliminar) tienen un vocabulario fijo con conjuntos de variables bien definidos. Por ejemplo, las tareas de automatización del hogar, como el control de un termostato, son ideales.

   ![Ejemplos de escenarios de finalización de tareas](media/voice-assistants/task-completion-examples.png "ejemplos de finalización de tareas")

## <a name="getting-started-with-custom-commands-preview"></a>Introducción a los comandos personalizados (versión preliminar)

El primer paso para usar Custom Commands (versión preliminar) para crear una aplicación de voz es [obtener una clave de suscripción al Servicio de voz](get-started.md) y acceder a Custom Commands Builder (versión preliminar) en [Speech Studio](https://speech.microsoft.com). Desde allí, puede crear una nueva aplicación de comandos personalizados (versión preliminar) y publicarla, tras lo cual una aplicación en el dispositivo podrá comunicarse con ella mediante el SDK de Voz.

   ![Flujo de creación de comandos personalizados (versión preliminar)](media/voice-assistants/custom-commands-flow.png "Flujo de creación de los comandos personalizados (versión preliminar)")

Le ofrecemos inicios rápidos diseñados para que ejecute el código en menos de 10 minutos.

* [Creación de una aplicación de comandos personalizados (versión preliminar)](quickstart-custom-speech-commands-create-new.md)
* [Creación de una aplicación de comandos personalizados (versión preliminar) con parámetros](quickstart-custom-speech-commands-create-parameters.md)
* [Conexión a una aplicación de comandos personalizados (versión preliminar) con el SDK de Voz, C#](quickstart-custom-speech-commands-speech-sdk.md)

Una vez que haya terminado con los inicios rápidos, explore nuestros procedimientos.

- [Adición de validaciones a los parámetros de Comandos personalizados](./how-to-custom-speech-commands-validations.md)
- [Realización de comandos en el cliente con el SDK de Voz](./how-to-custom-speech-commands-fulfill-sdk.md)
- [Adición de una confirmación a un Comando personalizado](./how-to-custom-speech-commands-confirmations.md)
- [Incorporación de una corrección de un paso a un comando personalizado](./how-to-custom-speech-commands-one-step-correction.md)

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
* [Vaya a Speech Studio para probar Custom Commands](https://speech.microsoft.com)
* [Obtención del SDK de voz](speech-sdk.md)
