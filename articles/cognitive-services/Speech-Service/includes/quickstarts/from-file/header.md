---
title: 'Inicio rápido: Reconocimiento de la voz de un archivo de audio: servicio de voz'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: dapine
ms.openlocfilehash: 2b6270535c0cf69549a7412bd38d9207454e5500
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76038031"
---
En este inicio rápido usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer la voz que procede de un archivo de audio. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un archivo solo son necesarios unos pocos pasos:
> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `AudioConfig` que especifique el nombre de archivo .WAV.
> * Cree un objeto `SpeechRecognizer` con los objetos `SpeechConfig` y `AudioConfig` anteriores.
> * Con el objeto `SpeechRecognizer`, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto `SpeechRecognitionResult` devuelto.
