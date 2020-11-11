---
title: Disponibilidad regional de Azure Media Services | Microsoft Docs
description: En este artículo se ofrece información general sobre las características de Microsoft Azure Media Services y la disponibilidad regional del servicio.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: inhenkel
ms.custom: references_regions
ms.openlocfilehash: cf84e45ff43643c7e9a983b867194a7920aa1b57
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93351849"
---
# <a name="media-services-regional-availability"></a>Disponibilidad regional de Media Services

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> No hay características o funcionalidades nuevas para agregar a Media Services, versión 2. Finalice la compra de la versión más reciente, [Media Services v3](../latest/media-services-overview.md). Consulte también la [guía de migración de la versión v2 a v3](../latest/migrate-from-v2-to-v3.md).

Microsoft Azure Media Services (AMS) permite cargar, almacenar, codificar y empaquetar de forma segura contenido de audio o vídeo para su entrega a petición y mediante streaming en vivo a varios clientes (por ejemplo, televisión, PC y dispositivos móviles).

AMS funciona en varias regiones de todo el mundo, lo que le ofrece flexibilidad para elegir dónde compilar las aplicaciones. En este artículo se ofrece información general sobre las características de Microsoft Azure Media Services y la disponibilidad regional del servicio.

Para más información sobre la infraestructura global de Azure, consulte [Zonas geográficas de Azure](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="ams-accounts"></a>Cuentas de AMS

Use [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=media-services&regions=all) para determinar si Media Services está disponible en una región concreta.

## <a name="streaming-endpoints"></a>Puntos de conexión de streaming

Los clientes de Media Services pueden elegir un punto de conexión de streaming **estándar** o uno **premium**.

|Nombre|Status|Region
|---|---|---|
|Estándar|GA|All|
|Premium|GA|All|

## <a name="live-encoding"></a>Live Encoding

Disponible en todas las regiones excepto en: Alemania, Sur de Brasil, India occidental, Sur de la India y Centro de la India.

## <a name="encoding-media-processors"></a>Codificación de procesadores de multimedia

AMS ofrece dos codificadores a petición **Media Encoder Standard** y **Flujo de trabajo premium de codificación de medios**. Para más información, consulte [Información general y comparación de los codificadores multimedia a petición de Azure](media-services-encode-asset.md).

|Nombre de procesador de multimedia|Status|Regions
|---|---|---|
|Media Encoder Estándar|GA|All|
|Flujo de trabajo del Codificador multimedia|GA|Todos excepto China|

## <a name="analytics-media-processors"></a>Procesadores de multimedia de Analytics

Análisis multimedia es una colección de componentes de voz y visión que facilita a las empresas y organizaciones obtener conocimiento útil de sus archivos de vídeo. Para más información, consulte [Información general de análisis de Azure Media Services](./legacy-components.md).

> [!NOTE]
> Algunos procesadores de multimedia de análisis se retirarán. Para ver las fechas de retirada, consulte el tema sobre [componentes heredados](legacy-components.md).

|Nombre de procesador de multimedia|Status|Region
|---|---|---|
|Azure Media Face Detector|Vista previa|All|
|Azure Media Indexer|GA|All|
|Azure Media Motion Detector|Vista previa|All|
|Azure Media OCR|Vista previa|All|
|Azure Media Redactor|GA|All|
|Azure Media Video Thumbnails|Vista previa|All|

## <a name="protection"></a>Protección

Microsoft Azure Media Services le permite proteger su contenido multimedia desde el momento en que deja el equipo a través de almacenamiento, procesamiento y entrega. Para más información, consulte [Protección de contenido de AMS](media-services-content-protection-overview.md).

|Cifrado|Status|Regions|
|---|---|---| 
|Storage|GA|All|
|Claves AES-128|GA|All|
|Fairplay|GA|All|
|PlayReady|GA|All|
|Widevine|GA|Todas, excepto Alemania, el Gobierno Federal y China.

> [!NOTE]
> Widevine es un servicio que ofrece Google Inc. y que está sujeto a los términos del servicio y la directiva de privacidad de Google, Inc.

## <a name="reserved-units-rus"></a>Unidades reservadas (RU)

El número de unidades reservadas de codificación aprovisionadas determina el número de tareas de medios que se pueden procesar de forma simultánea en una cuenta determinada.

Disponible en todas las regiones.

## <a name="reserved-unit-ru-type"></a>Tipo de unidad reservada (RU)

Una cuenta de Media Services está asociada con un tipo de unidad reservada que determina la rapidez con la que se completan las tareas de procesamiento multimedia. Puede elegir entre los siguientes tipos de unidad reservada: S1, S2 o S3.

|Nombre de tipo de RU|Status|Regions
|---|---|---|
|S1|GA|All|
|S2|GA|Todos, excepto Sur de Brasil e India occidental|
|S3|GA|Todos, excepto India occidental|

## <a name="next-steps"></a>Pasos siguientes

[Migración a Media Services, versión 3](../latest/media-services-overview.md)

## <a name="provide-feedback"></a>Envío de comentarios

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]