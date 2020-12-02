---
title: Guía de publicación de ofertas de aplicaciones administradas de Azure - Azure Marketplace
description: En este artículo se describen los requisitos para publicar una aplicación administrada en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: msjogarrig
ms.author: jogarrig
ms.date: 09/04/2020
ms.openlocfilehash: d4fb3354b7035149b80191528b2f5335b593b764
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96433548"
---
# <a name="publishing-guide-for-azure-managed-applications"></a>Guía de publicación de aplicaciones administradas de Azure

Una oferta de *aplicación administrada* de Azure es una manera de publicar una aplicación de Azure en Azure Marketplace. Las aplicaciones administradas son ofertas de transacciones que se implementan y facturan mediante Azure Marketplace. La opción de publicación que el usuario ve es *Obtener ahora*.

En este artículo se explican los requisitos para el tipo de oferta de aplicación administrada.

Use el tipo de oferta de aplicación administrada con las siguientes condiciones:

- Está implementando una solución basada en suscripción para su cliente mediante una máquina virtual (VM) o una solución completa basada en la infraestructura como servicio (IaaS).
- Usted o su cliente requieren que un asociado administre la solución.

>[!NOTE]
>Por ejemplo, un asociado puede ser un integrador de sistemas o un proveedor de servicios administrados (MSP).  

## <a name="managed-application-offer-requirements"></a>Requisitos de las ofertas de aplicaciones administradas

|Requisitos |Detalles  |
|---------|---------|
|Una suscripción de Azure | Las aplicaciones administradas se deben implementar en la suscripción del cliente, pero las puede administrar un tercero. |
|Facturación y medición    |  Los recursos se proporcionan en la suscripción de Azure de un cliente. Las transacciones de VM que usan el modelo de pago por uso se realizan con el cliente mediante Microsoft y se facturan con la suscripción de Azure del cliente. <br><br> En el caso de las VM con el modelo "traiga su propia licencia", Microsoft factura los costos de infraestructura derivados en la suscripción del cliente, mientras que usted realizará la transacción de las tarifas de licencia de software directamente con el cliente.        |
|Un disco duro virtual (VHD) compatible con Azure    |   Las máquinas virtuales deben estar basadas en Windows o Linux.<br><br>Para obtener más información acerca de cómo crear un VHD de Linux, consulte [Distribuciones de Linux aprobadas en Azure](../virtual-machines/linux/endorsed-distros.md).<br><br>Para más información sobre la creación de un disco duro virtual de Windows, consulte [Creación de una oferta de aplicación de Azure](./create-new-azure-apps-offer.md). |

---

> [!NOTE]
> Las aplicaciones administradas se deben poder implementar desde Azure Marketplace. Si la comunicación con el cliente es una preocupación, póngase en contacto con los clientes interesados después de habilitar el uso compartido de clientes potenciales.  

> [!Note]
> La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible. Para más información sobre cómo comercializar su oferta a través de los canales de asociados de CSP de Microsoft, consulte [Proveedores de soluciones en la nube](./cloud-solution-providers.md).

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, aprenda a [Ampliar su negocio en la nube con Azure Marketplace](https://azuremarketplace.microsoft.com/sell).

Para registrarse y empezar a trabajar en el Centro de partners, haga lo siguiente:

- [Inicie sesión en el Centro de partners](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) para crear o completar la oferta.
- Para obtener más información, consulte [Creación de una oferta de aplicaciones de Azure](./create-new-azure-apps-offer.md).
