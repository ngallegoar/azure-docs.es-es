---
title: 'Solución de problemas con la conexión al servicio de Windows Virtual Desktop: Azure'
description: Cómo resolver problemas al configurar conexiones de servicio en un entorno de inquilinos de Windows Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 09/14/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5eb5602b8330906311df4a0d1f59bc5e5130237e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90089911"
---
# <a name="windows-virtual-desktop-service-connections"></a>Conexiones al servicio de Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa Windows Virtual Desktop (clásico) sin objetos para Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/troubleshoot-service-connection-2019.md).

Use este artículo para resolver problemas con conexiones de cliente de Windows Virtual Desktop.

## <a name="provide-feedback"></a>Envío de comentarios

Puede proporcionarnos comentarios y hablar sobre el servicio Windows Virtual Desktop con el equipo del producto y otros miembros activos de la comunidad en la [comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>El usuario se conecta, pero no se muestra nada (ninguna fuente)

Un usuario puede iniciar clientes de Escritorio remoto y es capaz de realizar la autenticación, pero no ve ningún icono en la fuente de detección de web.

1. Confirme que el usuario que notificó los problemas se ha asignado a grupos de aplicaciones mediante el uso de esta línea de comandos:

     ```powershell
     Get-AzRoleAssignment -SignInName <userupn>
     ```

2. Confirme que el usuario inicia sesión con las credenciales correctas.

3. Si se usa el cliente web, confirme que no hay ningún problema de credenciales almacenadas en caché.

4. Si el usuario forma parte de un grupo de usuarios de Azure Active Directory (AD), asegúrese de que dicho grupo es un grupo de seguridad en lugar de un grupo de distribución. Windows Virtual Desktop no admite los grupos de distribución de Azure AD.

## <a name="user-loses-existing-feed-and-no-remote-resource-is-displayed-no-feed"></a>El usuario pierde la fuente existente y no se muestra ningún recurso remoto (sin fuente)

Este error suele aparecer después de que un usuario haya trasladado su suscripción de un inquilino de Azure AD a otro. Como resultado, el servicio pierde el seguimiento de las asignaciones de usuario, ya que siguen estando asociadas al inquilino de Azure AD anterior.

Para resolver esto, lo único que debe hacer es reasignar los usuarios a sus grupos de aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Windows Virtual Desktop y las pistas de escalación, consulte [Introducción, comentarios y soporte técnico para solucionar problemas](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un entorno de Windows Virtual Desktop y de un grupo de hosts de este, consulte [Creación de entorno y grupo de hosts](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Windows Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas al usar PowerShell con Windows Virtual Desktop, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
