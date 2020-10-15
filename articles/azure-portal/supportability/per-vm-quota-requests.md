---
title: Solicitud de aumento de los límites de cuota de vCPU por serie de máquinas virtuales de Azure
description: Cómo solicitar un aumento del límite de cuota de vCPU de una serie de máquinas virtuales en Azure Portal, que aumenta el límite de vCPU regional total en la misma cantidad.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 26fc916a2bacc0f6ae9791a53b642e93bb031466
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92077990"
---
# <a name="standard-quota-increase-limits-by-vm-series"></a>Cuota estándar: Aumento de los límites por serie de máquinas virtuales

Azure Resource Manager admite dos tipos de cuotas de vCPU para las máquinas virtuales:

* Las *máquinas virtuales de pago por uso* y las *instancias reservadas de máquina virtual* están sujetas a una *cuota de vCPU estándar*.
* Las *máquinas virtuales al contado* están sujetas a una *cuota de vCPU al contado*.

La cuota de vCPU estándar para las instancias reservadas de máquina virtual y de pago por uso se aplica en dos niveles para cada suscripción de cada región:

* El primer nivel es el *límite de vCPU regionales totales*, en todas las series de máquinas virtuales.
* El segundo nivel es el *límite de vCPU por serie de máquinas virtuales*; por ejemplo, las vCPU de la serie Dv3.

Cada vez que se implementa una nueva máquina virtual al contado, el total de uso de vCPU nuevo y existente para todas las instancias de máquina virtual al contado no debe superar el límite de cuota de vCPU al contado aprobado. Si se supera la cuota al contado, no se permite la implementación de máquinas virtuales al contado.

Puede solicitar un aumento del límite de cuota de vCPU para la serie de máquinas virtuales a través de Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad.

Para más información sobre las cuotas de vCPU estándar, consulte los artículos [Cuotas de vCPU de máquinas virtuales](../../virtual-machines/windows/quotas.md) y [Suscripción a Azure y límites de servicio](./classic-deployment-model-quota-increase-requests.md).

Para información sobre cómo aumentar el límite de vCPU por región para la cuota estándar, consulte [Cuota estándar: aumento de los límites por región](regional-quota-requests.md).

Para más información sobre cómo aumentar los límites de vCPU de máquinas virtuales al contado, consulte el artículo [Cuota al contado: aumento de los límites para todas las series de máquinas virtuales](low-priority-quota.md).

Puede solicitar un aumento de los límites de cuota de vCPU estándar por serie de máquinas virtuales de dos maneras, tal como se describe en las secciones siguientes.

## <a name="request-a-standard-quota-increase-from-help--support"></a>Solicitud de un aumento de la cuota estándar desde Ayuda y soporte técnico

Para solicitar un aumento de cuota de vCPU estándar por serie de máquinas virtuales desde **Ayuda y soporte técnico**, haga lo siguiente:

> [!NOTE]
> También puede solicitar un aumento del límite de cuota para varias regiones a través de un único caso de soporte técnico. Para más información, consulte el paso 8.

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. Seleccione **Ayuda y soporte técnico** y **Nueva solicitud de soporte técnico**.

    ![Creación de una solicitud de soporte técnico](./media/resource-manager-core-quotas-request/new-support-request.png)

1. En **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Selección de un tipo de problema](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. En **Suscripción**, seleccione la suscripción cuya cuota quiere aumentar.

   ![Selección de una suscripción para el aumento de cuota](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. Seleccione **Tipo de cuota** y, luego, **Compute-VM (cores-vCPUs) subscription limit increases** (Aumento del límite de suscripción para VM de proceso [núcleos/vCPU]).

   ![Selección de un tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. Seleccione **Siguiente: Soluciones** para abrir **DETALLES DEL PROBLEMA**. Seleccione **Proporcionar detalles** para especificar información adicional.

   ![El vínculo "Proporcionar detalles"](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. En los **Detalles de la cuota**, realice los pasos siguientes:

   ![Proporcionar detalles adicionales de la cuota](./media/resource-manager-core-quotas-request/quota-details-deployment-rm-locations.png)

   1. Para el **Modelo de implementación**, seleccione el adecuado.

   1. Para las **Ubicaciones**, seleccione una. En la ubicación seleccionada, en **Tipos**, en **Seleccionar un tipo**, elija **Estándar**.

      ![Detalles de la cuota: tipos de cuota](./media/resource-manager-core-quotas-request/quota-details-select-standard-type.png)

      En **Tipos**, puede solicitar los dos tipos de cuota (Estándar y Al contado) desde una única solicitud de soporte técnico mediante la selección múltiple.

      Para más información sobre cómo aumentar los límites de cuota al contado, consulte [Máquinas virtuales de Azure Spot para los conjuntos de escalado](../../virtual-machine-scale-sets/use-spot.md).

   1. En **Estándar**, seleccione la serie de SKU para el aumento de cuotas.

      ![Detalles de la cuota: serie de SKU](./media/resource-manager-core-quotas-request/quota-details-standard-select-series.png)

   1. Escriba el nuevo límite de cuota que quiere para esta suscripción. Para quitar una SKU de la lista, desactive la casilla junto a la SKU o seleccione el icono de descarte "X".

      ![Selección de un nuevo límite de vCPU](./media/resource-manager-core-quotas-request/quota-details-standard-set-vcpu-limit.png)

1. Para solicitar un aumento de cuota en más de una ubicación, seleccione una ubicación adicional en **Ubicaciones** y, luego, seleccione un tipo de máquina virtual adecuado. Después, puede especificar el límite que se aplicará a la ubicación adicional.

   ![Especificación de ubicaciones adicionales en los detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-a-standard-quota-increase-from-subscriptions"></a>Solicitud de un aumento de la cuota estándar desde Suscripciones

Para solicitar un aumento de cuota de vCPU estándar por serie de máquinas virtuales desde **Suscripciones**, haga lo siguiente:

> [!NOTE]
> También puede solicitar un aumento del límite de cuota para varias regiones a través de un único caso de soporte técnico. Para más información, consulte el paso 7.

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Suscripciones**.

   ![Suscripciones en la búsqueda de Azure Portal](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Seleccione la suscripción cuya cuota quiere aumentar.

   ![Suscripciones que puede seleccionar para los cambios](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. En el panel izquierdo, seleccione **Uso y cuotas**.

   ![El vínculo "Uso y cuotas"](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. En la parte superior derecha, seleccione **Solicitar aumento**.

   ![Selección para aumentar la cuota](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. Seleccione **Tipo de cuota** y, luego, **Compute-VM (cores-vCPUs) subscription limit increases** (Aumento del límite de suscripción para VM de proceso [núcleos/vCPU]).

   ![Selección de un tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. En los **Detalles de la cuota**, realice los pasos siguientes:

   1. En **Modelo de implementación**, seleccione el modelo adecuado y, en **Ubicaciones**, seleccione una ubicación.

      ![Especificación de los detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-deployment-rm-locations.png)

   1. En la ubicación seleccionada, en **Tipos**, seleccione **Seleccionar un tipo** y, a continuación, **Estándar**.

      ![Selección del tipo estándar](./media/resource-manager-core-quotas-request/quota-details-select-standard-type.png)

      En **Tipos**, puede solicitar los dos tipos de cuota (Estándar y Al contado) desde una única solicitud de soporte técnico mediante la selección múltiple.

      Para más información sobre cómo aumentar los límites de cuota al contado, consulte [Máquinas virtuales de Azure Spot para los conjuntos de escalado](../../virtual-machine-scale-sets/use-spot.md).

   1. Para **Estándar**, seleccione la serie de SKU cuyas cuotas quiere aumentar.

      ![Detalles de la cuota: serie de SKU](./media/resource-manager-core-quotas-request/quota-details-standard-select-series.png)

   1. Escriba el nuevo límite de cuota que quiere para esta suscripción. Para quitar una SKU de la lista, anule la selección de la casilla junto a la SKU o seleccione el icono de descarte "X".

      ![Selección de un nuevo límite de vCPU](./media/resource-manager-core-quotas-request/quota-details-standard-set-vcpu-limit.png)

1. Para solicitar un aumento de cuota en más de una ubicación, seleccione una ubicación adicional en **Ubicaciones** y, luego, seleccione un tipo de máquina virtual adecuado.

   Este paso carga previamente la serie de SKU seleccionada para las ubicaciones anteriores. Escriba los límites de cuota que quiere aplicar a la serie adicional.

   ![Selección de ubicaciones adicionales en los detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.