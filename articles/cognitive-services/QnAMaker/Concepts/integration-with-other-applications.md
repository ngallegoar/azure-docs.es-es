---
title: 'Integración con otras aplicaciones: QnA Maker'
description: QnA Maker se integra en las aplicaciones cliente como los bots de chat, así como con otros servicios de procesamiento de lenguaje natural, como Language Understanding (LUIS).
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: ff5e64dd31542b1e31df6d9ca709567b322995ce
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91776534"
---
# <a name="design-knowledge-base-for-client-applications"></a>Diseño de una base de conocimiento para aplicaciones cliente

QnA Maker se integra en las aplicaciones cliente como los bots de chat, así como con otros servicios de procesamiento de lenguaje natural, como Language Understanding (LUIS).

## <a name="integration-with-a-conversational-client"></a>Integración con un cliente de conversación

QnA Maker se integra con las aplicaciones cliente de conversación, como [Microsoft Bot Framework](https://dev.botframework.com/). No es necesario organizar ni transformar el texto que se envía a QnA Maker. QnA Maker acepta idiomas naturales y devuelve la mejor respuesta.

## <a name="create-a-bot-without-writing-any-code"></a>Creación de un bot sin escribir ningún código

Después de publicar la base de conocimiento, cree un bot en la página **Publicar** al seleccionar el botón **Create bot** (Crear bot). Use el [tutorial de bot](../Quickstarts/create-publish-knowledge-base.md) para obtener información sobre lo que ocurre después de seleccionar el botón.

## <a name="providing-multi-turn-conversations"></a>Aprovisionamiento de conversaciones multiturno

Un cliente de bot proporciona la mejor respuesta seleccionada de knowledge base y puede proporcionar avisos de seguimiento si la respuesta forma parte de un par de QnA de varios turnos. Obtenga información sobre [cómo](../how-to/multiturn-conversation.md) agregar conjuntos de preguntas y respuestas de conversación multiturno a una base de conocimiento.

## <a name="natural-language-processing"></a>Procesamiento de lenguaje natural

Aunque QnA Maker procesa preguntas que utilizan el procesamiento de lenguaje natural, también se puede usar como parte de un sistema más grande que responde preguntas de varias bases de conocimiento. Puede combinar QnA Maker con otra instancia de Cognitive Services, (Language Understanding [LUIS]), para proporcionar procesamiento de lenguaje natural antes dirigirse a una base de conocimiento específica. Obtenga más información sobre cuándo y cómo usar [LUIS y QnA Maker](../../luis/choose-natural-language-processing-service.md?toc=/azure/cognitive-services/qnamaker/toc.json) en conjunto.

## <a name="next-steps"></a>Pasos siguientes

Aprenda los [conceptos](development-lifecycle-knowledge-base.md) del ciclo de desarrollo para QnA Maker.