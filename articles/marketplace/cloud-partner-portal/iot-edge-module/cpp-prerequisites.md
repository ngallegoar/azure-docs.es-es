---
title: Requisitos previos de los módulos Azure IoT Edge | Azure Marketplace
description: Requisitos previos para publicar un módulo IoT Edge.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: d1bfc29a0cb031421b4017454bc8a2cdd8be09f9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142368"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Requisitos previos de publicación de módulos IoT Edge

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de módulos de IoT Edge al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de módulo de IoT Edge](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation) para administrar las ofertas migradas.

En este artículo se describen los requisitos previos para publicar una oferta de módulo IoT Edge.  Si aún no lo ha hecho, revise el [Guía de publicación de módulos de IoT Edge](../..//iot-edge-module.md).


## <a name="publishing-prerequisites"></a>Requisitos previos de publicación

Para publicar un módulo IoT Edge en Azure Marketplace, es preciso cumplir los siguientes requisitos previos:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Acceso a [Cloud Partner Portal](https://cloudpartner.azure.com/). Para obtener más información, vea [Guía de publicación de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Aceptación de los [Términos de Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Hospedaje de los recursos técnicos del módulo IoT Edge en Azure Container Registry.  Para obtener más información, vea [Preparar los recursos técnicos del módulo IoT Edge](./cpp-create-technical-assets.md)
- Metadatos del módulo IoT Edge listos para usar. Por ejemplo, prepare los recursos siguientes:
    - Un título
    - Una descripción (en formato HTML)
    - Una imagen de logotipo (formato PNG y tamaños de imagen fijos, como 40 x 40 px, 90 x 90 px, 115 x 115 px, 255 x 115 px)
    - Un término de uso y directiva de privacidad
    - Una configuración predeterminada del módulo que incluya: rutas, propiedades deseadas gemelas, createOptions y variables de entorno.
    - Documentación del módulo
    - Contactos de soporte técnico


## <a name="next-steps"></a>Pasos siguientes

Una vez que tenga [preparado el recurso técnico del módulo IoT Edge](./cpp-create-technical-assets.md), estará listo para [crear su oferta de módulo IoT Edge](./cpp-create-offer.md). 
