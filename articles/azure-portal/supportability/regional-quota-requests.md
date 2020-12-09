---
title: Solicitud de aumento del límite de cuota regional para vCPU de Azure
description: Cómo solicitar un aumento del límite de cuota de vCPU para una región en Azure Portal.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: eadf740c6b5caccbf678a1238f993d4ec0b34095
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2020
ms.locfileid: "96745424"
---
# <a name="standard-quota-increase-limits-by-region"></a>Cuota estándar: aumento de los límites por región

Azure Resource Manager admite dos tipos de cuotas de vCPU para máquinas virtuales:

* Las *máquinas virtuales de pago por uso* y las *instancias reservadas de máquina virtual* están sujetas a una *cuota de vCPU estándar*.
* Las *máquinas virtuales al contado* están sujetas a una *cuota de vCPU al contado*.

La cuota de vCPU estándar para las instancias reservadas de máquina virtual y de pago por uso se aplica en dos niveles para cada suscripción de cada región:

* El primer nivel es el *límite de vCPU regionales totales*, en todas las series de máquinas virtuales.
* El segundo nivel es el *límite de vCPU por serie de máquinas virtuales*; por ejemplo, las vCPU de la serie D.

Cada vez que implemente una nueva máquina virtual al contado, el uso total de las vCPU nuevas y existentes de esa serie de máquinas virtuales no deberá superar la cuota de vCPU aprobada para esa serie. Además, el número total de vCPU nuevas y existentes que se implementan en todas las series de máquinas virtuales no debe superar la cuota de vCPU regionales totales aprobada para la suscripción. Si se supera alguna de estas cuotas, no se permitirá la implementación de la máquina virtual.

Puede solicitar un aumento del límite de cuota de vCPU para la serie de máquinas virtuales a través de Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad.

Cuando crea una nueva suscripción, el número total predeterminado de vCPU regionales no puede ser igual a la suma de las cuotas de vCPU predeterminadas para todas las series individuales de máquinas virtuales. Esta discrepancia puede dar lugar a una suscripción con suficiente cuota para cada serie individual de máquinas virtuales que quiera implementar. Pero es posible que no haya cuota suficiente para el total de vCPU regionales de todas las implementaciones. En este caso, debe enviar una solicitud para aumentar explícitamente el límite del total de vCPU regionales. El límite de vCPU regionales totales no puede superar el total de la cuota aprobada en todas las series de máquinas virtuales de la región.

Para más información sobre las cuotas de vCPU estándar, consulte [Cuotas de vCPU de máquina virtual](../../virtual-machines/windows/quotas.md) y [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md).

Para más información sobre cómo aumentar los límites de vCPU de máquinas virtuales al contado, vea el artículo [Cuota al contado: aumento de los límites para todas las series de máquinas virtuales](low-priority-quota.md).

Puede solicitar un aumento del límite de cuota de vCPU estándar por región de dos maneras.

## <a name="request-a-quota-increase-by-region-from-help--support"></a>Solicitud de un aumento de la cuota por región desde Ayuda y soporte técnico

Para solicitar un aumento de la cuota de vCPU por región desde **Ayuda y soporte técnico**, haga lo siguiente:

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de "Ayuda y soporte técnico"](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. Seleccione **Ayuda y soporte técnico** y **Nueva solicitud de soporte técnico**.

    ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/new-support-request.png)

1. En **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Selección de un tipo de problema](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. En **Suscripción**, seleccione la suscripción cuya cuota quiere aumentar.

   ![Selección de una suscripción](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. En **Tipo de cuota**, seleccione **Otras solicitudes**.

   ![Selección de un tipo de cuota](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. Seleccione **Siguiente: Soluciones** para abrir **DETALLES DEL PROBLEMA**. En **Descripción**, proporcione la siguiente información:

    1. Especifique el **modelo de implementación**, en este caso **Resource Manager**.  
    1. Especifique la **región** que corresponda, por ejemplo, **Este de EE. UU. 2**.  
    1. Especifique el **nuevo límite** de vCPU para la región. Este valor no debería superar la suma de las cuotas aprobadas para las series de SKU individuales de esta suscripción.

    ![Escritura de los detalles de la solicitud de cuota](./media/resource-manager-core-quotas-request/regional-details.png)

1. Seleccione **Revisar y crear** para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-a-quota-increase-by-region-from-subscriptions"></a>Solicitud de aumento de la cuota por región desde Suscripciones

Para solicitar un aumento de la cuota de vCPU por región desde **Suscripciones**, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Suscripciones**.

   ![Navegación a Suscripciones en Azure Portal](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Seleccione la suscripción cuya cuota quiere aumentar.

   ![Selección de la suscripción que se va a modificar](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. En el panel izquierdo, seleccione **Uso y cuotas**.

   ![Uso del vínculo Uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. En la parte superior derecha, seleccione **Solicitar aumento**.

   ![Selección para aumentar la cuota](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. En la lista **Tipo de cuota**, seleccione **Otras solicitudes**.

   ![Selección del tipo de cuota](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. Seleccione **Siguiente: Soluciones** para abrir **DETALLES DEL PROBLEMA**. En el cuadro **Descripción**, proporcione la siguiente información adicional:

    1. Especifique el **modelo de implementación**, en este caso **Resource Manager**.  
    1. Especifique la **región** que corresponda, por ejemplo, **Este de EE. UU. 2**.  
    1. Especifique el **nuevo límite** de vCPU para la región. Este valor no debería superar la suma de las cuotas aprobadas para las series de SKU individuales de esta suscripción.

    ![Escritura de información en la sección de detalles](./media/resource-manager-core-quotas-request/regional-details.png)

1. Seleccione **Revisar y crear** para continuar con la creación de la solicitud de soporte técnico.
