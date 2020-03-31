---
title: Requisitos previos de ofertas de Aplicación de Azure | Azure Marketplace
description: En este artículo se describen los requisitos previos para la publicación de una oferta de aplicación de Azure en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: dsindona
ms.openlocfilehash: b927404758dc59419fb3c5e638ac32758d534681
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281755"
---
# <a name="azure-application-prerequisites"></a>Requisitos previos de la aplicación de Azure

En este artículo se describen los requisitos previos técnicos y empresariales para la publicación de una oferta de aplicación administrada en Azure Marketplace.  Si aún no lo ha hecho, revise las siguientes fuentes de información:
- Dependiendo del tipo de SKU, ya sea [Aplicaciones de Azure: guía de publicación de ofertas de plantillas de solución](../../marketplace-solution-templates.md) o [Aplicaciones de Azure: guía de publicación de ofertas de aplicaciones administradas](../../marketplace-managed-apps.md)
- Vídeo [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Creación de plantillas de soluciones y aplicaciones administradas para Azure Marketplace)


## <a name="technical-requirements"></a>Requisitos técnicos

Entre los requisitos técnicos se incluyen los siguientes:

*   Plantillas de Azure Resource Manager Para más información, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). En este artículo se describe la estructura de una plantilla de Azure Resource Manager. Presenta las distintas secciones de una plantilla y las propiedades que están disponibles en esas secciones. La plantilla consta de JSON y expresiones que puede usar para generar valores para su implementación. 
* Plantillas de inicio rápido de Azure.<br> Para más información, consulte:

  * [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/). Implemente recursos de Azure mediante Azure Resource Manager con plantillas proporcionadas por la comunidad para aumentar su productividad. El Administrador de recursos de Azure le permite aprovisionar sus aplicaciones mediante una plantilla declarativa. En una plantilla, puede implementar varios servicios junto con sus dependencias. Use la misma plantilla para implementar su aplicación de forma repetida durante cada fase de su ciclo de vida.
  * [GitHub: Plantillas de inicio rápido de Azure Resource Manager](https://github.com/azure/azure-quickstart-templates). Este repositorio contiene todas las plantillas de Azure Resource Manager disponibles actualmente aportadas por la comunidad. Un índice de plantillas que admite búsquedas se mantiene en https://azure.microsoft.com/documentation/templates/.
* Crear definición de UI<br>
Para más información, consulte [Creación de la interfaz de usuario de Azure Portal para una aplicación administrada](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview). En este artículo se presentan los conceptos principales del archivo createUiDefinition.json. Azure Portal usa este archivo para generar la interfaz de usuario para crear una aplicación administrada.


## <a name="business-requirements"></a>Requisitos empresariales

Los requisitos empresariales incluyen las siguientes obligaciones contractuales, de procedimientos y legales:

* Debe ser publicador de Cloud Marketplace registrado. Si no está registrado, siga los pasos del artículo [Conviértase en anunciante de Cloud Marketplace](https://docs.microsoft.com/azure/marketplace/become-publisher
).

>[!NOTE]
>Debe usar la misma cuenta de registro de Microsoft Developer Center para iniciar sesión en Cloud Partner Portal. Solo debe tener una cuenta Microsoft para sus ofertas de Azure Marketplace. Esta cuenta no debería ser específica de servicios ni ofertas individuales.

* La empresa (o sus subsidiarias) debe estar en uno de los países/regiones de origen de venta admitidos por Azure Marketplace. Para obtener una lista actual de estos países y regiones, consulte [Directivas de Participación de Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
* El producto debe contar con una licencia que sea compatible con los modelos de facturación admitidos por Azure Marketplace. Para obtener más información, vea las [opciones de facturación](https://docs.microsoft.com/azure/marketplace/marketplace-commercial-transaction-capabilities-and-considerations) en Azure Marketplace.
* Usted es el responsable de poner el soporte técnico a disposición de los clientes de forma comercialmente razonable. Puede ser gratuito, de pago o a través de métodos de la comunidad.
* Asimismo, es responsable de la concesión de licencias para su software y las dependencias de software de terceros.
* Debe proporcionar contenido que cumpla los criterios para que la oferta se publique en Azure Marketplace y en Azure Portal.
* Debe aceptar los términos de las Directivas de Participación de Microsoft Azure Marketplace y del Acuerdo del publicador.
* También debe cumplir los Términos de Uso del Sitio Web de Microsoft Azure, la Declaración de privacidad de Microsoft y el Contrato del Programa Microsoft Azure Certified.


## <a name="publishing-requirements"></a>Requisitos de publicación

Para publicar una nueva oferta de aplicación de Azure, debe cumplir los siguientes requisitos previos:

* Tener los metadatos listos para su uso. En la lista siguiente (que no es exhaustiva) se muestra un ejemplo de estos metadatos:
  * Un título
  * Una descripción (en formato HTML)
  * Una imagen de logotipo (en formato PNG) y en estos tamaños de imagen fijos: 40 x 40 píxeles, 90 x 90 píxeles, 115 x 115 píxeles y 255 x 115 píxeles.
* Documentos de *Términos de uso* y *Directiva de privacidad*
* Documentación de Aplicación
* Contactos de soporte técnico


## <a name="next-steps"></a>Pasos siguientes

Una vez cumplidos todos los requisitos, estará todo listo para [crear una oferta de aplicación de Azure](./cpp-create-offer.md). 
 
