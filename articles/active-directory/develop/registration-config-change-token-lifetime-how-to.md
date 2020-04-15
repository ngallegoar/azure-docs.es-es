---
title: Cambio de los valores predeterminados de vigencia del token para aplicaciones personalizadas de Azure AD
description: Aprenda a actualizar las directivas de vigencia de los tokens para la aplicación que está desarrollando en Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ms.openlocfilehash: fc71ebe61fba8c1bdb6b7625b16a50d8995a581a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883360"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Modificación de los valores predeterminados de vigencia de los tokens en una aplicación personalizada

En este artículo se muestra cómo usar Azure AD PowerShell para establecer una directiva de vigencia del token. Azure AD Premium permite a los desarrolladores de aplicaciones y a los administradores de inquilinos configurar la vigencia de los tokens emitidos para clientes no confidenciales. Las directivas de vigencia de los tokens se establecen para todos los inquilinos o para los recursos a los que se va a acceder.

1. Para establecer una directiva de vigencia de tokens, debe descargar el [módulo Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).
1. Ejecute el comando **Connect-AzureAD -Confirm**.

    En esta directiva de ejemplo, se establece la actualización del token con arreglo a un único factor de antigüedad máxima. Cree la directiva: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Configurable token lifetimes in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) (Duraciones de tokens configurables) para aprender a configurar las duraciones de los tokens generadas por Azure AD, lo que incluye cómo establecer las duraciones de los tokens de todas las aplicaciones de su organización, de una aplicación multiinquilino o de una entidad de servicio concreta de su organización. 
* [Referencia de tokens de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)
