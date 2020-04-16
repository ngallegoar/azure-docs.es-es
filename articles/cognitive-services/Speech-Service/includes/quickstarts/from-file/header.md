---
title: 'Inicio rápido: Reconocimiento de la voz de un archivo de audio: servicio de voz'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: trbye
ms.openlocfilehash: a7c91775411f100a92dfcb0a7199dd4b25f6eca4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400513"
---
En este inicio rápido usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer la voz que procede de un archivo de audio. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un archivo solo son necesarios unos pocos pasos:
> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `AudioConfig` que especifique el nombre de archivo .WAV.
> * Cree un objeto `SpeechRecognizer` con los objetos `SpeechConfig` y `AudioConfig` anteriores.
> * Con el objeto `SpeechRecognizer`, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto `SpeechRecognitionResult` devuelto.
