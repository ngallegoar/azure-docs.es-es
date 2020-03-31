---
title: Streaming de alta disponibilidad de Azure Media Services
description: Aprenda a conmutar por error a una cuenta secundaria de Media Services si se produce una interrupción o un error en un centro de datos regional.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: 1492dd392eabc4331f8e3d4604fb245a89dedff5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898990"
---
# <a name="media-services-high-availability-streaming"></a>Streaming de alta disponibilidad de Azure Media Services

Azure Media Services no proporciona actualmente conmutación por error instantánea del servicio si se produce una interrupción de un centro de datos regional o un error de componentes subyacentes o de servicios dependientes. En este artículo se proporcionan instrucciones sobre cómo crear streaming de vídeo a petición entre regiones.

## <a name="prerequisites"></a>Prerrequisitos

Revise [Compilación de un sistema de codificación entre regiones](media-services-high-availability-encoding.md).

## <a name="how-to-build-video-on-demand-cross-region-streaming"></a>Creación de streaming de vídeo a petición entre regiones 

* El streaming de vídeo a petición entre regiones implica la duplicación de [Recursos](assets-concept.md), [Directivas de clave de contenido](content-key-policy-concept.md) (si se utilizan), [Directivas de streaming](streaming-policy-concept.md) y [Localizadores de streaming](streaming-locators-concept.md). 
* Tendrá que crear las directivas en ambas regiones y mantenerlas actualizadas. 
* Al crear los localizadores de streaming, querrá usar el mismo valor de identificador de localizador, valor de identificador de ContentKey y valor de ContentKey.  
* Si está codificando el contenido, se recomienda codificar el contenido en la región A y publicarlo, luego copiar el contenido codificado en la región B y publicarlo con los mismos valores que en la región A.
* Puede usar Traffic Manager en los nombres de host para el origen y el servicio de entrega de claves (en la configuración de Media Services, esto se parecerá a una dirección URL de servidor de claves personalizadas).

## <a name="next-steps"></a>Pasos siguientes

Consulte:

* [Tutorial: Codificación de un archivo remoto según una dirección URL y transmisión del vídeo: REST](stream-files-dotnet-quickstart.md)
* [ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
