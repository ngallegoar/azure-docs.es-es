---
title: Oferta de máquina virtual en Azure Marketplace
description: Información general del proceso de publicación de una oferta de máquina virtual en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: dsindona
ms.openlocfilehash: b857cf8b7485beb884adf77d99f82d965f55a0ad
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142798"
---
# <a name="virtual-machine-offer"></a>Oferta de máquina virtual

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) para administrar las ofertas migradas.

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| En esta sección se explica cómo publicar una nueva oferta de máquina virtual en [Azure Marketplace](https://azuremarketplace.microsoft.com). Se proporciona soporte técnico para máquinas virtuales basadas tanto en Windows como en Linux, que contienen un disco de duro virtual (VHD) de sistema operativo y cero o más VHD de datos. | ![icono de máquina virtual](./media/virtual-machine-icon.png)  |


## <a name="publishing-overview"></a>Introducción a la publicación

En el siguiente vídeo, [Optimize Your Azure Marketplace Offer](https://channel9.msdn.com/Events/Build/2017/P4026?ocid=player) (Optimizar su oferta de Azure Marketplace), se presenta una visión general amplia de Azure Marketplace, incluida la forma de publicar en Marketplace (mediante una solución de máquina virtual), cómo optimizar la experiencia del usuario con la página de productos y la experiencia de versión de prueba opcional, cómo se generan usuarios potenciales y cómo puede usarlos, y cómo optimizar la involucración de los clientes.

> [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4026/player]


## <a name="vm-publishing-process-flow"></a>Flujo de proceso de publicación de máquina virtual

En el diagrama siguiente se ilustran los pasos generales de la publicación de una oferta de máquina virtual. 

![Proceso de publicación de máquina virtual](./media/publishvm_001.png)

1. Creación de la oferta: se configuran todos los detalles y la información sobre la oferta, incluida la descripción de la oferta, los materiales de marketing, la información legal y de soporte técnico, y las especificaciones de los recursos.

2. Creación de los recursos técnicos y empresariales: se crean los recursos empresariales (documentos legales y materiales de marketing) y los recursos técnicos de la solución asociada (en este caso, las máquinas virtuales y los discos conectados). 

3. Creación de la SKU: se crean las SKU asociadas con la oferta y se envían.  Se necesita una SKU única para cada imagen que se vaya a publicar. 
 
4. Certificación y publicación de la oferta: después de completar la oferta y los recursos técnicos, se puede enviar. Este envío iniciará el proceso de publicación, en el que la solución se evalúa, se valida, se certifica y después se "publica" en Marketplace.  

## <a name="next-steps"></a>Pasos siguientes

Antes de considerar estos pasos, debe cumplir los [requisitos técnicos y empresariales](./cpp-prerequisites.md) para publicar una máquina virtual en Microsoft Azure Marketplace. 
