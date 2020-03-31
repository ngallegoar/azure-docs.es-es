---
title: Ofertas del Marketplace de Azure y AppSource
description: Creación y administración de las ofertas del Marketplace de Azure y AppSource
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: dsindona
ms.openlocfilehash: 7f6fd723355426a49cff032d51da0e09f13e295d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278491"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Ofertas del Marketplace de Azure y AppSource

En la primera parte de esta sección se presentan las operaciones generales que se usan para crear y administrar las ofertas del Marketplace de Azure y AppSource.  En esta parte se proporcionan los antecedentes necesarios para administrar ciertos tipos de ofertas, así la información técnica que es común a todos los tipos de ofertas.  La mayor parte de esta sección contiene instrucciones detalladas sobre cómo crear y administrar tipos de ofertas específicas.  

En el siguiente vídeo se presentan las diversas funcionalidades y diferentes tipos d ofertas disponibles en Azure Marketplace o AppSource.  También explica importantes aspectos técnicos y empresariales para la publicación de aplicaciones o servicios en dichos Marketplace.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Creación de aplicaciones y servicios para Azure Marketplace y AppSource: Build 2018**

Para obtener más información sobre los Marketplace, consulte la [guía de publicación de Azure Marketplace y AppSource](../marketplace-publishers-guide.md).


## <a name="common-offer-operations"></a>Operaciones comunes de las ofertas

El proceso de creación de una nueva oferta difiere en gran medida de los tipos de oferta, por ejemplo entre una [oferta de aplicación de Azure](./azure-applications/cpp-azure-app-offer.md) y una [oferta de servicio de consultoría](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md).  En cambio, muchas de las demás operaciones que realiza en una oferta del [Cloud Partner Portal](https://cloudpartner.azure.com) están bastante estandarizadas en los tipos de oferta.  Estas operaciones comunes, incluidas publicar, ver estado, actualizar y eliminar, se tratan en la sección [Administrar ofertas](./manage-offers/cpp-manage-offers.md)


## <a name="test-drive"></a>Versión de prueba

*Versión de prueba* es una característica de Marketplace que proporciona a los clientes una opción de demostración "probar antes de comprar" para determinadas ofertas.  La capacidad de la versión de prueba se limita al siguiente subconjunto de tipos de oferta: [Aplicaciones de Azure](./azure-applications/cpp-azure-app-offer.md), [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md), [Dynamics 365 for Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md), [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md), [aplicaciones SaaS](./saas-app/cpp-saas-offer.md) y [máquinas virtuales](./virtual-machine/cpp-virtual-machine-offer.md).  Esta funcionalidad requiere que el publicador cree una plantilla Versión de prueba personalizada para su oferta.  Para más información, consulte la sección [Versión de prueba](./test-drive/what-is-test-drive.md).

Puede examinar las ofertas de Marketplace existentes que tienen las demostraciones de la Versión de prueba mediante el uso del [filtro de versión de prueba](https://azuremarketplace.microsoft.com/marketplace/apps?filters=test-drive). 


## <a name="azure-marketplace-and-appsource-offer-types"></a>Tipos de oferta de Azure Marketplace y AppSource

En la tabla siguiente se enumeran los tipos de oferta actuales compatibles con [Cloud Partner Portal](https://cloudpartner.azure.com).  Para cada tipo de oferta, se muestran los Marketplace en los que se puede publicar la oferta, así como una descripción general de la tecnología de solución de la oferta.

|                Tipo de oferta                |  Marketplace  |   Descripción                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Aplicación de Azure](./azure-applications/cpp-azure-app-offer.md) | Azure | Solución compuesta de una o más máquinas virtuales (VM) y código personalizado opcional de Azure. Se implementa a través de una plantilla de Azure Resource Manager.  La implementación puede realizarla el usuario a través de una plantilla de solución o el anunciante puede encargarse de ella. Este tipo de oferta se usa para una obtener una flexibilidad mayor que la de las ofertas de máquina virtual proporcionadas.  |
| [Servicio de consultoría](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Ambos | Los consultores de Microsoft que cumplen los requisitos pueden publicar servicios específicos para un dominio en Azure Marketplace o AppSource.  Su experiencia ayuda a los clientes a evaluar sus problemas y crear e implementar las soluciones correctas para cumplir sus objetivos empresariales.  |
| [Contenedor](./containers/cpp-containers-offer.md)  | Azure | Imagen de contenedor de Docker aprovisionada como un servicio basado en Kubernetes o como una instancia de contenedor de Azure. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Paquete que extiende al sistema de planeación de recursos de empresariales (ERP) y administración empresarial. |
| [Dynamics 365 for Customer Engagement ](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Paquete que extiende este sistema de administración de recursos del usuario (CRM) gracias a los módulos de ventas, servicios, servicio de proyecto y servicio de campo.  |
| [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Paquete que extiende al servicio de planeación de recursos empresariales (ERP) que admite finanzas avanzadas, operaciones, fabricación y administración de la cadena de suministro. |
| [Módulo IoT Edge](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Contenedor compatible con Docker que se ejecuta en un dispositivo IoT Edge.  Está formado por módulos pequeños de cálculo que usan una combinación de código personalizado, otros servicios de Azure y servicios de terceros. |
| [Power BI App](./power-bi/cpp-power-bi-offer.md) | AppSource | Aplicación de Power BI que empaqueta contenido de Power BI personalizable, incluidos conjuntos de datos, informes y paneles. |
| [Aplicación SaaS](./saas-app/cpp-saas-offer.md) | Azure | Suscripción de software como servicio administrada por el anunciante en la que los usuarios inician sesión mediante una interfaz personalizada que usa Azure Active Directory. |
| [Máquina virtual](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Solución contenida dentro de una sola máquina virtual que se implementa en la suscripción del cliente.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Para obtener más información, consulte la [Guía de publicación por tipo de oferta](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Pasos siguientes

Obtendrá información sobre las operaciones generales que puede realizar en las ofertas de Marketplace y sus atributos y recursos técnicos comunes en el artículo [Administración de ofertas](./manage-offers/cpp-manage-offers.md).
