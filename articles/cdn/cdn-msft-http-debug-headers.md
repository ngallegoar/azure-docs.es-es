---
title: Encabezados HTTP de depuración para Azure CDN de Microsoft | Microsoft Docs
description: Los encabezados de solicitud de caché de depuración proporciona información adicional sobre la directiva de caché aplicada al recurso solicitado. Estos encabezados son específicos para Azure CDN de Microsoft.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: allensu
ms.openlocfilehash: 2475bdce3ab8f153cc837601964bf4a2e90a470c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260421"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Encabezados HTTP de depuración para Azure CDN de Microsoft
El encabezado de respuesta de depuración, `X-Cache`, brinda detalles sobre la capa de la pila de CDN desde donde se sirvió el contenido. Este encabezado es específico para Azure CDN de Microsoft.

### <a name="response-header-format"></a>Formato del encabezado de respuesta

Encabezado | Descripción
-------|------------
X-Cache: TCP_HIT | Este encabezado se devuelve cuando el contenido se sirve desde la caché perimetral de CDN. 
X-Cache: TCP_REMOTE_HIT | Este encabezado se devuelve cuando el contenido se sirve desde la caché regional de CDN (capa de escudo de origen).
X-Cache: TCP_MISS | Este encabezado se devuelve cuando se produce un error de caché y el contenido se sirve desde el origen. 


