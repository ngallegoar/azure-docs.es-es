---
title: ¿Qué es Azure Lighthouse?
description: Azure Lighthouse permite a los proveedores de servicios ofrecer servicios administrados para sus clientes con mayor automatización y eficacia a escala.
ms.date: 05/05/2020
ms.topic: overview
ms.openlocfilehash: bc54a6f67ed3d0ac194793baba4270d524edf5b1
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857132"
---
# <a name="what-is-azure-lighthouse"></a>¿Qué es Azure Lighthouse?

Azure Lighthouse ofrece a los proveedores de servicios un plano de control único para ver y administrar Azure a través de todos sus clientes con una mayor automatización, escalado y gobernanza mejorada. Con Azure Lighthouse, los proveedores de servicios pueden ofrecer servicios administrados mediante herramientas de administración completas y sólidas integradas en la plataforma Azure. Esta oferta también puede ayudar a las organizaciones de TI empresariales a administrar recursos en varios inquilinos. 

![Diagrama de información general de Azure Lighthouse](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Ventajas

Azure Lighthouse le ayuda a compilar y ofrecer servicios administrados de forma rentable y eficaz para sus clientes. Entre las ventajas se incluye lo siguiente:

- **Administración a escala**: la involucración del cliente y las operaciones de ciclo de vida para administrar los recursos del cliente son más fáciles y escalables. Las API, las herramientas de administración y los flujos de trabajo existentes se pueden usar con los recursos de cliente delegados.
- **Mayor visibilidad y precisión para los clientes**: los clientes tendrán mayor visibilidad de sus acciones y un control preciso sobre el ámbito que delegan para la administración, incluida la capacidad de quitar el acceso por completo, mientras que la dirección IP se conserva.
- **Herramientas de plataforma completas y unificadas**: nuestra experiencia de herramientas aborda los escenarios clave del proveedor de servicios, incluidos varios modelos de licencia como EA, CSP y pago por uso. Las nuevas funcionalidades funcionan con herramientas y API, modelos de licencias y programas de asociados existentes, como el [Programa Cloud Solution Provider](https://docs.microsoft.com/partner-center/csp-overview). Azure Lighthouse se puede integrar en sus flujos de trabajo y aplicaciones existentes, y puede realizar un seguimiento de su impacto sobre la involucración del cliente [vinculando su identificador de asociado](../billing/billing-partner-admin-link-started.md).

No hay costos adicionales asociados con el uso de Azure Lighthouse para administrar los recursos de Azure de los clientes. Cualquier cliente o asociado de Azure puede usar Azure Lighthouse.

## <a name="capabilities"></a>Capacidades

Azure Lighthouse incluye varias maneras de ayudar a simplificar la administración y la involucración de los clientes:

- **Administración de recursos delegados de Azure**: Administre los recursos de Azure de sus clientes de forma segura desde su propio inquilino sin tener que cambiar los planos de contexto y control. Las suscripciones y los grupos de recursos se pueden delegar a usuarios y roles especificados en el inquilino de administración, con la capacidad de quitar el acceso según sea necesario. Para obtener más información, consulte la [administración de recursos delegados de Azure](concepts/azure-delegated-resource-management.md).
- **Nuevas experiencias de Azure Portal**: vea la información de distintos inquilinos en la nueva página **Mis clientes** de [Azure Portal](https://portal.azure.com). Una hoja de **proveedores de servicios** correspondiente permite a los clientes ver y administrar el acceso del proveedor de servicios. Para obtener más información, consulte [Ver y administrar clientes](./how-to/view-manage-customers.md) y [View and manage service providers](how-to/view-manage-service-providers.md) (Ver y administrar proveedores de servicios).
- **Plantillas de Azure Resource Manager**: realice tareas de administración más fácilmente, incluida la incorporación de clientes para la administración de recursos delegados de Azure. Para obtener más información, consulte nuestro [repositorio de ejemplos](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates) y [Onboard a customer to Azure delegated resource management](how-to/onboard-customer.md) (Incorporar a un cliente en la administración de recursos delegados de Azure).
- **Ofertas de servicio administrado en Azure Marketplace**: ofrezca sus servicios a los clientes mediante ofertas privadas o públicas, y pídales que se incorporen automáticamente a la administración de recursos delegados de Azure, como alternativa a la incorporación mediante plantillas de Azure Resource Manager. Para más información, consulte [Ofertas de servicio administrado en Azure Marketplace](concepts/managed-services-offers.md).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [administración de recursos delegados de Azure](concepts/azure-delegated-resource-management.md).
- Más información sobre las [experiencias de administración entre inquilinos](concepts/cross-tenant-management-experience.md).
