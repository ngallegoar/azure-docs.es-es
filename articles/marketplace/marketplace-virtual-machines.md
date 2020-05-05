---
title: Guía de publicación de ofertas de máquinas virtuales para Azure Marketplace
description: En este artículo se describen los requisitos para publicar una máquina virtual y una evaluación gratuita de software para su implementación desde Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: 2fa67d81546db86535c179a9c59d0602c1175cba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687496"
---
# <a name="virtual-machine-offer-publishing-guide"></a>Guía de publicación de ofertas de máquinas virtuales

Las imágenes de máquina virtual son uno de los principales mecanismos para publicar una solución en Azure Marketplace. Use esta guía para comprender los requisitos para esta oferta. 

Estas ofertas de transacción se implementan y facturan a través de Marketplace. La llamada a la acción que el usuario ve es "Obtener ahora".

## <a name="free-trial"></a>Versión de prueba gratuita 

Puede disponer que los usuarios prueben la oferta mediante el acceso a licencias de software de tiempo limitado si se usa el modelo de facturación traiga su propia licencia (BYOL). 

## <a name="test-drive"></a>Versión de prueba

Se implementan una o varias máquinas virtuales a través de aplicaciones de infraestructura como servicio (IaaS) o software como servicio (SaaS). Una ventaja de la opción de publicación de versión de prueba es el aprovisionamiento automatizado de una máquina virtual o una solución completa dirigida por una visita guiada hospedada por un asociado. Una versión limitada de prueba proporciona una prueba sin ningún costo adicional para el cliente. El cliente no tiene que ser un cliente existente de Azure para poder participar en la experiencia de prueba. 

Póngase en contacto con nosotros en [amp-testdrive](mailto:amp-testdrive@microsoft.com) para empezar a trabajar. 

|Requisitos  |Detalles |
|---------|---------|
| Tiene una aplicación de Marketplace   |    Una o varias máquinas virtuales mediante IaaS o SaaS.      |

## <a name="interactive-demo"></a>Demostración interactiva

Proporciona a los clientes una experiencia guiada de su solución mediante una demostración interactiva. La ventaja de la opción de publicación de demostración interactiva es que proporciona una experiencia de prueba sin aprovisionamiento complicado de soluciones complejas. 

## <a name="virtual-machine-offer"></a>Oferta de máquina virtual

Use el tipo de oferta de máquina virtual cuando implemente un dispositivo virtual para la suscripción asociada con el cliente. Las VM se pueden comercializar íntegramente con los modelos de licencia de pago por uso o BYOL (traiga su propia licencia). Microsoft hospeda la transacción comercial y factura al cliente en su nombre. Usted obtiene la ventaja de usar la relación de pago preferida entre el cliente y Microsoft, incluidos los contratos empresariales existentes.

> [!NOTE]
> En este momento, los compromisos monetarios asociados a un contrato empresarial se pueden usar contra el uso que Azure hace de su máquina virtual, pero no contra sus tarifas de licencias de software.  
> 
> [!NOTE]
> Puede restringir la detección y la implementación de su VM para un conjunto de clientes específico. Para ello, publique la imagen y los precios como una oferta privada. Las ofertas privadas le ofrecen la posibilidad de crear ofertas exclusivas para sus clientes más próximos y proporcionan software y términos personalizados. Los términos personalizados le permiten resaltar una gran variedad de escenarios, incluidas las ofertas sobre el terreno con precios y términos especializados, así como acceso anticipado a software de versión limitada. Las ofertas privadas permiten ofrecer precios o productos específicos a un conjunto limitado de clientes mediante la creación de una nueva SKU con esos detalles.  
> *   Para obtener más información sobre las ofertas privadas, visite la página sobre ofertas privadas en Azure Marketplace que se encuentra en [azure.microsoft.com/blog/private-offers-on-azure-marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Requisito | Detalles |  
|:--- |:--- | 
| Facturación y medición | Su máquina virtual debe admitir las opciones de facturación mensual BYOL o de pago por uso. |  
| Disco duro virtual (VHD) compatible con Azure | Las máquinas virtuales deben estar basadas en Windows o Linux. <ul> <li>Para obtener más información acerca de cómo crear un VHD de Linux, consulte [Distribuciones de Linux aprobadas en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Para más información acerca de la creación de un VHD de Windows, consulte [Creación de un disco duro virtual compatible con Azure](./partner-center-portal/azure-vm-create-offer.md).</li> </ul> |  

>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](./cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, 

- [Obtenga información](https://azuremarketplace.microsoft.com/sell) sobre Marketplace.

Si está registrado y está creando una oferta nueva o trabajando en una existente,

- [Inicie sesión en el Centro de partners](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) para crear o completar la oferta.
- Para más información, consulte [Creación de una oferta de máquina virtual](./partner-center-portal/azure-vm-create-offer.md).
