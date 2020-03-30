---
title: Reintento de tareas después de un error
description: Compruebe si hay errores y vuelva a intentarlo.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-batch
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/15/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 94ed936e619461a2dbf7ec837c2d80e21c01c88e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919998"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Detección y control de errores en el servicio Batch

Es importante recordar comprobar si hay errores al trabajar con una API del servicio REST. No es raro que se produzcan errores al ejecutar trabajos por lotes.

## <a name="common-errors"></a>Errores comunes 

- Errores de redes: se trata de solicitudes que nunca llegaron a Batch o que la respuesta de Batch no llegó al cliente a tiempo.
- Errores internos del servidor: son las respuestas con código de estado HTTP 5xx estándar.
- La limitación puede producir errores como respuestas con código de estado HTTP 429 o 503 con el encabezado Retry-after.
- Los errores 4xx que incluyen errores como AlreadyExists e InvalidOperation. Esto significa que el recurso no está en el estado correcto para la transición de estado.

Para información detallada sobre los distintos tipos de códigos de error y los códigos de error específicos, vea [Estado del lote y códigos de error](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).

## <a name="when-to-retry"></a>Cuándo se debe reintentar

Las API de Batch le notificarán si se produce un error. Todos pueden volver a intentarse y todos incluyen un controlador de reintentos global para ese propósito. Es mejor utilizar este mecanismo integrado.

Después de un error, debe esperar un poco (varios segundos entre reintentos) antes de volver a intentarlo. Si realiza reintentos con demasiada frecuencia o demasiado rápido, el controlador de reintentos lo limitará.

### <a name="for-more-information"></a>Para obtener más información  

Vínculos a [API y herramientas de Batch](batch-apis-tools.md) e información de referencia de las API. La API .NET, por ejemplo, tiene una [clase RetryPolicyProvider]( https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet) donde debe especificarse la directiva de reintentos necesaria. 

Para obtener información detallada sobre cada API y sus directivas de reintento predeterminadas, consulte [Códigos de estado y de error de Batch](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).
