---
title: Análisis de Azure Batch
description: Los temas de análisis por lotes contienen información de referencia para los eventos y las alertas disponibles para los recursos del servicio de Batch.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: d9538461dfa48a961f9325566b2adaf265e8daeb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025969"
---
# <a name="batch-analytics"></a>Análisis por lotes
Los temas de análisis por lotes contienen información de referencia para los eventos y las alertas disponibles para los recursos del servicio de Batch.

Vea [Registro de diagnóstico de Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) para obtener más información sobre cómo habilitar y consumir los registros de diagnóstico por lotes.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

El servicio de Azure Batch genera los siguientes eventos de registro de diagnóstico mientras duren determinados recursos de Batch.

**Eventos del registro del servicio**
* [Creación del grupo](batch-pool-create-event.md)
* [Inicio de eliminación del grupo](batch-pool-delete-start-event.md)
* [Finalización de eliminación del grupo](batch-pool-delete-complete-event.md)
* [Inicio de cambio de tamaño del grupo](batch-pool-resize-start-event.md)
* [Finalización de cambio de tamaño del grupo](batch-pool-resize-complete-event.md)
* [Inicio de tarea](batch-task-start-event.md)
* [Finalización de tarea](batch-task-complete-event.md)
* [Error en la tarea](batch-task-fail-event.md)