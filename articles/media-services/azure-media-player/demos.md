---
title: Demostraciones de Azure Media Player
description: Esta página contiene un listado de vínculos a demostraciones de Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: article
ms.date: 04/24/2020
ms.openlocfilehash: 584748b23f526e6f03b543b8298927e3f202f743
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82138915"
---
# <a name="azure-media-player-demos"></a>Demostraciones de Azure Media Player

A continuación se muestra una lista de vínculos a las demostraciones de Azure Media Player. Puede descargar todos los [ejemplos de Azure Media Player](https://github.com/Azure-Samples/azure-media-player-samples) desde GitHub.

## <a name="demo-listing"></a>Listado de demostraciones

| Nombre del ejemplo | Mediante programación a través de JavaScript | Estático a través de elemento de vídeo HTML5 | Descripción |
| ------------|----------------------------|-------------------------------------|--------------|
| Básica |
| Establecer origen | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_setsource.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_setsource.html) |Reproducir contenido no protegido.|
| Características |
| Inserción de anuncios VoD: VAST | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_vast_ads_vod.html) | N/D | Inserte anuncios VAS previos a mid y posteriores a roll en un recurso de VoD. |
| Velocidad de reproducción | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_playback_speed.html)| N/D | Permite que los espectadores controles la velocidad a la que ven el vídeo. |
| Máscara de vaciado de AMP | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_flush_skin.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_flush_skin.html) | Habilita la nueva máscara de AMP. **Nota:** El vaciado de AMP solo se admite en las versiones 2.1.0 y posteriores de AMP |
| Títulos y subtítulos | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_webvtt.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_webvtt.html) | Reproducción con subtítulos de WebVTT.
| Subtítulos CEA 708 en directo | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_live_captions.html) | N/D | Reproducción con subtítulos de entrada CEA 708 en directo con los subtítulos alineados a la izquierda. |
| Streaming con reserva progresiva | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveFallback.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveFallback.html) | Configuración básica de la reproducción adaptable con reserva progresiva si el streaming no se admite en la plataforma. |
| MP4 de vídeo progresivo | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveVideo.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveVideo.html) | Reproducción de MP4 de audio progresivo. |
| MP3 de audio progresivo | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_progressiveAudio.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_progressiveAudio.html) | Reproducción de MP3 de audio progresivo. |
| DD+ | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_dolbyDigitalPlus.html) | N/D | Reproducción de contenido con audio DD+. |
| Opciones |
| Perfil heurístico | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_heuristicsProfile.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_heuristicsProfile.html) | Cambio del perfil heurístico |
| Localización | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_localization.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_localization.html) |
Configuración de la localización |
| Menú de pistas de audio | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiAudio.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_multiAudio.html) |
Opciones para mostrar cómo mostrar el menú de pistas de audio en la máscara predeterminada. |
| Teclas de acceso rápido | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_hotKeys.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_hotKeys.html) | En este ejemplo se muestra cómo configurar las teclas de acceso rápido que están habilitadas en el reproductor |
| Eventos, registro y diagnóstico |
| Registro de eventos | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_registerEvents.html) | N/D | Reproducción con clientes de escucha de eventos. |
| Registro | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_logging.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_logging.html) | Activación del registro detallado en la consola. |
| Diagnóstico | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_diagnostics.html) | N/D | Obtención de datos de diagnóstico. Este ejemplo solo funciona en algunas tecnologías. |
| AES |
| AES sin token | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_notoken.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_notoken.html) | Reproducción de contenido AES sin token. |
| Token AES | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token.html) | Reproducción de contenido AES con token. |
| Simulación de proxy AES HLS | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token_withHLSProxy.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token_withHLSProxy.html) | Reproducción de contenido AES con token, que muestra un proxy para HLS de modo que el token se pueda usar con dispositivos iOS. |
| Token AES, forzar flash | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_aes_token_forceFlash.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_aes_token_forceFlash.html) | Reproducción de contenido AES con token, que fuerza la tecnología flashSS. |
| DRM |
| DRM múltiple con PlayReady, Widevine y FairPlay | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevineFairPlay_notoken.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_multiDRM_PlayReadyWidevineFairPlay_notoken.html) | Reproducción de contenido DRM sin token, con encabezados PlayReady, Widevine y FairPlay. |
| PlayReady sin token | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_notoken.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_notoken.html) | Reproducción de contenido PlayReady sin token. |
| PlayReady sin token, forzar Silverlight | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_notoken_forceSilverlight.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_notoken_forceSilverlight.html) | Reproducción de contenido PlayReady sin token, que fuerza la tecnología silverlightSS. |
| Token PlayReady | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_token.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_token.html) | Reproducción de contenido PlayReady con token. |
| Token PlayReady, forzar Silverlight | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_playready_token_forceSilverlight.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_playready_token_forceSilverlight.html) | Reproducción de contenido PlayReady con token, que fuerza la tecnología silverlightSS. |
| Protocolo y tecnología |
| Cambiar techOrder | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_techOrder.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_techOrder.html) | Cambio del orden de tecnología |
| Forzar MPEG-DASH solo en UrlRewriter | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_forceDash.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_forceDash.html) | Reproducción de contenido no protegido solo con el protocolo DASH. |
| Excluir MPEG-DASH en UrlRewriter | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_forceNoDash.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_forceNoDash.html) | Reproducción de contenido no protegido solo con los protocolos Smooth y HLS. |
| Directiva de entrega múltiple | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_multipleDeliveryPolicy.html) | [Estática](https://amp.azure.net/libs/amp/latest/samples/videotag_multipleDeliveryPolicy.html) | Establecer el origen con contenido que tiene varias directivas de entrega desde Azure Media Services |
| Selección mediante programación |
| Seleccionar pista de texto | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectTextTrack.html) | N/D | Seleccionar una pista de WebVTT de la lista de pistas. |
| Seleccionar velocidad de bits | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectBitrate.html) | N/D | Seleccionar una velocidad de bits de la lista de velocidades de bits. Este ejemplo solo funciona en algunas tecnologías. |
| Seleccionar secuencia de audio | [Dinámica](https://amp.azure.net/libs/amp/latest/samples/dynamic_selectAudioStream.html) | N/D | Seleccionar una secuencia de audio de la lista de secuencias de audio disponibles. Este ejemplo solo funciona en algunas tecnologías. |

## <a name="next-steps"></a>Pasos siguientes

<!---Some context for the following links goes here--->
- [Inicio rápido de Azure Media Player](azure-media-player-quickstart.md)