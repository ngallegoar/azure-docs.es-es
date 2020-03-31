---
title: Ofertas de servicios administrados en Azure Marketplace
description: Los servicios administrados permiten a los proveedores de servicios vender ofertas de administración de recursos a los clientes de Azure Marketplace.
ms.date: 03/17/2020
ms.topic: conceptual
ms.openlocfilehash: 2d6e39f753736c0582e9d91870a99b66ae41255b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500789"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Ofertas de servicios administrados en Azure Marketplace

En este artículo se describe el nuevo tipo de oferta de **Servicios administrados** de [Azure Marketplace](https://azuremarketplace.microsoft.com). Las ofertas de servicios administrados le permiten ofrecer servicios de administración de recursos a los clientes con la administración de recursos delegados de Azure. Puede hacer que estas ofertas estén disponibles para todos los clientes potenciales o solo para uno o varios clientes específicos. Dado que los clientes facturan directamente los costos relacionados con estos servicios administrados, Microsoft no aplica ningún cargo.

## <a name="understand-managed-services-offers"></a>Comprender las ofertas de servicios administrados

Las ofertas de servicios administrados simplifican el proceso de incorporación de clientes para la administración de recursos delegados de Azure. Cuando un cliente compre una oferta en Azure Marketplace, podrá especificar los suscripciones o grupos de recursos que deben incorporarse. Tenga en cuenta que la incorporación de la suscripción se debe autorizar de antemano mediante el registro manual del proveedor de recursos **Microsoft.ManagedServices**.

Después, los usuarios de la organización podrán realizar tareas de administración para esos recursos desde dentro del inquilino de la organización en función del acceso que se haya definido al crear la oferta en [Cloud Partner Portal](https://cloudpartner.azure.com/). Esto se realiza mediante un manifiesto que especifica los usuarios, grupos y entidades de servicio de Azure AD que tendrán acceso a los recursos del cliente mediante la administración de recursos delegados de Azure, junto con los roles que definen su nivel de acceso. Al asignar permisos a un grupo de Azure AD, en lugar de una serie de cuentas de usuario o de aplicación individuales, puede agregar o quitar usuarios individuales cuando cambien sus requisitos de acceso.

## <a name="public-and-private-offers"></a>Ofertas públicas y privadas

Cada oferta de servicios administrados incluye uno o varios planes. Estos planes pueden ser privados o públicos. 

Si quiere limitar su oferta a clientes específicos, puede publicar un plan privado. Al hacerlo, el plan solo se puede comprar para los identificadores de suscripción específicos que proporcione. Para obtener más información, consulte [Ofertas privadas](../../marketplace/private-offers.md).

Los planes públicos permiten promover los servicios para nuevos clientes. Suelen ser más adecuadas cuando solo se necesita acceso limitado al inquilino del cliente. Una vez establecida una relación con un cliente, si decide conceder acceso adicional a su organización, puede hacerlo mediante la publicación de un nuevo plan privado solo para ese cliente, o bien [mediante su incorporación para ampliar su acceso mediante plantillas de Azure Resource Manager](../how-to/onboard-customer.md).

Si es necesario, puede incluir planes públicos y privados en la misma oferta.

> [!IMPORTANT]
> Una vez publicado un plan como público, no puede cambiarlo a privado. Para controlar qué clientes pueden aceptar su oferta y delegar recursos, use un plan privado. Con un plan público, no puede restringir la disponibilidad a determinados clientes ni a un determinado número de clientes, aunque puede dejar de vender el plan por completo si decide hacerlo. Puede [quitar el acceso a una delegación](../how-to/onboard-customer.md#remove-access-to-a-delegation) después de que un cliente acepte una oferta solo si incluyó una **autorización** con la **definición de rol** establecida en [Rol para eliminar la asignación de registros de servicios administrados](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) al publicar la oferta. También puede ponerse en contacto con el cliente y solicitarle [que quite el acceso](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

## <a name="publish-managed-service-offers"></a>Publicar ofertas de servicio administrado

Para obtener información sobre cómo publicar una oferta de servicios administrados, consulte [Publish a Managed Services offer to Azure Marketplace](../how-to/publish-managed-services-offers.md) (Publicación de una oferta de servicios administrados en Azure Marketplace). Para obtener información general sobre la publicación en Azure Marketplace mediante Cloud Partner Portal, consulte [Azure Marketplace and AppSource Publishing Guide](../../marketplace/marketplace-publishers-guide.md) (Guía de publicación de Azure Marketplace y AppSource) y [Administración de las ofertas del Marketplace de Azure y AppSource](../../marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de la [administración de recursos delegados de Azure](azure-delegated-resource-management.md) y las [experiencias de administración entre inquilinos](cross-tenant-management-experience.md).
- [Publicar ofertas de servicios administrados](../how-to/publish-managed-services-offers.md) en Azure Marketplace.
