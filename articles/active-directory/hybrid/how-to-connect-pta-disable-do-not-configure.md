---
title: Deshabilitación de la autenticación de paso a través mediante la característica "No configurar" de Azure AD Connect | Microsoft Docs
description: En este artículo se describe cómo deshabilitar la autenticación de paso a través (PTA) con la característica "No configurar" de Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 04/20/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa1046dc64fed3edb6c9d04f76a96f488769ff42
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81725765"
---
# <a name="disable-pta-when-using-azure-ad-connect-do-not-configure"></a>Deshabilitación de la autenticación de paso a través con la opción "No configurar" de Azure AD Connect

Si usa la autenticación de paso a través (PTA) con Azure AD Connect y está establecida en "No configurar", puede deshabilitarla. La deshabilitación de PTA se puede realizar mediante los siguientes cmdlets. 

## <a name="prerequisites"></a>Prerrequisitos
Se necesitan los siguientes requisitos previos:
- Una máquina Windows que tenga instalado el agente de PTA. 
- El agente debe estar en la versión 1.5.1742.0 o posterior. 
- Una cuenta de administrador global de Azure para ejecutar los cmdlets de PowerShell que deshabilitan PTA.

>[!NOTE]
> Si el agente es de una versión anterior, es posible que no tenga los cmdlets necesarios para completar esta operación. Puede obtener un nuevo agente desde Azure Portal, instalarlo en cualquier máquina Windows y proporcionar credenciales de administrador. La instalación del agente no afecta al estado de PTA en la nube.

> [!IMPORTANT]
> Si usa la nube de Azure Government, tendrá que pasar el parámetro ENVIRONMENTNAME con el siguiente valor. 
>
>| Nombre del entorno | Nube |
>| - | - |
>| AzureUSGovernment | US Gov|


## <a name="to-disable-pta"></a>Para deshabilitar PTA
Desde una sesión de PowerShell, use lo siguiente para deshabilitar PTA:
1. PS C:\Archivos de programa\Microsoft Azure AD Connect Authentication Agent> `Import-Module .\Modules\PassthroughAuthPSModule`
2. `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth` o `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth -EnvironmentName <identifier>`
3. `Disable-PassthroughAuthentication  -Feature PassthroughAuth` o `Disable-PassthroughAuthentication -Feature PassthroughAuth -EnvironmentName <identifier>`

## <a name="if-you-dont-have-access-to-an-agent"></a>Si no tiene acceso a ningún agente

Si no tiene una máquina de agente, puede usar el siguiente comando para instalar un agente.

1. Descargue el agente de autenticación más reciente de portal.azure.com.
2. Instale la característica: `.\AADConnectAuthAgentSetup.exe` o `.\AADConnectAuthAgentSetup.exe ENVIRONMENTNAME=<identifier>`


## <a name="next-steps"></a>Pasos siguientes

- [Inicio de sesión del usuario con la autenticación de paso a través de Azure Active Directory](how-to-connect-pta.md)