---
title: Errores de solución de problemas de traslado
description: Solución de problemas al mover los recursos a un nuevo grupo de recursos o a una nueva suscripción
ms.topic: conceptual
ms.date: 08/27/2019
ms.openlocfilehash: 41b1e2435caf9874f3582a3394664c7b7f5a8d29
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90054169"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Solución de problemas al mover los recursos de Azure a una nueva suscripción o grupo de recursos

Este artículo contiene sugerencias que le ayudarán a resolver los problemas que pueden surgir al mover recursos.

## <a name="upgrade-a-subscription"></a>Actualización de una suscripción

Si quiere actualizar la suscripción de Azure (por ejemplo, para cambiar de gratuita a de pago por uso) deberá convertir su suscripción.

* Para actualizar a una evaluación gratuita, consulte [Actualización de la suscripción de Microsoft Imagine Azure o la prueba gratuita al plan de pago por uso](../../cost-management-billing/manage/upgrade-azure-subscription.md).
* Para cambiar a una cuenta de pago por uso, consulte [Cambie la suscripción de pago por uso de Azure a otra oferta](../../cost-management-billing/manage/switch-azure-offer.md).

Si no puede convertir la suscripción, [cree una solicitud de soporte técnico de Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). Seleccione **Administración de suscripciones** para el tipo de problema.

## <a name="service-limitations"></a>Limitaciones del servicio

Algunos servicios requieren otras consideraciones a la hora de mover recursos. Si va a mover los siguientes servicios, no olvide consultar las guías y las limitaciones.

* [App Services](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Modelo de implementación clásica](./move-limitations/classic-model-move-limitations.md)
* [Redes](./move-limitations/networking-move-limitations.md)
* [Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Máquinas virtuales](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>Solicitudes grandes

Cuando sea posible, divida las operaciones de movimiento grandes en varias operaciones de movimiento independientes. Resource Manager inmediatamente devolverá un error si hay más de 800 recursos en una sola operación. No obstante, también se puede producir un error por agotamiento del tiempo de espera al mover menos de 800 recursos.

## <a name="resource-not-in-succeeded-state"></a>El recurso no está en el estado correcto

Si recibe un mensaje de error que indica que no se puede mover un recurso porque no se encuentra en un estado correcto, es posible que en realidad sea un recurso dependiente lo que esté bloqueando el movimiento. Normalmente, el código de error es **MoveCannotProceedWithResourcesNotInSucceededState**.

Si el grupo de recursos de origen o de destino contiene una red virtual, los estados de todos los recursos dependientes de la red virtual se comprueban durante el traslado. La comprobación incluye los recursos directa e indirectamente dependientes de la red virtual. Si alguno de esos recursos se encuentra en un estado de error, el traslado se bloqueará. Por ejemplo, si se produce un error en una máquina virtual que usa la red virtual, el traslado se bloqueará. El traslado se bloqueará incluso cuando la máquina virtual no sea uno de los recursos que se están trasladando y no esté en uno de los grupos de recursos que forma parte del traslado.

Cuando recibe este error, tiene dos opciones: trasladar los recursos a un grupo de recursos que no tenga una red virtual o [ponerse en contacto con el departamento de soporte técnico](../../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Pasos siguientes

Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](move-resource-group-and-subscription.md).
