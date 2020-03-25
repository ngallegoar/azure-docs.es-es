---
title: Estados de la suscripción de Azure
description: En este artículo se describen los diferentes estados de una suscripción de Azure.
keywords: estado de la suscripción de Azure
author: anuragdalmia
ms.reviewer: banders
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: andalmia
ms.openlocfilehash: 29af3d064b481548cdb9b9518e9735eb34aaf034
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78270233"
---
# <a name="azure-subscription-states"></a>Estados de la suscripción de Azure

En este artículo se describen los distintos estados que puede tener una suscripción de Azure. Estos estados se mostrarán como **Estado** en las áreas de suscripción de Azure Portal.

| Estado de la suscripción | Descripción |
|-------------| ----------------|
| **Activo** | La suscripción de Azure está activa. La suscripción se puede usar para implementar recursos nuevos y administrar los existentes.|
| **Eliminado** | La suscripción de Azure se ha eliminado junto con todos los recursos o datos subyacentes. |
| **Deshabilitada** | La suscripción de Azure está deshabilitada y ya no se puede usar para crear o administrar recursos de Azure. Mientras se está en este estado, las máquinas virtuales se desasignan, las direcciones IP temporales se liberan, el almacenamiento es de solo lectura y los demás servicios se deshabilitan. Una suscripción se puede deshabilitar por cualquiera de los siguientes motivos: El crédito puede haber expirado. Es posible que se haya alcanzado el límite de gasto. Hay una factura vencida. Se ha superado el límite de la tarjeta de crédito. O bien, se ha deshabilitado o cancelado explícitamente. En función del tipo suscripción, esta puede permanecer deshabilitada entre 1 y 90 días. Tras ese periodo se elimina de forma permanente. Para más información, consulte [Reactivación de una suscripción de Azure deshabilitada](subscription-disabled.md). |
| **Expired** | La suscripción de Azure ha expirado porque se ha cancelado. Las suscripciones que hayan expirado se pueden volver a activar. Para más información, consulte [Reactivación de una suscripción de Azure deshabilitada](subscription-disabled.md).|
| **Vencida** | La suscripción de Azure tiene un pago pendiente. La suscripción todavía está activa, pero si se produce un error al pagar los gastos, es posible que se deshabilite la suscripción. Para más información, consulte [Resolución del saldo vencido de una suscripción de Azure](resolve-past-due-balance.md). |
