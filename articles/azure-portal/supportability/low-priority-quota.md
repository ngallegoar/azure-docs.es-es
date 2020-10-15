---
title: 'Cuota de máquina virtual al contado: Azure'
description: Aumente los límites de cuota de las máquinas virtuales al contado, que proporcionan un modelo de uso de Azure que le permite asumir costos menores a cambio de dejar que Azure quite las máquinas virtuales según sea necesario.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 513bde930f9c7c8892164c50c3866ff6e948ed4e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84763880"
---
# <a name="spot-quota-increase-limits-for-all-vm-series"></a>Cuota al contado: aumento de los límites para todas las series de máquinas virtuales

Las máquinas virtuales (VM) al contado ofrecen un modelo de uso de Azure diferente. Permiten asumir costos menores a cambio de que Azure pueda quitar máquinas virtuales según sea necesario para las implementaciones de pago por uso o instancia reservada de máquina virtual. Para obtener más información acerca de las máquinas virtuales al contado, consulte [Máquinas virtuales de Azure Spot para los conjuntos de escalado](../../virtual-machine-scale-sets/use-spot.md).

Azure Resource Manager admite dos tipos de cuotas de vCPU para las máquinas virtuales:

* Las *máquinas virtuales de pago por uso* y las *instancias reservadas de máquina virtual* están sujetas a una *cuota de vCPU estándar*.
* Las *máquinas virtuales al contado* están sujetas a una *cuota de vCPU al contado*.

Para el tipo de cuota de vCPU al contado, las cuotas de vCPU de Resource Manager se aplican a todas las series de máquina virtual disponibles como un límite regional único.

Cada vez que se implementa una nueva máquina virtual al contado, el total de uso de vCPU nuevo y existente para todas las instancias de máquina virtual al contado no debe superar el límite de cuota de vCPU al contado aprobado. Si se supera la cuota al contado, no se permite la implementación de máquinas virtuales al contado.

En este artículo se explica cómo solicitar un aumento en el límite de cuota de vCPU al contado con Azure Portal.

Para más información sobre las cuotas de vCPU estándar, consulte [Cuotas de vCPU de máquina virtual](../../virtual-machines/windows/quotas.md) y [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md).

Para obtener información sobre cómo aumentar el límite de vCPU por región, consulte [Cuota estándar: aumento de los límites por región](regional-quota-requests.md).

## <a name="request-a-quota-limit-increase-from-help--support"></a>Solicitud de un aumento del límite de cuota desde Ayuda y soporte técnico

Para solicitar un aumento del límite de cuota al contado en todas las series de máquina virtual con **Ayuda y soporte técnico**:

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

1. En **Detalles de la cuota**, realice los pasos siguientes:

   1. En **Modelo de implementación**, seleccione el modelo adecuado y, en **Ubicaciones**, seleccione una ubicación.

      ![Proporcionar detalles adicionales de la cuota](./media/resource-manager-core-quotas-request/quota-details-deployment-locations.png)

   1. En la ubicación seleccionada, en **Tipos**, en **Seleccionar un tipo**, elija **Al contado**.

      ![Selección del tipo al contado](./media/resource-manager-core-quotas-request/select-spot-type.png)

       En **Tipos**, puede solicitar los dos tipos de cuota (Estándar y Al contado) desde una única solicitud de soporte técnico mediante la selección múltiple.

       Para más información, consulte [Cuota estándar: Aumento de los límites por serie de máquinas virtuales](per-vm-quota-requests.md).

   1. Escriba el nuevo límite de cuota que quiere para esta suscripción.

      ![Selección de una nueva cuota para la máquina virtual al contado](./media/resource-manager-core-quotas-request/spot-set-new-quota.png)

1. Para solicitar un aumento de cuota en más de una ubicación, seleccione otra ubicación en **Ubicaciones** y, luego, seleccione un tipo de máquina virtual adecuado. Después, puede especificar el límite que se aplicará a la ubicación adicional.

   ![Especificación de ubicaciones adicionales en los detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-a-quota-limit-increase-from-the-subscriptions-pane"></a>Solicitud de un aumento del límite de cuota desde el panel Suscripciones

Para solicitar un aumento del límite de cuota al contado en todas las series de máquina virtual desde el panel **Suscripciones**, haga lo siguiente:

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

1. Seleccione **Siguiente: Soluciones** para abrir **DETALLES DEL PROBLEMA**. Seleccione **Proporcionar detalles** para especificar información adicional. En **Detalles de la cuota**, escriba la siguiente información:

   1. En **Modelo de implementación**, seleccione el modelo adecuado y, en **Ubicaciones**, seleccione una ubicación.

      ![Proporcionar los detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-deployment-locations.png)

   1. En la ubicación seleccionada, en **Tipos**, en **Seleccionar un tipo**, elija **Al contado**.

      ![Selección del tipo al contado](./media/resource-manager-core-quotas-request/select-spot-type.png)

      Para más información, consulte [Cuota estándar: Aumento de los límites por serie de máquinas virtuales](per-vm-quota-requests.md).

   1. Escriba el nuevo límite de cuota que quiere para esta suscripción.

      ![Escribir un nuevo valor para el límite de vCPU](./media/resource-manager-core-quotas-request/spot-set-new-quota.png)

1. Para solicitar un aumento de cuota en más de una ubicación, seleccione otra ubicación en **Ubicaciones** y, luego, seleccione un tipo de máquina virtual adecuado. Después, puede especificar el límite que se aplicará a la ubicación adicional.

   ![Selección de ubicaciones adicionales en los detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-multiple-locations.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.
