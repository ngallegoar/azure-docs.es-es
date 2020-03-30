---
title: 'Tipo de asignación de escritorio personal de Windows Virtual Desktop: Azure'
description: Procedimiento para configurar el tipo de asignación para un grupo de host de escritorio personal de Windows Virtual Desktop.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 41b24a94d36b21fe5d5f539e056abb535bda433a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128290"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Configuración del tipo de asignación de grupo de host de escritorio personal

Puede configurar el tipo de asignación del grupo de host de escritorio personal para ajustar el entorno de Windows Virtual Desktop para que se adapte mejor a sus necesidades. En este tema, se mostrará cómo configurar la asignación automática o directa para los usuarios.

>[!NOTE]
> Las instrucciones de este artículo solo se aplican a los grupos de host de escritorio personal, no a los grupos de host agrupados, ya que los usuarios de grupos de host agrupados no se asignan a hosts de sesión específicos.

## <a name="configure-automatic-assignment"></a>Configuración de la asignación automática

La asignación automática es el tipo de asignación predeterminado para los nuevos grupos de host de escritorio personal creados en el entorno de Windows Virtual Desktop. La asignación automática de usuarios no requiere un host de sesión específico.

Para asignar usuarios de forma automática, asígnelos primero al grupo de host de escritorio personal para que puedan ver el escritorio en su fuente. Cuando un usuario asignado inicia el escritorio en su fuente, notificará un host de sesión disponible si todavía no se ha conectado al grupo de host, que completa el proceso de asignación.

Antes de empezar, [descargue e importe el módulo de PowerShell para Windows Virtual Desktop](/powershell/windows-virtual-desktop/overview/), si todavía no lo ha hecho. 

> [!NOTE]
> Asegúrese de haber instalado el módulo de PowerShell para Windows Virtual Desktop versión 1.0.1534.2001 o posterior antes de seguir estas instrucciones.

Después, ejecute el siguiente cmdlet para iniciar sesión en su cuenta:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Para configurar un grupo de host a fin de asignar automáticamente usuarios a las máquinas virtuales, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Para asignar un usuario al grupo de host de escritorio personal, ejecute el siguiente cmdlet de PowerShell:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Configuración de la asignación directa

A diferencia de la asignación automática, cuando se usa la asignación directa, debe asignar el usuario al grupo de host de escritorio personal y a un host de sesión específico antes de que se pueda conectar a su escritorio personal. Si el usuario solo se asigna a un grupo de host sin una asignación de host de sesión, no podrá acceder a los recursos.

Para configurar un grupo de host para que requiera la asignación directa de usuarios a los hosts de sesión, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Para asignar un usuario al grupo de host de escritorio personal, ejecute el siguiente cmdlet de PowerShell:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Para asignar un usuario a un host de sesión específico, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado el tipo de asignación de escritorio personal, puede iniciar sesión en un cliente de Windows Virtual Desktop para probarlo como parte de una sesión de usuario. En las dos guías paso a paso siguientes se indica cómo conectarse a una sesión mediante el cliente que elija:

- [Conexión con el cliente de Escritorio de Windows](connect-windows-7-and-10.md)
- [Conexión con el cliente web](connect-web.md)
