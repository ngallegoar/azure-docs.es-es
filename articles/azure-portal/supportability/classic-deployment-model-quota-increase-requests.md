---
title: modelo de implementación clásica de Azure
description: El modelo de implementación clásica, que ahora se ha sustituido por el modelo de Resource Manager, aplica un límite de cuota de vCPU global para las máquinas virtuales y los conjuntos de escalado de máquinas virtuales.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: a3d5106cafc1d3bfe77f3e42e85cedb668fc4fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76835632"
---
# <a name="classic-deployment-model"></a>Modelo de implementación clásica

El modelo de implementación clásica es el modo de implementación de Azure de la generación anterior. Este modelo impone un límite de cuota de vCPU global para máquinas virtuales y conjuntos de escalado de máquinas virtuales. El modelo de implementación clásica ya no se recomienda y ahora se reemplaza por el modelo de Resource Manager.

Para obtener más información sobre estos dos modelos de implementación y las ventajas de usar Resource Manager, consulte [Implementación de Resource Manager y clásica](../../azure-resource-manager/management/deployment-models.md).

Cuando se crea una nueva suscripción, se asigna una cuota predeterminada de vCPU a ella. Cada vez que se implementa una nueva máquina virtual mediante el modelo de implementación clásica, la suma del uso de vCPU nuevas y existentes en todas las regiones no debe superar la cuota de vCPU aprobada para dicho modelo.

Para aprender más sobre estas cuotas, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md).

Puede solicitar un aumento del límite de cuota de vCPU para el modelo de implementación clásica. Use **Ayuda y soporte técnico** o **Uso y cuotas** en Azure Portal.

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-help--support"></a>Solicitud de un aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción mediante Ayuda y soporte técnico

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante **Ayuda y soporte técnico** en Azure Portal.

1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![Selección de Ayuda y soporte técnico en Azure Portal](./media/resource-manager-core-quotas-request/help-plus-support.png)

1. Seleccione **Nueva solicitud de soporte técnico**.

   ![Creación de una solicitud de soporte técnico en Azure Portal](./media/resource-manager-core-quotas-request/new-support-request.png)

1. En **Tipo de problema**, elija **Límites de servicio y suscripción (cuotas)** .

   ![Selección de cuotas como el tipo de problema](./media/resource-manager-core-quotas-request/select-quota-issue-type.png)

1. Seleccione la suscripción cuya cuota quiere aumentar.

   ![Selección de la suscripción para la que se va a aumentar una cuota](./media/resource-manager-core-quotas-request/select-subscription-support-request.png)

1. Seleccione **Tipo de cuota** y, luego, **Compute-VM (cores-vCPUs) subscription limit increases** (Aumento del límite de suscripción para VM de proceso [núcleos/vCPU]).

   ![Selección del tipo de cuota que se va a aumentar](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. Seleccione **Siguiente: Soluciones** para abrir **DETALLES DEL PROBLEMA**. Seleccione **Proporcionar detalles** para proporcionar información adicional.

   ![Especificación de detalles para ayudar a la solicitud](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. En **Detalles de la cuota**, seleccione **Clásico** y elija una **Ubicación**.

   ![Adición de detalles, incluido el modelo de implementación y la ubicación](./media/resource-manager-core-quotas-request/quota-details-classic.png)

1. Para **Familia de SKU**, seleccione una o varias familias de SKU para aplicarles el aumento.

   ![Especificación de la familia de SKU que se va a aumentar](./media/resource-manager-core-quotas-request/sku-family-classic.png)

1. Escriba los nuevos límites que quiere en la suscripción. Para eliminar una línea, anule la selección de SKU en **Familia de SKU** o seleccione el icono de descarte "X". Después de especificar una cuota para cada familia de SKU, seleccione **Guardar y continuar** en **Detalles de la cuota** para continuar con la solicitud de soporte técnico.

   ![Solicitud de nuevos límites](./media/resource-manager-core-quotas-request/new-limits-classic.png)

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-usage--quotas"></a>Solicitud de aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción mediante Usos y cuotas

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante **Uso y cuotas** en Azure Portal.

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Suscripciones**.

   ![Navegación a Suscripciones en Azure Portal](./media/resource-manager-core-quotas-request/search-for-subscriptions.png)

1. Seleccione la suscripción cuya cuota quiere aumentar.

   ![Selección de la suscripción que se va a modificar](./media/resource-manager-core-quotas-request/select-subscription-change-quota.png)

1. Seleccione **Uso y cuotas**.

   ![Selección de uso y cuotas para una suscripción](./media/resource-manager-core-quotas-request/select-usage-plus-quotas.png)

1. En la esquina superior derecha, seleccione **Solicitar aumento**.

   ![Selección para aumentar la cuota](./media/resource-manager-core-quotas-request/request-increase-from-subscription.png)

1. Seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (Aumento del límite de suscripción para VM de proceso [núcleos/vCPU]) como **Tipo de cuota**.

   ![Selección del tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. Seleccione **Siguiente: Soluciones** para abrir **DETALLES DEL PROBLEMA**. Seleccione **Proporcionar detalles** para proporcionar información adicional.

   ![Especificación de detalles para la solicitud](./media/resource-manager-core-quotas-request/provide-details-link.png)

1. En **Detalles de la cuota**, seleccione **Clásico** y una **Ubicación**.

   ![Selección de detalles de la cuota, incluido el modelo de implementación y la ubicación](./media/resource-manager-core-quotas-request/quota-details-classic.png)

1. Seleccione una o varias familias de SKU para el aumento.

   ![Selección de la familia de SKU para el aumento](./media/resource-manager-core-quotas-request/sku-family-classic.png)

1. Escriba los nuevos límites que quiere en la suscripción. Para eliminar una línea, anule la selección de SKU en **Familia de SKU** o seleccione el icono de descarte "X". Después de especificar una cuota para cada familia de SKU, seleccione **Guardar y continuar** en **Detalles de la cuota** para continuar con la solicitud de soporte técnico.

   ![Escritura de una nueva cuota](./media/resource-manager-core-quotas-request/new-limits-classic.png)

