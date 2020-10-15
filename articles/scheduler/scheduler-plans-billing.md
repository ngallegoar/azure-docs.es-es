---
title: Planes y facturación
description: Obtenga información acerca de los planes y la facturación de Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: e821036ff4ddb5a9786bc4f4537bb81539ab2c87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78898476"
---
# <a name="plans-and-billing-for-azure-scheduler"></a>Planes y facturación de Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

## <a name="job-collection-plans"></a>Planes de colección de trabajos

En Azure Scheduler, una colección de trabajos contiene un número específico de trabajos. La colección de trabajos es la entidad facturable y viene en los planes Estándar, P10 Premium y P20 Premium, que se describen aquí: 

| Plan de colección de trabajos | Nº máx. de trabajos por colección | Periodicidad máxima | N.º máximo de colecciones de trabajos por suscripción | límites | 
|:--- |:--- |:--- |:--- |:--- |
| **Estándar** | 50 trabajos por colección | Uno por minuto. No se puede ejecutar trabajos con más frecuencia que uno por minuto. | Cada suscripción de Azure puede tener un máximo de 100 colecciones de trabajos Estándar. | Acceso al conjunto completo de características de Scheduler | 
| **Premium P10** | 50 trabajos por colección | Uno por minuto. No se puede ejecutar trabajos con más frecuencia que uno por minuto. | Cada suscripción de Azure puede tener un máximo de 10 000 colecciones de trabajos P10 Premium. Para más colecciones, <a href="mailto:wapteams@microsoft.com">póngase en contacto con nosotros</a>. | Acceso al conjunto completo de características de Scheduler |
| **Premium P20** | 1000 trabajos por colección | Uno por minuto. No se puede ejecutar trabajos con más frecuencia que uno por minuto. | Cada suscripción de Azure puede tener un máximo de 5 000 colecciones de trabajos P20 Premium. Para más colecciones, <a href="mailto:wapteams@microsoft.com">póngase en contacto con nosotros</a>. | Acceso al conjunto completo de características de Scheduler |
|||||| 

## <a name="pricing"></a>Precios

Para obtener información detallada sobre los precios, consulte [Precios de Scheduler](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="upgrade-or-downgrade-plans"></a>Actualizar o degradar planes

En los planes Estándar, P10 Premium y P20 Premium los planes de colección de trabajos se pueden actualizar o cambiar a una versión anterior en cualquier momento.

## <a name="active-status-and-billing"></a>Estado activo y facturación

Las colecciones de trabajos están siempre activas, salvo que toda la suscripción a Azure pase a un estado deshabilitado temporal por problemas de facturación. Y aunque todos los trabajos de una colección se pueden deshabilitar con una sola operación, esta acción no cambia el estado de facturación de la colección, por lo que la colección se *sigue* facturando. Las colecciones de trabajos vacías se consideran activas y se facturan.

Para asegurarse de que a una colección de trabajos no se factura, es preciso eliminarla.

## <a name="standard-billable-units"></a>Unidades facturables estándar

Una unidad facturable estándar puede tener un máximo de 10 colecciones de trabajos Estándar. Como una colección de trabajos Estándar puede tener hasta 50 trabajos, una unidad de facturación estándar permite que una suscripción a Azure tenga un máximo de 500 trabajos, es decir, casi 22 *millones* de ejecuciones de trabajos al mes. En esta lista se explica cómo se factura en función del número de colecciones de trabajos Estándar:

* Si tiene entre 1 y 10 colecciones de trabajos Estándar, se le factura una unidad de facturación estándar. 

* Si tiene entre 11 y 20 colecciones de trabajos Estándar, se le facturan dos unidades de facturación estándar. 

* Si tiene entre 21 y 30 colecciones de trabajos Estándar, se le facturan tres unidades de facturación estándar, y así sucesivamente.

## <a name="p10-premium-billable-units"></a>Unidades facturables del plan Premium P10

Una unidad facturable Premium P10 puede tener un máximo de 10 000 colecciones de trabajos Premium P10. Como una colección de trabajos Premium P10 puede tener hasta 50 trabajos por colección de trabajos, una unidad de facturación Premium P10 permite que una suscripción a Azure tenga hasta 500 000 trabajos, es decir, casi 22 000 *millones* de ejecuciones de trabajos al mes. 

Las colecciones de trabajos Premium P10 proporcionan las mismas funcionalidades que las Estándar, pero ofrecen un descuento para las aplicaciones que requieren muchas colecciones de trabajos y proporcionan mayor escalabilidad. En esta lista se explica cómo se factura en función del número de colecciones de trabajos Premium P10:

* Si tiene entre 1 y 10 000 colecciones de trabajos Premium P10, se le facturará una unidad de facturación Premium P10. 

* Si tiene entre 10 001 y 20 000 colecciones de trabajos Premium P10, se le facturarán 2 unidades de facturación Premium P10., y así sucesivamente.

## <a name="p20-premium-billable-units"></a>Unidades facturables Premium P20

Una unidad facturable Premium P20 puede incluir un máximo de 5000 colecciones de trabajos Premium P20. Como una colección de trabajos Premium P20 puede tener hasta 1000 trabajos por colección de trabajos, una unidad de facturación Premium P20 permite que una suscripción a Azure tenga hasta 5 000 000 trabajos, es decir, casi 220 000 *millones* de ejecuciones de trabajos al mes.

Las colecciones de trabajos Premium P20 proporcionan las mismas funcionalidades que las Premium P10, pero también admiten un mayor número de trabajos por colección de trabajos y un mayor número total de trabajos globales que las Premium P10, por lo que proporciona más escalabilidad.

## <a name="plan-comparison"></a>Comparación de planes

* Si tiene más de 100 colecciones de trabajos Estándar (10 unidades de facturación estándar), le resulta mejor tener todas ellas en un plan Premium.

* Si tiene una colección de trabajos Estándar y una colección de trabajos Premium, se le factura una unidad de facturación estándar *y* una unidad de facturación premium.

  Las facturas del servicio Scheduler se basan en el número de colecciones de trabajos activas que sean Estándar o Premium.

## <a name="next-steps"></a>Pasos siguientes

* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
* [Límites, valores predeterminados y códigos de error de Azure Scheduler](scheduler-limits-defaults-errors.md)