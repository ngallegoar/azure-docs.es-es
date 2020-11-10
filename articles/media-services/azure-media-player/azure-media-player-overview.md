---
title: Introducción a Azure Media Player
description: Más información acerca de Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 9b99e24e47da69aa0f081e056230f4b0c1aa6bb1
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321726"
---
# <a name="azure-media-player-overview"></a>Información general sobre Azure Media Player #

Azure Media Player es un reproductor de vídeo web que reproduce contenido multimedia de [Microsoft Azure Media Services](https://azure.microsoft.com/services/media-services/) en una gran variedad de exploradores y dispositivos. Explorador multimedia de Azure utiliza los estándares del sector, como HTML5, Media Source Extensions (MSE, extensiones de origen multimedia) y Encrypted Media Extensions (EME, extensiones multimedia cifradas) para proporcionar una experiencia de streaming adaptativa enriquecida.  Cuando estos estándares no están disponibles en un dispositivo o en un explorador, Azure Media Player usa Flash y Silverlight como tecnología de reserva. Independientemente de la tecnología de reproducción que se use, los desarrolladores tienen una interfaz de JavaScript unificada para acceder a las API, lo que permite que el contenido servido por Azure Media Services se reproduzca en una amplia gama de dispositivos y exploradores sin ningún esfuerzo adicional.

Microsoft Azure Media Services permite ofrecer contenido con formatos de streaming DASH, Smooth Streaming y HLS para reproducir contenido. Azure Media Player tiene en cuenta estos diversos formatos y reproduce automáticamente el vínculo mejor según las capacidades del explorador y plataforma. Microsoft Azure Media Services también proporciona cifrado dinámico de recursos con el cifrado común (PlayReady o Widevine) o el cifrado de sobre de AES de 128 bits. Azure Media Player permite el descifrado del contenido cifrado de AES de 128 bits y PlayReady cuando se configura correctamente.  Consulte la sección [Contenido protegido](azure-media-player-protected-content.md) para conocer cómo configurar el reproductor.

Para solicitar características nuevas, proporcionar ideas o comentarios, envíelos a [UserVoice para Azure Media Player](https://aka.ms/ampuservoice). Si tiene problemas concretos, preguntas o encontró algún error, escríbanos a ampinfo@microsoft.com.

> [!NOTE]
> Tenga en cuenta que Azure Media Player solo admite transmisiones multimedia de Azure Media Services.

## <a name="license"></a>Licencia ##

Azure Media Player se usa bajo licencia y está sujeto a los términos descritos en los términos de licencia del software de Microsoft para Azure Media Player. Consulte el [archivo de licencia](/legal/azure-media-player/azure-media-player-license) para ver todos los términos. Para más información, consulte la [Declaración de privacidad](https://www.microsoft.com/en-us/privacystatement/default.aspx).

Copyright 2015 Microsoft Corporation.

## <a name="next-steps"></a>Pasos siguientes ##

- [Inicio rápido de Azure Media Player](azure-media-player-quickstart.md)
