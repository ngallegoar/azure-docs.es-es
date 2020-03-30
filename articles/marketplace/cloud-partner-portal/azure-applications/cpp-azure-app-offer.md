---
title: Oferta de aplicación de Azure | Azure Marketplace
description: Descripción general del proceso de publicación de una oferta de aplicación de Azure en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: dsindona
ms.openlocfilehash: ed086ffdc49e21b819c0ee05b38ad882b4e269d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285323"
---
# <a name="azure-application-offer"></a>Oferta de aplicación de Azure

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| <div class="body"> En esta sección se explica cómo publicar una nueva oferta de aplicación de Azure en [Azure Marketplace](https://azuremarketplace.microsoft.com).  Cada aplicación de Azure contiene una plantilla de Azure Resource Manager que define todos los recursos técnicos utilizados por la aplicación, que normalmente incluye una o más máquinas virtuales y otros servicios de soporte técnico basados en web o en Azure. Todas las ofertas de aplicaciones de Azure deben habilitar la seguridad de acceso a través de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).  </div> | ![Icono de aplicaciones de Azure](./media/azureapp-icon1.png)  |

## <a name="publishing-overview"></a>Introducción a la publicación

El siguiente vídeo, [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Compilación de plantillas de soluciones y aplicaciones administradas para Azure Marketplace), es una introducción a qué tipos de ofertas hay disponibles, a cuáles son los recursos técnicos necesarios, a cómo crear una plantilla de Azure Resource Manager, al desarrollo y la prueba de la IU de la aplicación, a cómo publicar la oferta de la aplicación y al proceso de revisión de la aplicación.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


## <a name="types-of-azure-applications"></a>Tipos de aplicaciones de Azure

Hay dos tipos de aplicaciones de Azure: una aplicación administrada y una plantilla de solución. 

- Las plantillas de solución son uno de los principales mecanismos para publicar una solución en Marketplace. Este tipo de oferta se usa cuando la solución requiere automatización adicional de implementación y configuración para algo más que una sola máquina virtual (VM). Puede automatizar proporcionando más de una VM mediante una plantilla de solución. Esta automatización incluye el aprovisionamiento de recursos de red y de almacenamiento para proporcionar soluciones complejas de IaaS. Para obtener información general de los requisitos de una plantilla de solución y el modelo de facturación, consulte [Aplicaciones de Azure: guía de publicación de ofertas de plantillas de solución](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

- Las aplicaciones administradas son similares a las plantillas de soluciones, aunque hay una diferencia importante. En una aplicación administrada, los recursos se implementan en un grupo de recursos que está administrado por el editor de la aplicación. El grupo de recursos está presente en la suscripción del consumidor, pero una identidad en el inquilino del editor tiene acceso al grupo de recursos. Como editor, puede especificar el costo del soporte técnico continuado de la solución. Use aplicaciones administradas de Azure para crear y entregar con facilidad a los clientes aplicaciones llave en mano totalmente administradas.

Además de Azure Marketplace, también puede ofrecer aplicaciones administradas en un catálogo de servicios. El catálogo de servicios es un catálogo interno de soluciones aprobadas para los usuarios de una organización. Use el catálogo para cumplir los estándares organizativos y ofrecer al mismo tiempo soluciones para los grupos en una organización. Los empleados usarán el catálogo para encontrar fácilmente las aplicaciones que el departamento de TI recomienda y aprueba.

>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](../../cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft.

Para más información sobre las ventajas y los tipos de aplicaciones administradas, consulte [Introducción a las aplicaciones administradas de Azure](https://docs.microsoft.com/azure/managed-applications/overview).


## <a name="publishing-process-workflow"></a>Flujo de trabajo del proceso de publicación

El siguiente diagrama muestra el proceso general para publicar una oferta de aplicación de Azure.

![Flujo de trabajo para publicar una oferta](./media/new-offer-process.png)

Los pasos generales para publicar una oferta de aplicación de Azure son:

1. Cumplir los [Requisitos previos](./cpp-prerequisites.md): (no se muestra) compruebe que se han cumplido los requisitos empresariales y técnicos para publicar una aplicación de Azure en Azure Marketplace. 

1. [Crear la oferta](./cpp-create-offer.md): proporcione información detallada sobre la oferta. Esta información incluye: la descripción de la oferta, materiales de marketing, información de soporte técnico y especificaciones de recursos.

1. [Creación o compilación de los recursos técnicos y empresariales](./cpp-create-technical-assets.md): se crean los recursos empresariales (documentos legales y materiales de marketing) y los recursos técnicos de la solución asociada.

1. [Crear las SKU](./cpp-skus-tab.md): cree las SKU asociadas a la oferta. Se necesita una SKU única para cada imagen que se vaya a publicar.

1. Certificar y [publicar la oferta](./cpp-publish-offer.md): después de completar la oferta y los recursos técnicos, puede enviar la oferta. Este envío inicia el proceso de publicación. Durante este proceso, la solución se prueba, se valida, se certifica y luego se publica en Azure Marketplace.

## <a name="next-steps"></a>Pasos siguientes

Antes de considerar estos pasos, debe cumplir los [requisitos técnicos y empresariales](./cpp-prerequisites.md) para publicar una aplicación administrada en Microsoft Azure Marketplace.
