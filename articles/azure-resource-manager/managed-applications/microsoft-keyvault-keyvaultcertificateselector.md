---
title: Elemento de interfaz de usuario KeyVaultCertificateSelector
description: Se describe el elemento de interfaz de usuario Microsoft.KeyVault.KeyVaultCertificateSelector para Azure Portal.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/27/2020
ms.author: tomfitz
ms.openlocfilehash: 1ac6fd5a3f1e581fc9a676fe7f8f27b9742e1581
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93101228"
---
# <a name="microsoftkeyvaultkeyvaultcertificateselector-ui-element"></a>Elemento de interfaz de usuario Microsoft.KeyVault.KeyVaultCertificateSelector

Un control para seleccionar el certificado de un almacén de claves.

## <a name="ui-sample"></a>Ejemplo de interfaz de usuario

Al usuario se le presenta la opción de seleccionar un certificado disponible.

:::image type="content" source="./media/managed-application-elements/microsoft-keyvault-keyvaultcertificateselector-select.png" alt-text="Microsoft.KeyVault.KeyVaultCertificateSelector":::

Después de seleccionar la opción **Seleccionar un certificado** , el usuario puede especificar un almacén de claves y un certificado del almacén de claves.

:::image type="content" source="./media/managed-application-elements/microsoft-keyvault-keyvaultcertificateselector-certificate.png" alt-text="Seleccionar certificado con Microsoft.KeyVault.KeyVaultCertificateSelector":::

El control se actualiza para mostrar el nombre del almacén de claves y el certificado seleccionados.

:::image type="content" source="./media/managed-application-elements/microsoft-keyvault-keyvaultcertificateselector-result.png" alt-text="Mostrar certificado seleccionado Microsoft.KeyVault.KeyVaultCertificateSelector":::

## <a name="schema"></a>Schema

```json
{
  "name": "keyVaultCertificateSelection",
  "type": "Microsoft.KeyVault.KeyVaultCertificateSelector",
  "visible": true,
  "toolTip": "Select certificate",
  "label": "KeyVault certificates selection"
}
```

## <a name="sample-output"></a>Salida de ejemplo

```json
{
  "keyVaultName": "azuretestkeyvault1",
  "keyVaultId": "/subscriptions/{subscription-id}/resourceGroups/resourcegroup1/providers/Microsoft.KeyVault/vaults/azuretestkeyvault1",
  "certificateName": "certificate1",
  "certificateUrl": "https://azuretestkeyvault1.vault.azure.net/secrets/certificate1/{id}",
  "certificateThumbprint": "1B721E84DDDD1BB69282B4A54F18C6ADB1C174F2"
}
```

## <a name="next-steps"></a>Pasos siguientes

* Para ver una introducción sobre la creación de definiciones de interfaz de usuario, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).
* Para ver una descripción de las propiedades comunes de los elementos de interfaz de usuario, consulte [Elementos CreateUiDefinition](create-uidefinition-elements.md).
