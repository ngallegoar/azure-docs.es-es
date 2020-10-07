---
title: 'Inicio rápido: Creación de un asistente de voz personalizado (servicio de voz)'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 03/20/2020
ms.author: travisw
ms.openlocfilehash: bb88ef7f048b2454f05ef33286e59277070c3ea0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80241766"
---
En este inicio rápido, usará [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) para crear una aplicación de asistente de voz personalizada que se conecta a un bot que ya ha creado y configurado. Si necesita crear un bot, consulte [el tutorial relacionado](~/articles/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk.md), donde obtendrá una guía más completa.

Tras cumplir algunos requisitos previos, para conectar el asistente de voz personalizado hay que realizar pocos pasos:
> [!div class="checklist"]
> * Cree un objeto `BotFrameworkConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `DialogServiceConnector` con el objeto `BotFrameworkConfig` anterior.
> * Mediante el objeto `DialogServiceConnector`, inicie el proceso de escucha de una única expresión.
> * Inspeccione el objeto `ActivityReceivedEventArgs` devuelto.
