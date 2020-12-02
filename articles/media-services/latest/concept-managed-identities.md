---
title: Identidades administradas y almacenamiento de confianza con Media Services
description: Media Services puede usarse con identidades administradas para habilitar el almacenamiento de confianza.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 11/04/2020
ms.author: inhenkel
ms.openlocfilehash: d0811e8f9183ee334d413bcad69f2c7b32023be3
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499363"
---
# <a name="managed-identities-and-trusted-storage-with-media-services"></a>Identidades administradas y almacenamiento de confianza con Media Services

Media Services puede usarse con [identidades administradas](../../active-directory/managed-identities-azure-resources/overview.md) para habilitar el almacenamiento de confianza. Al crear una cuenta de Media Services, debe asociarla a una cuenta de almacenamiento. Media Services puede acceder a esa cuenta de almacenamiento mediante la autenticación del sistema. Media Services valida que la cuenta de Media Services y la cuenta de almacenamiento se encuentran en la misma suscripción y valida que el usuario que agrega la asociación tenga acceso a la cuenta de almacenamiento con Azure Resource Manager RBAC.

## <a name="trusted-storage"></a>Almacenamiento de confianza

No obstante, si quiere usar un firewall para proteger su cuenta de almacenamiento, debe emplear la autenticación de identidad administrada. Esta permite que Media Services acceda a la cuenta de almacenamiento que se ha configurado con un firewall o una restricción de red virtual a través del acceso de almacenamiento de confianza.  Para más información acerca de los servicios de Microsoft de confianza, consulte [Configuración de redes virtuales y firewalls de Azure Storage](../../storage/common/storage-network-security.md#trusted-microsoft-services).

## <a name="media-services-managed-identity-scenarios"></a>Escenarios de identidad administrada de Media Services

Actualmente hay dos escenarios en los que la identidad administrada se puede usar con Media Services:

- Usar la identidad administrada de la cuenta de Media Services para acceder a las cuentas de almacenamiento.

- Usar la identidad administrada de la cuenta de Media Services para acceder a las claves de cliente de una instancia de Key Vault.

En las dos secciones siguientes se describen las diferencias entre ambos escenarios.

### <a name="use-the-managed-identity-of-the-media-services-account-to-access-storage-accounts"></a>Uso de la identidad administrada de la cuenta de Media Services para acceder a las cuentas de almacenamiento

1. Cree una cuenta de Media Services con una identidad administrada.
1. Conceda a la entidad de seguridad administrada acceso a una cuenta de almacenamiento de su propiedad.
1. De este modo, Media Services podrá acceder a la cuenta de almacenamiento en su nombre mediante la identidad administrada.

### <a name="use-the-managed-identity-of-the-media-services-account-to-access-key-vault-to-access-customer-keys"></a>Uso de la identidad administrada de la cuenta de Media Services para acceder a las claves de cliente de una instancia de Key Vault

1. Cree una cuenta de Media Services con una identidad administrada.
1. Conceda acceso a la entidad de seguridad administrada a una instancia de Key Vault de su propiedad.
1. Configure la cuenta de Media Services para usar el cifrado de cuenta basado en claves de cliente.
1. Media Services accede a Key Vault en su nombre mediante la identidad administrada.

Para obtener más información acerca de las claves administradas por el cliente y Key Vault, consulte [Bring Your Own Key (claves administradas por el cliente) con Media Services](concept-use-customer-managed-keys-byok.md)

## <a name="tutorials"></a>Tutoriales

Estos tutoriales incluyen los dos escenarios mencionados anteriormente.

- [Uso de Azure Portal para emplear claves administradas por el cliente o BYOK con Media Services](tutorial-byok-portal.md)
- [Uso de claves administradas por el cliente o BYOK con la API REST de Media Services](tutorial-byok-postman.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las funciones de las identidades administradas y su uso con las aplicaciones de Azure, consulte [Identidades administradas de Azure AD](../../active-directory/managed-identities-azure-resources/overview.md).