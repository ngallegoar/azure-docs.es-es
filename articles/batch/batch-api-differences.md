---
title: Diferencias entre las API de administración y las API de servicio
description: Las API funcionan en los distintos niveles del servicio Azure Batch.
ms.topic: conceptual
ms.date: 02/26/2020
ms.custom: seodec18
ms.openlocfilehash: 33b8c5980aba1090155d6b136c6707e928666abf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82115404"
---
# <a name="service-level-and-management-level-apis"></a>API de nivel de servicio y de nivel de administración

Azure Batch tiene dos conjuntos de API, uno para el nivel de servicio y otro para el nivel de administración. La denominación suele ser similar pero devuelven resultados diferentes. Si desea los registros de actividad, debe usar las API de administración. Las API de nivel de servicio omiten la capa de administración de recursos de Azure y no se registran.


La administración de Batch y el servicio Batch tienen API para el grupo, por ejemplo. 
- Esta API para eliminar el grupo se dirige directamente a la cuenta de Batch: https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- Esta API para eliminar el grupo https://docs.microsoft.com/rest/api/batchmanagement/pool/delete se dirige a la capa management.azure.com.

