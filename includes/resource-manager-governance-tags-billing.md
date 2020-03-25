---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 226079b1970508004c55c54cd3de6d2e9475aa08
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75970119"
---
Después de aplicar etiquetas a los recursos, puede ver los costos de los recursos con esas etiquetas. El análisis de costos tarda un tiempo en mostrar el uso más reciente, por lo que quizás no pueda ver los costos todavía. Cuando los costos están disponibles, puede ver los costos de los recursos a través de los grupos de recursos en su suscripción. Los usuarios deben tener [acceso de nivel de suscripción a la información de facturación](../articles/cost-management-billing/manage/manage-billing-access.md) para ver los costos.

Para ver los costos por etiqueta en el portal, seleccione su suscripción y seleccione **Análisis de costos**.

![Análisis de costos](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

A continuación, filtre por el valor de etiqueta y seleccione **Aplicar**.

![Ver costo por etiqueta](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

También puede usar las [API de facturación de Azure](../articles/cost-management-billing/manage/usage-rate-card-overview.md) para ver los costos mediante programación.
