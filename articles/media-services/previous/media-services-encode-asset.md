---
title: Introducción a los codificadores multimedia a petición de Azure | Microsoft Docs
description: Azure Media Services ofrece varias opciones para la codificación de medios en la nube. En este artículo se proporciona información general de los codificadores multimedia a petición de Azure.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: e841d1922beb3f0276ff94cacbbdf566b23cdf6e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92017291"
---
# <a name="overview-of-azure-on-demand-media-encoders"></a>Introducción a los codificadores multimedia a petición de Azure

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> No hay características o funcionalidades nuevas para agregar a Media Services, versión 2. <br/>Finalice la compra de la versión más reciente, [Media Services v3](../latest/index.yml). Consulte también la [guía de migración de la versión v2 a v3](../latest/migrate-from-v2-to-v3.md).

Azure Media Services ofrece varias opciones para la codificación de medios en la nube.

Cuando comience con Media Services, es importante comprender la diferencia entre códecs y formatos de archivo.
Los códecs son el software que implementa los algoritmos de compresión/descompresión, mientras que los formatos de archivo son contenedores que contienen el vídeo comprimido.

Media Services proporciona empaquetado dinámico que permite entregar contenido codificado MP4 de velocidad de bits adaptable o Smooth Streaming en formatos de streaming admitidos por Media Services (MPEG-DASH, HLS y Smooth Streaming) sin tener que volver a realizar el empaquetamiento en estos formatos de streaming.

Cuando se crea la cuenta de Media Services, se agrega un punto de conexión de streaming **predeterminado** a la cuenta en estado **Detenido**. Para iniciar la transmisión del contenido y aprovechar el empaquetado dinámico y el cifrado dinámico, el punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado **Running** (En ejecución). La facturación de los puntos de conexión de streaming se produce siempre que el punto de conexión se encuentra en estado de **ejecución**.

Media Services admite el siguiente codificador a petición:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)

En este artículo se ofrece una breve introducción a los codificadores multimedia a petición, junto con vínculos a artículos con información más detallada.

De forma predeterminada cada cuenta de Media Services puede tener una tarea de codificación activa a la vez. Puede reservar unidades de codificación que permiten la ejecución simultánea de varias tareas de codificación, una para cada unidad reservada de codificación que ha adquirido. Para obtener información, consulte [Escalado de unidades de codificación](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Estándar

### <a name="how-to-use"></a>Modo de uso
[Codificación con Codificador multimedia estándar](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formatos
[Códecs y formatos](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Valores preestablecidos
Codificador multimedia estándar se configura mediante uno de los valores preestablecidos descritos [aquí](./media-services-mes-presets-overview.md).

### <a name="input-and-output-metadata"></a>Metadatos de entrada y salida
[Aquí](media-services-input-metadata-schema.md)se describen los metadatos de entrada de los codificadores.

[Aquí](media-services-output-metadata-schema.md)se describen los metadatos de salida de los codificadores.

### <a name="generate-thumbnails"></a>Generación de miniaturas
Para más información, consulte [Generación de miniaturas](media-services-advanced-encoding-with-mes.md).

### <a name="trim-videos-clipping"></a>Recorte de vídeos
Para más información, consulte [Recorte de un vídeo](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Creación de superposiciones
Para más información, consulte [Creación de una superposición](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Consulte también
[El blog de Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

### <a name="known-issues"></a>Problemas conocidos
Si el vídeo de entrada no contiene subtítulos, el recurso de salida seguirá conteniendo un archivo TTML vacío.

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Artículos relacionados
* [Realización de tareas de codificación avanzadas mediante la personalización de valores preestablecidos de Media Encoder Estándar](media-services-custom-mes-presets-with-dotnet.md)
* [Cuotas y limitaciones](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/