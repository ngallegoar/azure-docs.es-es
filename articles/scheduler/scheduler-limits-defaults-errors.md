---
title: Límites, cuotas y umbrales de Azure Scheduler
description: Obtenga información sobre los límites, cuotas, valores predeterminados y umbrales de limitación de Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 610232dab776648bb3dcc7c301ec292e9acad9fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898522"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Límites, cuotas y umbrales de limitación de Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

## <a name="limits-quotas-and-thresholds"></a>Límites, cuotas y umbrales

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>Encabezado x-ms-request-id

Cada solicitud realizada al servicio Scheduler devuelve un encabezado de respuesta llamado**x-ms-request-id**. Este encabezado contiene un valor opaco que identifica de forma única la solicitud. Por lo tanto, si una solicitud falla constantemente y ha confirmado que la solicitud tiene un formato apropiado, puede notificar el error a Microsoft y proporcionar el valor del encabezado de respuesta **x-ms-request-id** e incluir estos detalles: 

* El valor de **x-ms-request-id**
* La hora aproximada en la que se realizó la solicitud 
* Los identificadores de la suscripción de Azure, la colección de trabajos y el trabajo 
* El tipo de operación que intenta realizar la solicitud

## <a name="next-steps"></a>Pasos siguientes

* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
* [Planes y facturación de Azure Scheduler](scheduler-plans-billing.md)
* [Referencia de API de REST de Azure Scheduler](/rest/api/scheduler)
* [Referencia de cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md)