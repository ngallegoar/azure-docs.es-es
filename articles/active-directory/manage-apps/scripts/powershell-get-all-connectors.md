---
title: 'Ejemplo de PowerShell: lista de todos los grupos del conector de Application Proxy'
description: Ejemplo de PowerShell en el que se enumeran todos los conectores y grupos de conectores de Azure Active Directory (Azure AD) Application Proxy en el directorio.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53af7f3f9049edc23ed4226f43e7fe67398c29f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75475642"
---
# <a name="get-all-connector-groups-and-connectors-in-the-directory"></a>Obtención de todos los grupos de conectores y conectores del directorio

En este ejemplo de script de PowerShell se enumeran todos los conectores y grupos de conectores de Azure Active Directory (Azure AD) Application Proxy en el directorio.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

El ejemplo requiere el [módulo Azure AD V2 PowerShell para Graph](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (Azure AD) o la [versión preliminar del módulo Azure AD V2 PowerShell para Graph](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (versión preliminar de Azure AD).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-connectors.ps1 "Get all connector groups and connectors in the directory")]

## <a name="script-explanation"></a>Explicación del script

| Get-Help | Notas |
|---|---|
| [Get-AzureADApplicationProxyConnectorGroup](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup?view=azureadps-2.0) | Recupera una lista de todos los grupos de conectores o, si se especifican, los detalles del grupo de conectores especificado. |
| [Get-AzureADApplicationProxyConnectorGroupMembers](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyconnectorgroupmembers?view=azureadps-2.0) | Obtiene todos los conectores de Application Proxy asociados a cada grupo de conectores.|

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure AD PowerShell, consulte [Información general del módulo de Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

Para obtener otros ejemplos de PowerShell para Application Proxy, consulte [Ejemplos de Azure AD PowerShell para Azure AD Application Proxy](../application-proxy-powershell-samples.md).