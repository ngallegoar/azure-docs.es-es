---
title: 'Diferencias entre las API de administración y las API de servicio: Azure Batch | Microsoft Docs'
description: Las API funcionan en los distintos niveles del servicio Azure Batch.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 181515c0f497af8ffadcb909c13e51a40bfbf3b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672759"
---
# <a name="service-level-and-management-level-apis"></a>API de nivel de servicio y de nivel de administración

Azure Batch tiene dos conjuntos de API, uno para el nivel de servicio y otro para el nivel de administración. La denominación suele ser similar pero devuelven resultados diferentes. Si desea los registros de actividad, debe usar las API de administración. Las API de nivel de servicio omiten la capa de administración de recursos de Azure y no se registran.


La administración de Batch y el servicio Batch tienen API para el grupo, por ejemplo. 
- Esta API para eliminar el grupo se dirige directamente a la cuenta de Batch: https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- Esta API para eliminar el grupo https://docs.microsoft.com/rest/api/batchmanagement/pool/delete se dirige a la capa management.azure.com.

