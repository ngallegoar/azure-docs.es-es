---
title: archivo de inclusión
description: archivo de inclusión
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/28/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 22bfc3c86605f4c2eed4c022919b3643f394ea95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "67080920"
---
| **Recurso** | **Límite predeterminado** | **Límite máximo** |
| --- | --- | --- |
| Cuentas de Azure Batch por región y suscripción | 1-3 |50 |
| Núcleos dedicados por cuenta de Batch | 90-900 | Ponerse en contacto con soporte técnico |
| Núcleos de baja prioridad por cuenta de Batch | 10-100 | Ponerse en contacto con soporte técnico |
| Trabajos **[activos](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** y programaciones de trabajos por cuenta de Batch (los trabajos **completados** no tienen ningún límite) | 100-300 | 1000<sup>1</sup> |
| Grupos por cuenta de Batch | 20-100 | 500<sup>1</sup> |

> [!NOTE]
> Los límites predeterminados varían según el tipo de suscripción que se use para crear una cuenta de Batch. Las cuotas de núcleos mostradas son para las cuentas de Batch del modo de servicio Batch. [Vea las cuotas de su cuenta de Batch](../articles/batch/batch-quota-limit.md#view-batch-quotas).

<sup>1</sup> Si quiere solicitar un aumento de este límite, póngase en contacto con el soporte técnico de Azure.
