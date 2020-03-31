---
title: Análisis de streaming en vivo mediante Video Indexer
titleSuffix: Azure Media Services
description: En este artículo se muestra cómo realizar un análisis de streaming en vivo mediante Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/13/2019
ms.author: juliako
ms.openlocfilehash: 89d0254fc758834c437f347e6ecb7bcafc1fe467
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185992"
---
# <a name="live-stream-analysis-with-video-indexer"></a>Análisis de streaming en vivo con Video Indexer

Video Indexer de Azure Media Services es un servicio de Azure diseñado para extraer información detallada de archivos de vídeo y audio sin conexión. Se trata de analizar un archivo multimedia determinado que se ha creado de antemano. Sin embargo, en algunos casos de uso es importante obtener información multimedia a partir de una fuente dinámica lo más rápido posible para desbloquear casos de uso operativos y otros con presión de tiempo. Por ejemplo, los productores de contenido podrían usar estos metadatos enriquecidos de un streaming en vivo para automatizar la producción de TV.

Una solución que se describe en este artículo permite a los clientes usar Video Indexer en soluciones casi en tiempo real en fuentes en directo. El retraso en la indexación puede ser de tan solo cuatro minutos mediante esta solución, según los fragmentos de datos que se van a indexar, la resolución de entrada, el tipo de contenido y el proceso que se usa para este proceso.

![Metadatos de Video Indexer en el streaming en vivo](./media/live-stream-analysis/live-stream-analysis01.png)

*Figura 1: reproductor de ejemplo que muestra los metadatos de Video Indexer en el streaming en vivo*

La [solución de análisis de streaming](https://aka.ms/livestreamanalysis) disponible, usa Azure Functions y dos aplicaciones lógicas para procesar un programa en vivo de un canal en directo de Azure Media Services con Video Indexer y muestra el streaming resultante casi en tiempo real con Azure Media Player.

A nivel general, se compone de dos pasos principales. El primer paso se ejecuta cada 60 segundos y toma un subclip de los últimos 60 segundos reproducidos, crea un recurso a partir de él y lo indexa mediante Video Indexer. Después, se llama al segundo paso una vez completada la indexación. La información capturada se procesa, se envía a Azure Cosmos DB y se elimina el subclip indexado.

El reproductor de ejemplo reproduce el streaming en vivo y obtiene la información de Azure Cosmos DB mediante una función de Azure dedicada. Muestra los metadatos y las miniaturas sincronizados con el vídeo en directo.

![Las dos aplicaciones lógicas procesando el streaming en vivo cada minuto en la nube](./media/live-stream-analysis/live-stream-analysis02.png)

*Figura 2: las dos aplicaciones lógicas procesando el streaming en vivo cada minuto en la nube.*

## <a name="step-by-step-guide"></a>Guía paso a paso 

Puede encontrar una guía paso a paso para implementar los resultados en este [proyecto de GitHub para el análisis de elementos multimedia en vivo con Video Indexer](https://aka.ms/livestreamanalysis). 

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Video Indexer](video-indexer-overview.md)
