---
title: 'Módulo de PowerShell para Windows Virtual Desktop: Azure'
description: Procedimientos de instalación y configuración del módulo de PowerShell para Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 54501e7e00ba8a28dd7cb421232b9a9587604338
ms.sourcegitcommit: d662eda7c8eec2a5e131935d16c80f1cf298cb6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82653133"
---
# <a name="set-up-the-powershell-module-for-windows-virtual-desktop"></a>Configuración del módulo de PowerShell para Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a la actualización de primavera de 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager.
>
> La actualización de primavera de 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. 
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El módulo de PowerShell para Windows Virtual Desktop está integrado en el módulo de Azure PowerShell. En este artículo se explica cómo configurar el módulo de PowerShell con el fin de ejecutar cmdlets para Windows Virtual Desktop.

## <a name="set-up-your-powershell-environment"></a>Configuración del entorno de PowerShell

Para empezar a usar el módulo, instale primero la [versión más reciente de PowerShell Core](/powershell/scripting/install/installing-powershell#powershell-core). Los cmdlets de Windows Virtual Desktop solo funcionan actualmente con PowerShell Core.

A continuación, deberá instalar el módulo DesktopVirtualization para usarlo en su sesión de PowerShell.

Ejecute el siguiente cmdlet de PowerShell con permisos elevados para instalar el módulo:

```powershell
Install-Module -Name Az.DesktopVirtualization
```

>[!NOTE]
> Si este cmdlet no funciona, intente ejecutarlo de nuevo con permisos elevados.

Use el siguiente cmdlet de PowerShell para conectarse a Azure:

```powershell
Connect-AzAccount
```

Si va a iniciar sesión en su cuenta de Azure, necesitará un código que se genera al ejecutar el cmdlet Connect. Para iniciar sesión, vaya a <https://microsoft.com/devicelogin>, escriba el código y, a continuación, utilice sus credenciales de administrador de Azure.

```powershell
Account SubscriptionName TenantId Environment

------- ---------------- -------- -----------

Youradminupn subscriptionname AzureADTenantID AzureCloud
```

De este modo, iniciará sesión directamente en la suscripción predeterminada de sus credenciales de administrador.

## <a name="change-the-default-subscription"></a>Cambio de la suscripción predeterminada

Si desea cambiar la suscripción predeterminada después de iniciar sesión, ejecute el siguiente cmdlet:

```powershell
Select-AzureSubscription -SubscriptionName <preferredsubscriptionname>
```

Si selecciona una nueva suscripción, no es necesario que especifique el identificador de la suscripción en los cmdlets que ejecute posteriormente. Por ejemplo, el siguiente cmdlet recupera un host de sesión concreto, sin necesidad de especificar el identificador de suscripción:

```powershell
Get-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname>
```

También puede cambiar las suscripciones para cada cmdlet. Para ello, agregue el nombre de suscripción que desee como parámetro. El siguiente cmdlet es el mismo que en el ejemplo anterior, excepto que se ha agregado el identificador de suscripción como parámetro para cambiar la suscripción que el cmdlet utiliza.

```powershell
Get-AzWvdSessionHost -HostPoolName <hostpoolname> -Name <sessionhostname> -ResourceGroupName <resourcegroupname> -SubscriptionId <subscriptionGUID>
```

## <a name="get-locations"></a>Obtención de ubicaciones

El parámetro de ubicación es obligatorio para todos los cmdlets **New-AzWVD** que crean nuevos** objetos.

Ejecute el siguiente cmdlet para obtener una lista de las ubicaciones admitidas para su suscripción:

```powershell
Get-AzLocation
```

El resultado de **Get-AzLocation** será similar al siguiente:

```powershell
Location : eastasia

DisplayName : East Asia

Providers : {Microsoft.RecoveryServices, Microsoft.ManagedIdentity,
Microsoft.SqlVirtualMachine, microsoft.insightsΓÇª}

Location : southeastasia

DisplayName : Southeast Asia

Providers : {Microsoft.RecoveryServices, Microsoft.ManagedIdentity,
Microsoft.SqlVirtualMachine, microsoft.insightsΓÇª}

Location : centralus

DisplayName : Central US

Providers : {Microsoft.RecoveryServices, Microsoft.DesktopVirtualization,
Microsoft.ManagedIdentity, Microsoft.SqlVirtualMachineΓÇª}

Location : eastus

DisplayName : East US

Providers : {Microsoft.RecoveryServices, Microsoft.DesktopVirtualization,
Microsoft.ManagedIdentity, Microsoft.SqlVirtualMachineΓÇª}
```

Una vez que conozca la ubicación de su cuenta, podrá usarla en un cmdlet. Por ejemplo, a continuación se incluye un cmdlet que crea un grupo de hosts en la ubicación "southeastasia":

```powershell
New-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -WorkspaceName <workspacename> -Location “southeastasia”
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado el módulo de PowerShell, puede ejecutar cmdlets para realizar todo tipo de tareas en Windows Virtual Desktop. Estos son algunos de los lugares en los que puede usar su módulo:

- Realice nuestros [tutoriales de Windows Virtual Desktop]() para configurar su propio entorno de Windows Virtual Desktop.
- [Creación de un grupo host con PowerShell](create-host-pools-powershell.md)
- [Configuración del método de equilibrio de carga de Windows Virtual Desktop](configure-host-pool-load-balancing.md)
- [Configuración del tipo de asignación de grupo de host de escritorio personal](configure-host-pool-personal-desktop-assignment-type.md)
- Y mucho más.

Si tiene algún problema, consulte nuestro [artículo de solución de problemas de PowerShell](troubleshoot-powershell.md) para obtener ayuda.

