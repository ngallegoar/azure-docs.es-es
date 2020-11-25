---
title: Preguntas frecuentes sobre Azure Peering Service
description: Conozca las preguntas frecuentes sobre Microsoft Azure Peering Service.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 95ce90dbbf47ffe527fe6f25704d9cd28b834ea9
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026702"
---
# <a name="peering-service-faq"></a>Preguntas frecuentes sobre Peering Service

En este artículo se describen las preguntas frecuentes sobre las conexiones de Azure Peering Service.


**P. ¿Quiénes son los clientes de destino?**

A. Los clientes de destino son empresas que se conectan a Microsoft Cloud mediante el uso de Internet como transporte.

**P. ¿Los clientes pueden suscribirse a Peering Service con varios proveedores?** 

A. Sí, los clientes pueden suscribirse a Peering Service con varios proveedores de la misma u otra región, pero no con el mismo prefijo.

**P. ¿Pueden seleccionar los clientes un ISP único para sus sitios por región geográfica?**

A. Sí, los clientes pueden hacerlo. Seleccione el ISP del asociado por región que se adapte a sus necesidades empresariales y operativas.

**P. ¿Qué es un PoP de Microsoft Edge?**

A. Es una ubicación física en la que Microsoft se interconecta con otras redes. En la ubicación PoP de Microsoft Edge, se hospedan servicios como Azure Front Door y Azure CDN. Para más información, consulte [Azure CDN](../cdn/cdn-features.md).

## <a name="peering-service-unique-characteristics"></a>Peering Service: Características singulares

**P. ¿En qué se diferencia Peering Service del acceso normal a Internet?**

A. Los asociados que se han registrado en Microsoft Peering Service trabajan con Microsoft para ofrecer un enrutamiento optimizado y una conectividad confiable a los servicios de Microsoft.  

**P. ¿En qué se diferencia Peering Service de ExpressRoute?**

A. Azure ExpressRoute es una conexión privada dedicada desde una o varias ubicaciones de clientes. Si bien Peering Service ofrece conectividad pública optimizada y no admite conectividad privada, también ofrece conectividad optimizada para los grupos de Internet locales.

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre Peering Service, consulte [Introducción a Peering Service](about.md).
- Para buscar un proveedor de servicios, consulte [Asociados y ubicaciones de Peering Service](location-partners.md).
- Para incorporar una conexión de Peering Service, consulte [Incorporación de Peering Service](onboarding-model.md).
- Para registrar una conexión de Peering Service, consulte [Registro de una conexión de Peering Service: Azure Portal](azure-portal.md).
- Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).