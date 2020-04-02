---
title: Notas de la versión de Video Indexer de Azure Media Services | Microsoft Docs
description: Para mantenerse al día con los últimos desarrollos, en este artículo se proporcionan las actualizaciones más reciente en Video Indexer de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 01/07/2020
ms.author: juliako
ms.openlocfilehash: f1387273f9736fea70682177d5d48dc2f141bbad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76933858"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Notas de la versión de Video Indexer de Azure Media Services

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue esta URL (`https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+Video+Indexer+release+notes%22&locale=en-us`) en el lector de fuentes RSS.

Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas

## <a name="january-2020"></a>Enero de 2020
 
### <a name="custom-language-support-for-additional-languages"></a>Compatibilidad con idiomas personalizados adicionales

Video Indexer admite ahora modelos de lenguaje personalizados para `ar-SY`, `en-UK` y `en-AU` (solo API).
 
### <a name="delete-account-timeframe-action-update"></a>Actualización del período de tiempo de la acción de eliminación de una cuenta

La acción de eliminación de una cuenta ahora elimina la cuenta en un plazo de 90 días en lugar de 48 horas.
 
### <a name="new-video-indexer-github-repository"></a>Nuevo repositorio de GitHub para Video Indexer

Ahora hay disponible un nuevo repositorio de GitHub para Video Indexer con distintos proyectos, guías de introducción y ejemplos de código: https://github.com/Azure-Samples/media-services-video-indexer
 
### <a name="swagger-update"></a>Actualización de Swagger

Video Indexer ha unificado las **autenticaciones** y **operaciones** en una [especificación de OpenAPI para Video Indexer (swagger)](https://api-portal.videoindexer.ai/docs/services/Operations/export?DocumentFormat=OpenApiJson). Los desarrolladores pueden encontrar las API en el [Portal para desarrolladores de Video Indexer](https://api-portal.videoindexer.ai/).

## <a name="december-2019"></a>Diciembre de 2019

### <a name="update-transcript-with-the-new-api"></a>Actualización de transcripción con la nueva API

Actualice una sección específica de la transcripción mediante la API [Update-Video-Index](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?&pattern=update).

### <a name="fix-account-configuration-from-the-video-indexer-portal"></a>Corrección de la configuración de la cuenta desde el portal de Video Indexer

Ahora puede actualizar la configuración de conexión de Media Services y usar la autoayuda para resolver problemas como estos: 

* Recurso incorrecto de Azure Media Services
* Cambios de contraseña
* Los recursos de Media Services se han movido entre suscripciones  

Para corregir la configuración de la cuenta, en el portal de Video Indexer, vaya a Configuración > pestaña Cuenta (como propietario).

### <a name="configure-the-custom-vision-account"></a>Configuración de la cuenta de Custom Vision

Configure la cuenta de Custom Vision en cuentas de pago mediante el portal de Video Indexer (anteriormente, esto solo lo admitía la API). Para ello, inicie sesión en el portal de Video Indexer, elija Personalización de modelos > Personajes animados > Configurar. 

### <a name="scenes-shots-and-keyframes--now-in-one-insight-pane"></a>Escenas, tomas y fotogramas clave: ahora en un panel de información detallada

Ahora las escenas, las tomas y los fotogramas clave se combinan en un panel de información detallada para facilitar el consumo y la navegación. Al seleccionar la escena deseada, puede ver qué tomas y fotogramas clave contiene. 

### <a name="notification-about-a-long-video-name"></a>Notificación sobre un nombre de vídeo largo

Cuando el nombre de un vídeo tiene más de 80 caracteres, Video Indexer muestra un error descriptivo durante la carga.

### <a name="streaming-endpoint-is-disabled-notification"></a>Notificación de punto de conexión de streaming deshabilitado

Cuando el punto de conexión de streaming está deshabilitado, Video Indexer mostrará un error descriptivo en la página del reproductor.

### <a name="error-handling-improvement"></a>Mejora en el control de errores

Ahora, si un vídeo se indexa de forma activa, se devolverá el código de estado 409 desde las API [Re-Index Video](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?) (Volver a indexar el vídeo) y [Update Video Index](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?) (Actualizar índice de vídeo), para evitar que se invaliden por accidente los cambios de volver a indexar.

## <a name="november-2019"></a>Noviembre de 2019
 
* Compatibilidad con modelos de lenguaje personalizado en coreano

    Video Indexer ahora admite modelos de lenguaje personalizados en coreano (`ko-KR`) tanto en la API como en el portal. 
* Nuevos idiomas compatibles con Conversión de voz en texto (STT)

    Las API de Video Indexer ahora admiten STT en árabe levantino (ar-SY), dialecto inglés del Reino Unido (en-GB) y dialecto australiano inglés (en-AU).
    
    En el caso de la carga de vídeo, reemplazamos zh-HANS por zh-CN, se admiten ambos, pero zh-CN es el recomendado y el más preciso.
    
## <a name="october-2019"></a>Octubre de 2019
 
* Búsqueda de personajes animados en la galería

    Ahora, al indexar personajes animados, puede buscarlos en la galería de vídeos de la cuenta. Para más información, consulte [Reconocimiento de personajes animados](animated-characters-recognition.md).

## <a name="september-2019"></a>Septiembre de 2019
 
En la feria IBC 2019, se anunciaron varios avances:
 
* Reconocimiento de caracteres animados (versión preliminar pública)

    Posibilidad de detectar, agrupar y reconocer caracteres en el contenido animado, gracias a la integración con Custom Vision. Para más información, consulte [Detección de personajes animados](animated-characters-recognition.md).
* Identificación de varios idiomas (versión preliminar pública)

    Detecte segmentos en varios idiomas en la pista de audio y cree una transcripción multilingüe a partir de ellos. Compatibilidad inicial: inglés, español, alemán y francés. Para más información, consulte [Identificación y transcripción automáticas del contenido de varios idiomas](multi-language-identification-transcription.md).
* Extracción de entidades con nombre para personas y ubicaciones

    extrae marcas, ubicaciones y personas del lenguaje hablado y del texto visual mediante el procesamiento de lenguaje natural (NLP).
* Clasificación del tipo de toma editorial

    Etiquetado de tomas con tipos editoriales como cierre, toma media, dos tomas, interior, exterior, etc. Para más información, consulte [Detección del tipo de toma editorial](scenes-shots-keyframes.md#editorial-shot-type-detection).
* Mejora de la inferencia de temas: ahora abarca el nivel 2
    
    El modelo de inferencia de temas ahora admite una granularidad más profunda de la taxonomía IPTC. Lea todos los detalles en [Innovación con inteligencia artificial de Azure Media Services](https://azure.microsoft.com/blog/azure-media-services-new-ai-powered-innovation/).

## <a name="august-2019"></a>Agosto de 2019
 
### <a name="video-indexer-deployed-in-uk-south"></a>Video Indexer implementado en Sur de Reino Unido

Ya puede crear una cuenta de pago de Video Indexer en la región Sur de Reino Unido.

### <a name="new-editorial-shot-type-insights-available"></a>Nueva información disponible sobre los tipos de capturas editoriales

Las nuevas etiquetas agregadas a las capturas de vídeo proporcionan "tipos de captura" editoriales para identificarlas con las frases editoriales que se suelen emplear en el flujo de trabajo de creación de contenido, como primer plano extremo, primer plano, plano general, plano medio, dos tomas, exteriores, interiores, cara izquierda y cara derecha (disponible en el JSON).

### <a name="new-people-and-locations-entities-extraction-available"></a>Extracción de entidades de nuevas personas y ubicaciones disponible

Video Indexer identifica las ubicaciones y las personas con nombre a través del procesamiento de lenguaje natural (NLP) desde el OCR y la transcripción del vídeo. Video Indexer usa el algoritmo de aprendizaje automático para reconocer cuándo se está haciendo mención a ubicaciones específicas (por ejemplo, la Torre Eiffel) o a personas (por ejemplo, John Doe) en un vídeo.

### <a name="keyframes-extraction-in-native-resolution"></a>Extracción de fotogramas clave en resolución nativa

Los fotogramas clave extraídos por Video Indexer están disponibles en la resolución original del vídeo.
 
### <a name="ga-for-training-custom-face-models-from-images"></a>Disponibilidad general para entrenar modelos de caras personalizadas a partir de imágenes

El entrenamiento de caras a partir de imágenes ha pasado del modo de versión preliminar a disponibilidad general (disponible mediante la API y en el portal).

> [!NOTE]
> La transición de versión preliminar a disponibilidad general no tiene ninguna repercusión en el precio.

### <a name="hide-gallery-toggle-option"></a>Ocultar opción de alternancia de la galería

El usuario puede optar por ocultar la pestaña de la galería en el portal (es similar a ocultar la pestaña de ejemplos).
 
### <a name="maximum-url-size-increased"></a>Aumento del tamaño máximo de la dirección URL

Compatibilidad con la cadena de consulta de dirección URL 4096 (en lugar de 2048) en la indexación de un vídeo.
 
### <a name="support-for-multi-lingual-projects"></a>Compatibilidad con proyectos multilingües

Ahora se pueden crear proyectos basados en vídeos indexados en distintos idiomas (solo API).

## <a name="july-2019"></a>Julio de 2019

### <a name="editor-as-a-widget"></a>Editor como un widget

El editor Video Indexer AI ahora está disponible como widget para insertarse en las aplicaciones de los clientes.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>Actualización del modelo de lenguaje personalizado desde el archivo de subtítulos cerrados desde el portal

Los clientes pueden proporcionar los formatos de archivo VTT, SRT y TTML como entrada para los modelos de idioma en la página de personalización del portal.

## <a name="june-2019"></a>Junio de 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer se ha implementado en el Japón Oriental

Ya puede crear una cuenta de pago de Video Indexer en la región del Japón Oriental.

### <a name="create-and-repair-account-api-preview"></a>Crear y reparar la API de la cuenta (versión preliminar)

Se agregó una nueva API que le permite [actualizar el punto de conexión o la clave de la instancia Azure Media Services](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Mejorar el control de errores en la carga 

Se devuelve un mensaje descriptivo en caso de que haya una configuración incorrecta de la cuenta subyacente de Azure Media Services.

### <a name="player-timeline-keyframes-preview"></a>Versión preliminar de los fotogramas clave de la escala de tiempo del reproductor 

Ya puede ver una versión preliminar de la imagen de cada hora en la escala de tiempo del reproductor.

### <a name="editor-semi-select"></a>Selección parcial del editor

Ya puede ver una versión preliminar de todas las conclusiones que se seleccionaron como resultado de elegir un período de tiempo específico en el editor.

## <a name="may-2019"></a>Mayo de 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Actualizar el modelo de lenguaje personalizado desde el archivo de subtítulos

Las API para [crear modelos de lenguaje personalizados](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) y [actualizar los modelos de lenguaje personalizados](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) admiten los formatos de archivo VTT, SRT y TTML como entrada para los modelos de lenguaje.

Al llamar a la [API para actualizar la transcripción de vídeo](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript), la transcripción se agrega automáticamente. El modelo de aprendizaje asociado con el vídeo también se actualiza automáticamente. Para obtener información sobre cómo personalizar y entrenar sus modelos de lenguaje, consulte [Customize a Language model with Video Indexer](customize-language-model-overview.md) (Personalizar un modelo de lenguaje con el Video Indexer).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Nuevos formatos de transcripción de descarga: TXT y CSV

Además del formato de subtítulos ya admitidos (SRT, VTT y TTML), Video Indexer ahora admite la descarga de la transcripción en los formatos TXT y CSV.

## <a name="next-steps"></a>Pasos siguientes

[Información general](video-indexer-overview.md)
