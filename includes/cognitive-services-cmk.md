---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.custom: include
ms.openlocfilehash: e5e1755e6351d308c041de5cefe943e9874c8ebd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372025"
---
### <a name="enable-customer-managed-keys"></a>Habilitar claves administradas del cliente

Debe usar Azure Key Vault para almacenar las claves administradas por el cliente. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. El recurso de Cognitive Services y el almacén de claves deben estar en la misma región y en el mismo inquilino de Azure Active Directory (Azure AD), pero pueden estar en distintas suscripciones. Para más información sobre Azure Key Vault, vea [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

Los recursos nuevos de Cognitive Services siempre se cifran mediante claves administradas por Microsoft. No es posible habilitar claves administradas por el cliente en el momento en que se crea el recurso. Las claves administradas por el cliente se almacenan en Azure Key Vault y el almacén de claves se debe aprovisionar con directivas de acceso que concedan permisos de clave a la identidad administrada que está asociada al recurso de Cognitive Services. La identidad administrada solo está disponible después de crear el recurso con el plan de tarifa para CMK.

Para aprender a usar claves administradas por el cliente con Azure Key Vault para el cifrado de Cognitive Services, consulte:

- [Configuración de claves administradas por el cliente con Key Vault para el cifrado de Cognitive Services mediante Azure Portal](..\articles\cognitive-services\Encryption\cognitive-services-encryption-keys-portal.md)

Al habilitar las claves administradas por el cliente también se habilitará una identidad administrada asignada por el sistema, una característica de Azure AD. Una vez habilitada la identidad administrada asignada por el sistema, este recurso se registrará con Azure Active Directory. Tras su registro, se concederá a la identidad administrada acceso a la instancia de Key Vault seleccionada durante la configuración de la clave administrada por el cliente. Puede obtener más información acerca de las [identidades administradas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> Si deshabilita las identidades administradas asignadas por el sistema, se quitará el acceso al almacén de claves y los datos cifrados con las claves de cliente dejarán de estar disponibles. Las características dependientes de estos datos dejarán de funcionar.

> [!IMPORTANT]
> Las identidades administradas no admiten actualmente escenarios entre directorios. Al configurar las claves administradas por el cliente en Azure Portal, se asigna automáticamente una identidad administrada en segundo plano. Si posteriormente mueve la suscripción, el grupo de recursos o el recurso de un directorio de Azure AD a otro, la identidad administrada asociada al recurso no se transfiere al nuevo inquilino, por lo que es posible que las claves administradas por el cliente dejen de funcionar. Para más información, vea **Transferencia de una suscripción entre directorios de Azure AD** en [Preguntas frecuentes y problemas conocidos con identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Almacenamiento de claves administradas por el cliente en Azure Key Vault

Para habilitar las claves administradas por el cliente, debe usar una instancia de Azure Key Vault para almacenarlas. Debe habilitar las propiedades **Eliminación temporal** y **No purgar** en el almacén de claves.

Las claves RSA de tamaño 2048 son las únicas que admite el cifrado de Cognitive Services. Para más información acerca de las claves, consulte la sección **Claves en Key Vault** en [Información acerca de claves, secretos y certificados de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys).

### <a name="rotate-customer-managed-keys"></a>Rotación de claves administradas por el cliente

Las claves administradas por el cliente se pueden rotar en Azure Key Vault según las directivas de cumplimiento. Si se rota la clave, es preciso actualizar el recurso de Cognitive Services para que use el identificador URI de la clave nueva. Para aprender a actualizar el recurso para que use una nueva versión de la clave en Azure Portal, consulte la sección **Actualización de la versión de la clave** en [Configuración de claves administradas por el cliente para Cognitive Services mediante Azure Portal](..\articles\cognitive-services\Encryption\cognitive-services-encryption-keys-portal.md).

La rotación de la clave no desencadena un nuevo cifrado de los datos del recurso. No es preciso que el usuario realice ninguna otra acción.

### <a name="revoke-access-to-customer-managed-keys"></a>Revocación del acceso a las claves administradas por el cliente

Para revocar el acceso a las claves administradas por el cliente, use PowerShell o la CLI de Azure. Para más información, consulte la referencia de [PowerShell para Azure Key Vault](https://docs.microsoft.com/powershell/module/az.keyvault//) o la referencia de la [CLI para Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los datos del recurso de Cognitive Services, ya que el servicio no puede acceder a la clave de cifrado.


