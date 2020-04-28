---
title: Guía de publicación de ofertas de aplicaciones administradas de Azure | Azure Marketplace
description: En este artículo se describen los requisitos para publicar una aplicación administrada en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 946e7524eada600d5ef17b2663a3fea066dcfaa2
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084879"
---
# <a name="azure-applications-managed-application-offer-publishing-requirements"></a>Aplicaciones de Azure: Requisitos de publicación de ofertas de aplicaciones administradas

En este artículo se explican los requisitos para el tipo de oferta de aplicación administrada, que es una manera de publicar una oferta de aplicación de Azure en Azure Marketplace. Las aplicaciones administradas son ofertas de transacciones que se implementan y facturan a través de Azure Marketplace. La llamada a la acción que el usuario ve es "Obtener ahora".

Use el tipo de oferta de aplicación administrada en las siguientes circunstancias:

- Va a implementar una solución basada en una suscripción para el cliente mediante una máquina virtual o una solución completa basada en IaaS.
- Usted o su cliente requieren que la solución la administre un asociado.

>[!NOTE]
>Por ejemplo, un asociado puede ser un SI o un proveedor de servicios administrados (MSP).  

## <a name="managed-application-offer"></a>Oferta de aplicación administrada

|Requisitos |Detalles  |
|---------|---------|
|Implementado en una suscripción de Azure de cliente | Las aplicaciones administradas se deben implementar en la suscripción del cliente y se pueden administrar mediante un tercero. |
|Facturación y medición    |  Los recursos se aprovisionarán en la suscripción a Azure del cliente. Las transacciones de máquinas virtuales de pago por uso (PAYGO) se realizarán con el cliente mediante Microsoft y se facturarán a través de la suscripción a Azure del cliente (PAYGO). <br> En el caso del modelo denominado traiga su propia licencia, Microsoft facturará los costos de infraestructura derivados de la suscripción del cliente, mientras que usted realizará la transacción de los honorarios de licencia de software directamente con el cliente.        |
|Disco duro virtual (VHD) compatible con Azure    |   Las máquinas virtuales deben estar basadas en Windows o Linux.<ul> <ul> <li>Para obtener más información acerca de cómo crear un VHD de Linux, consulte [Distribuciones de Linux aprobadas en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Para más información sobre la creación de un disco duro virtual de Windows, consulte [Creación de una oferta de aplicación de Azure](./partner-center-portal/create-new-azure-apps-offer.md).</li> </ul> |

>[!NOTE]
> Las aplicaciones administradas se deben poder implementar mediante Marketplace. Si la comunicación con el cliente es una preocupación, debe ponerse en contacto con los clientes interesados después de habilitar el uso compartido de clientes potenciales.  

>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible. Para más información sobre cómo comercializar su oferta a través de los canales de asociados de CSP de Microsoft, consulte [Proveedores de soluciones en la nube](./cloud-solution-providers.md).

## <a name="next-steps"></a>Pasos siguientes

- Si aún no lo ha hecho, [aprenda](https://azuremarketplace.microsoft.com/sell) sobre Azure Marketplace.
- [Inicie sesión en el Centro de Partners](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) para crear o completar la oferta.
- Para más información, consulte [Creación de una oferta de aplicaciones de Azure](./partner-center-portal/create-new-azure-apps-offer.md).
