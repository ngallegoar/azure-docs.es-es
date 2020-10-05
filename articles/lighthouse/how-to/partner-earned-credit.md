---
title: Vinculación de un identificador de asociado para habilitar el crédito que ha obtenido un asociado en los recursos delegados
description: Obtenga información acerca de cómo asociar el identificador de asociado para recibir créditos obtenidos por el asociado en los recursos de cliente que se administran a través de Azure Lighthouse.
ms.date: 09/04/2020
ms.topic: how-to
ms.openlocfilehash: 0a9e7f51e90b38bad24eada5a665ca60bf43452f
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89493288"
---
# <a name="link-your-partner-id-to-enable-partner-earned-credit-on-delegated-resources"></a>Vinculación de un identificador de asociado para habilitar el crédito que ha obtenido un asociado en los recursos delegados

Si es miembro de [Microsoft Partner Network](https://partner.microsoft.com/), puede vincular su identificador de asociado con las credenciales utilizadas para administrar los recursos de cliente delegados. De esta forma, podrá realizar un seguimiento del impacto en las interacciones con los clientes y recibir [crédito obtenido por el asociado (PEC) de servicios administrados](/partner-center/partner-earned-credit).

Si [incorpora clientes con ofertas de servicio administrados en Azure Marketplace](publish-managed-services-offers.md), este vínculo se realiza automáticamente, con el identificador de MPN asociado a la cuenta del Centro de partners usada para publicar las ofertas. No es necesario realizar ninguna otra acción para recibir PEC de estos clientes.

Si [incorpora clientes mediante plantillas de Azure Resource Manager](onboard-customer.md), deberá crear este vínculo manualmente. Esto se lleva a cabo [vinculando el identificador de MPN](../../cost-management-billing/manage/link-partner-id.md) con al menos una cuenta de usuario en el inquilino de administración que tiene acceso a cada una de las suscripciones incorporadas.

## <a name="associate-your-partner-id-when-you-onboard-new-customers"></a>Asociación del identificador de asociado al incorporar nuevos clientes

Al incorporar clientes a través de plantillas de Azure Resource Manager, utilice el siguiente proceso para vincular el identificador de asociado y habilitar el crédito obtenido por el asociado. Para completar estos pasos, necesitará conocer su [identificador de asociado de MPN](/partner-center/partner-center-account-setup#locate-your-mpn-id).

Para simplificar, se recomienda crear una cuenta de entidad de servicio en el inquilino, asociarla con el identificador de MPN y, después, concederle acceso a todos los clientes que incorpore con un [rol integrado de Azure que sea apto para el PEC](https://docs.microsoft.com/partner-center/azure-roles-perms-pec).

1. [Cree una cuenta de entidad de servicio](../../active-directory/develop/howto-authenticate-service-principal-powershell.md) en el inquilino de administración. En este ejemplo, asignaremos el nombre "PEC Automation Account" a esta entidad de servicio.
1. Con esa cuenta de entidad de servicio, [vincúlela a su identificador de asociado](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) en el inquilino de administración. Solo tiene que hacer esto una vez.
1. Al [incorporar un cliente mediante plantillas de Azure Resource Manager](onboard-customer.md), asegúrese de incluir una autorización que incluya la cuenta "PEC Automation Account" como usuario con un [rol integrado de Azure que sea apto para el PEC](https://docs.microsoft.com/partner-center/azure-roles-perms-pec).

Siguiendo estos pasos, todos los inquilinos de cliente que administre se asociarán con su identificador de asociado, lo que le permitirá recibir PEC de esos clientes. No es necesario que la cuenta "PEC Automation Account" autentique o realice ninguna acción en el inquilino del cliente.

## <a name="add-your-partner-id-to-previously-onboarded-customers"></a>Adición del identificador de asociado a los clientes incorporados anteriormente

Si ya ha incorporado un cliente, es posible que no desee realizar otra implementación para agregar la entidad de servicio de la cuenta "PEC Automation Account". En su lugar, puede asociar el identificador de MPN a una cuenta de usuario que ya tenga acceso para trabajar en el inquilino de ese cliente. Asegúrese de que se ha concedido a la cuenta un [rol integrado de Azure que sea apto para el PEC](https://docs.microsoft.com/partner-center/azure-roles-perms-pec).

Una vez que la cuenta se ha [vinculado a su identificador de asociado](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) en el inquilino de administración, podrá recibir PEC de ese cliente.

## <a name="confirm-partner-earned-credit"></a>Confirmación de crédito obtenido por el asociado

Puede [ver los detalles del PEC en Azure Portal](/partner-center/partner-earned-credit-explanation#azure-cost-management) y confirmar qué costos han recibido la ventaja del PEC.

Si ha seguido los pasos anteriores y no ve la asociación, abra una solicitud de soporte técnico en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre el programa [Microsoft Partner Network](/partner-center/mpn-overview).
- Aprenda [cómo se calcula y se paga el PEC](/partner-center/partner-earned-credit-explanation).
- [Incorpore clientes](onboard-customer.md) a Azure Lighthouse.
