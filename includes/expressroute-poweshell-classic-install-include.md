---
author: cherylmc
ms.date: 12/06/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: f04861d55c9cea3c79f4983f7be2e1f5a3c6d864
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74926217"
---
Instale las versiones más recientes de los módulos de ExpressRoute y de PowerShell de Azure Service Management (SM). No se puede usar el entorno de Azure CloudShell para ejecutar módulos de SM.

1. Siga las instrucciones del artículo [Instalación del módulo administración de servicios](/powershell/azure/servicemanagement/install-azure-ps) para instalar el módulo de administración de servicios de Azure. Si ya tiene instalado el módulo AZ o RM, asegúrese de usar "-AllowClobber".
2. Importe los módulos instalados. Cuando use el ejemplo siguiente, ajuste la ruta de acceso para que refleje tanto la ubicación como la versión de sus módulos de PowerShell instalados.

   ```powershell
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\Azure.psd1'
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\ExpressRoute\ExpressRoute.psd1'
   ```
3. Para iniciar sesión en su cuenta de Azure, abra la consola de PowerShell con privilegios elevados y conéctela a su cuenta. Use el ejemplo siguiente de ayuda para conectarse mediante el módulo de administración de servicios:

   ```powershell
   Add-AzureAccount
   ```