---
title: 'SDK de dispositivos de voz Roobo Smart Audio Dev Kit v2: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Requisitos previos e instrucciones para comenzar a usar el SDK de dispositivos de voz, Roobo Smart Audio Dev Kit v2.
services: cognitive-services
author: anushapatnala
manager: wellsi
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: v-anusp
ms.openlocfilehash: 2f325c80877068c53d690bd7ff74f768dab3a174
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80371574"
---
# <a name="device-roobo-smart-audio-dev-kit-v2"></a>Dispositivo: Roobo Smart Audio Dev Kit v2

En este artículo se proporciona información específica de dispositivo para Roobo Smart Audio Dev Kit2.

## <a name="set-up-the-development-kit"></a>Configuración del kit de desarrollo

1. El kit de desarrollo tiene dos conectores micro USB. El conector de la izquierda sirve para conectar el kit de desarrollo y aparece resaltado como Power (Conectar) en la siguiente imagen. El de la derecha sirve para controlarlo y aparece marcado como Debug (Depurar) en la imagen. 
    ![Conexión del kit de desarrollo](media/speech-devices-sdk/roobo-v2-connections.png)
1. Conecte el kit de desarrollo mediante un cable micro USB para conectar el puerto de alimentación a un equipo o adaptador de corriente. Se iluminará un indicador de encendido verde en el panel superior.
1. Para controlar el kit de desarrollo, conecte el puerto de depuración a un equipo mediante un segundo cable micro USB. Es fundamental usar un cable de alta calidad para garantizar unas comunicaciones de confianza.
1. Oriente el kit de desarrollo de forma circular y vertical, con micrófonos orientados al techo como se muestra arriba.


## <a name="development-information"></a>Información de desarrollo

Para más información sobre desarrollo, consulte la [guía de desarrollo de Roobo](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

## <a name="audio-recordplay"></a>Grabación y reproducción de audio

Las operaciones de audio DDK2 se pueden realizar de las siguientes maneras:
* Use bibliotecas de código abierto de ALSA y sus aplicaciones.
* Use la interfaz appmainprog para realizar el desarrollo de aplicaciones. El marco de software relacionado con el audio de DDK2 usa el marco estándar de ALSA; puede usar libasound. Por lo tanto, puede desarrollar software directamente. De esta manera, puede usar arecord y aplay de ALSA directamente para grabar y reproducir audio.
