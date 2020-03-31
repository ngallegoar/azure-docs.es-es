---
title: Guía de publicación de ofertas de aplicaciones administradas para las aplicaciones de Azure
description: En este artículo se describen los requisitos para publicar una aplicación administrada en Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: dsindona
ms.openlocfilehash: 38323ecfee69460b16542a3e004513f6a7389c41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288417"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Aplicaciones de Azure: guía de publicación de ofertas de aplicaciones administradas

Una aplicación administrada es uno de los principales mecanismos para publicar una solución en Marketplace. Use esta guía para comprender los requisitos para esta oferta. 

Estas ofertas de transacción se implementan y facturan a través de Marketplace. La llamada a la acción que el usuario ve es "Obtener ahora".

Use el tipo de oferta Aplicación de Azure: aplicación administrada cuando se requieran las condiciones siguientes:
- Se implementa una solución basada en suscripciones para al cliente con una máquina virtual o una solución completa basada en IaaS.
- Usted o su cliente requieren que la solución la administre un asociado.

>[!NOTE]
>Por ejemplo, un asociado puede ser un SI o un proveedor de servicios administrados (MSP).  

## <a name="managed-application-offer"></a>Oferta de aplicación administrada

|Requisitos |Detalles  |
|---------|---------|
|Implementado en una suscripción de Azure de cliente | Las aplicaciones administradas se deben implementar en la suscripción del cliente y se pueden administrar mediante un tercero. | 
|Facturación y medición    |  Los recursos se aprovisionarán en la suscripción a Azure del cliente. Las transacciones de máquinas virtuales de pago por uso (PAYGO) se realizarán con el cliente mediante Microsoft y se facturarán a través de la suscripción a Azure del cliente (PAYGO). <br> En el caso del modelo denominado traiga su propia licencia, Microsoft facturará los costos de infraestructura derivados de la suscripción del cliente, mientras que usted realizará la transacción de los honorarios de licencia de software directamente con el cliente.        |
|Disco duro virtual (VHD) compatible con Azure    |   Las máquinas virtuales deben estar basadas en Windows o Linux.<ul> <ul> <li>Para obtener más información acerca de cómo crear un VHD de Linux, consulte [Distribuciones de Linux aprobadas en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Para más información acerca de la creación de un VHD de Windows, consulte [Creación de un disco duro virtual compatible con Azure](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |

>[!NOTE]
> Las aplicaciones administradas se deben poder implementar mediante Marketplace. Si la comunicación con el cliente es una preocupación, debe ponerse en contacto con los clientes interesados después de habilitar el uso compartido de clientes potenciales.  

>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](./cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft.

## <a name="next-steps"></a>Pasos siguientes
Si aún no lo ha hecho, 

- [Regístrese](https://azuremarketplace.microsoft.com/sell) en Marketplace.

Si está registrado y está creando una oferta nueva o trabajando en una existente,

- [Inicie sesión en Cloud Partner Portal](https://cloudpartner.azure.com) para crear o completar su oferta.
