---
title: Requisitos de consulta
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 68b5946cb21bef1c8979a94c8780682cfe9defa6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376936"
---
Dentro de la consulta, use el parámetro `@StartTime` para obtener datos de métricas para una marca de tiempo determinada. Se reemplazará por una cadena de formato `yyyy-MM-ddTHH:mm:ss`. 

> [!IMPORTANT]
> Asegúrese de que la consulta solo devolverá los datos de métricas de **una marca de tiempo única**. Metrics Advisor ejecutará la consulta en todas las marcas de tiempo para obtener los datos de métricas correspondientes. Por ejemplo, una consulta para una métrica con granularidad *diaria* solo debe contener una marca de tiempo única, como `2020-06-21T00:00:00Z` al ejecutar la consulta una vez. 
