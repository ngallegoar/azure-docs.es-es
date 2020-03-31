---
title: Cifrado de recursos seguros en Automation
description: Azure Automation protege los recursos seguros mediante varios niveles de cifrado. De forma predeterminada, el cifrado se realiza mediante claves administradas por Microsoft. Los clientes pueden configurar sus cuentas de automatización para usar claves administradas por el cliente para el cifrado. En este artículo se describen los detalles de ambos modos de cifrado y cómo se puede cambiar entre los dos.
services: automation
ms.service: automation
ms.subservice: process-automation
author: snehithm
ms.author: snmuvva
ms.date: 01/11/2020
ms.topic: conceptual
manager: kmadnani
ms.openlocfilehash: 3c21e2fcdde9bffac91af56d49dfa0bf336e8c0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78246245"
---
# <a name="secure-assets-in-azure-automation"></a>Recursos protegidos en Azure Automation

Los recursos protegidos en Azure Automation incluyen credenciales, certificados, conexiones y variables cifradas. Estos recursos se protegen en Azure Automation mediante varios niveles de cifrado. En función de la clave de nivel superior utilizada para el cifrado, hay dos modelos de cifrado:
-   Usando claves administradas por Microsoft
-   Usando claves administradas por el cliente

## <a name="microsoft-managed-keys"></a>Claves administradas por Microsoft

De forma predeterminada, la cuenta de Azure Automation usa claves de cifrado administradas por Microsoft.

Cada recurso seguro se cifra y se almacena en Azure Automation con una clave única(clave de cifrado de datos) que se genera para cada cuenta de Automation. Estas claves se cifran y almacenan en Azure Automation con otra clave única que se genera para cada cuenta, denominada clave de cifrado de cuenta (AEK). Estas claves de cifrado de cuenta se cifran y almacenan en Azure Automation mediante claves administradas por Microsoft. 

## <a name="customer-managed-keys-with-key-vault-preview"></a>Claves administradas por el cliente con Key Vault (versión preliminar)

Puede administrar el cifrado de recursos seguros para su cuenta de Automation con sus propias claves. Cuando especifica una clave administrada por el cliente en el nivel de la cuenta de Automation, esa clave se usa para proteger y controlar el acceso a la clave de cifrado de cuenta para la cuenta de Automation. Esta, a su vez, se usa para cifrar y descifrar todos los recursos seguros. Las claves administradas por el cliente ofrecen más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. También puede auditar las claves de cifrado que se usan para proteger los recursos seguros.

Use Azure Key Vault para almacenar las claves administradas por el cliente. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas.  Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../key-vault/key-vault-overview.md)

## <a name="enable-customer-managed-keys-for-an-automation-account"></a>Habilitación de las claves administradas por el cliente para una cuenta de Automation

Al habilitar el cifrado con claves administradas por el cliente para una cuenta de Automation, Azure Automation encapsula la clave de cifrado de la cuenta con la clave administrada por el cliente en el almacén de claves asociado. La habilitación de claves administradas por el cliente no afecta al rendimiento, y la cuenta se cifra con la nueva clave inmediatamente sin retraso alguno.

Las cuentas de Automation nuevas siempre se cifran mediante claves administradas por Microsoft. No es posible habilitar claves administradas por el cliente en el momento en que se crea la cuenta. Las claves administradas por el cliente se almacenan en Azure Key Vault, y el almacén de claves se debe aprovisionar con directivas de acceso que concedan permisos de clave a la identidad administrada que está asociada a la cuenta de Automation. La identidad administrada solo está disponible después de crear la cuenta de almacenamiento.

Al modificar la clave que se usa para el cifrado de recursos seguros de Azure Automation habilitando o deshabilitando las claves administradas por el cliente, actualizando la versión de la clave o especificando una clave diferente, el cifrado de la clave de cifrado de cuenta cambia, pero los recursos seguros de su cuenta de Azure Storage no tienen que volver a cifrarse.

En las tres secciones siguientes se describe la mecánica para habilitar las claves administradas por el cliente para una cuenta de Automation. 

> [!NOTE] 
> Para habilitar las claves administradas por el cliente, tiene que realizar llamadas a la API REST de Azure Automation con la versión de API 2020-01-13-preview.

### <a name="pre-requisites-for-using-customer-managed-keys-in-azure-automation"></a>Requisitos previos para el uso de claves administradas por el cliente en Azure Automation

Antes de habilitar las claves administradas por el cliente para una cuenta de Automation, debe asegurarse de que se cumplen los siguientes requisitos previos:

 - La clave administrada por el cliente se almacena en Azure Key Vault. 
 - Habilite las propiedades **Eliminación temporal** y **No purgar** en el almacén de claves. Estas características son necesarias para permitir la recuperación de claves en caso de que se eliminen accidentalmente.
 - Solo se admiten claves RSA con para el cifrado de Azure Automation. Para obtener más información acerca de las claves, consulte [Información acerca de claves, secretos y certificados de Azure Key Vault](../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).
- La cuenta de Automation y el almacén de claves pueden estar en distintas suscripciones, pero deben estar en el mismo inquilino de Azure Active Directory.

### <a name="assign-an-identity-to-the-automation-account"></a>Asignación de una identidad a la cuenta de Automation

Para usar las claves administradas por el cliente con una cuenta de Automation, la cuenta de Automation debe autenticarse en el almacén de claves que contiene las claves administradas por el cliente. Azure Automation usa identidades administradas asignadas por el sistema para autenticar la cuenta con Azure Key Vault. Para obtener más información sobre las identidades administradas, consulte [¿Qué es Managed Identities for Azure Resources?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

Configure una identidad administrada asignada por el sistema a la cuenta de Automation mediante la siguiente llamada a la API REST.

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Cuerpo de la solicitud:

```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```

La identidad asignada por el sistema para la cuenta de Automation se devuelve en una respuesta similar a la siguiente:

```json
{
 "name": "automation-account-name",
 "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
 ..
 "identity": {
    "type": "SystemAssigned",
    "principalId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
    "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb"
 },
..
}
```

### <a name="configure-the-key-vault-access-policy"></a>Configuración de la directiva de acceso de Key Vault

Una vez que se asigna una identidad administrada a la cuenta de Automation, se configura el acceso al almacén de claves que contiene las claves administradas por el cliente. Azure Automation requiere **get**, **recover**, **wrapKey** y **UnwrapKey** en las claves administradas por el cliente.

Este tipo de directiva de acceso se puede establecer mediante la siguiente llamada a la API REST:

```http
PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sample-group/providers/Microsoft.KeyVault/vaults/sample-vault/accessPolicies/add?api-version=2018-02-14
```

Cuerpo de la solicitud:

```json
{
  "properties": {
    "accessPolicies": [
      {
        "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
        "objectId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
        "permissions": {
          "keys": [
            "get",
            "recover",
            "wrapKey",
            "unwrapKey"
          ],
          "secrets": [],
          "certificates": []
        }
      }
    ]
  }
}
```

> [!NOTE]
> Se deben proporcionar los campos **tenantId** y **objectId** con valores de **identity.tenantId** y **identity.principalId** respectivamente de la respuesta de la identidad administrada para la cuenta de Automation.

### <a name="change-the-configuration-of-automation-account-to-use-customer-managed-key"></a>Cambio de la configuración de la cuenta de Automation para usar la clave administrada por el cliente

Por último, puede cambiar la cuenta de Automation de las claves administradas por Microsoft a las claves administradas por el cliente, mediante la siguiente llamada a la API REST:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Cuerpo de la solicitud:

```json
 {
    "properties": {
      "encryption": {
        "keySource": "Microsoft.Keyvault",
        "keyvaultProperties": {
          "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
        }
      }
    }
  }
```

Respuesta de muestra

```json
{
  "name": "automation-account-name",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
  ..
  "properties": {
    ..
    "encryption": {
      "keyvaultProperties": {
         "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
      },
      "keySource": "Microsoft.Keyvault"
    },
    ..
  }
}
```

## <a name="manage-customer-managed-keys-lifecycle"></a>Ciclo de vida de administración de las claves administradas por el cliente

### <a name="rotate-customer-managed-keys"></a>Rotación de claves administradas por el cliente

Las claves administradas por el cliente se pueden rotar en Azure Key Vault según las directivas de cumplimiento. Cuando la clave rota, hay que actualizar la cuenta de Automation para usar el nuevo identificador URI de la clave.

La rotación de la clave no desencadena un nuevo cifrado de los recursos seguros en la cuenta de Automation. No se requiere ninguna acción adicional.

### <a name="revoke-access-to-customer-managed-keys"></a>Revocación del acceso a las claves administradas por el cliente

Para revocar el acceso a las claves administradas por el cliente, use PowerShell o la CLI de Azure. Para más información, consulte la referencia de [PowerShell para Azure Key Vault](https://docs.microsoft.com/powershell/module/az.keyvault/) o la referencia de la [CLI para Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los recursos seguros de la cuenta de Automation, ya que Azure Automation no puede acceder a la clave de cifrado.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../key-vault/key-vault-overview.md)

- [Recursos de certificados en Azure Automation](shared-resources/certificates.md)

- [Recursos de credenciales en Azure Automation](shared-resources/credentials.md)

- [Recursos de variables en Azure Automation](shared-resources/variables.md)
