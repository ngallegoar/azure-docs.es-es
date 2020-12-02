---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: 27da28073b01c6bc43868172d6c989d9518d8f53
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95563835"
---
## <a name="start-your-powershell-session"></a>Inicio de una sesión de PowerShell
 

Ejecute el cmdlet [**Connect-Az Account**](/powershell/module/Az.Accounts/Connect-AzAccount) y aparecerá una pantalla de inicio de sesión donde podrá especificar sus credenciales. Use las mismas credenciales que las utilizadas para iniciar sesión en el Portal de Azure.

```powershell
Connect-AzAccount
```

Si tiene varias suscripciones, use el cmdlet [**Set-AzContext**](/powershell/module/az.accounts/set-azcontext) para seleccionar la suscripción que se debe usar en la sesión de PowerShell. Para ver la suscripción que se usa en la sesión actual de PowerShell, ejecute [**Get-AzContext**](/powershell/module/az.accounts/get-azcontext). Para ver todas las suscripciones, ejecute [**Get-AzSubscription**](/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```