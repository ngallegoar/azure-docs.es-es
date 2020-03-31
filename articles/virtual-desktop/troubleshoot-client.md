---
title: 'Solución de problemas del cliente de Escritorio remoto de Windows Virtual Desktop: Azure'
description: Cómo resolver problemas al configurar conexiones de cliente en un entorno de inquilinos de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: e3a240901ffca2c126e2b61eaee0cf287cc31d6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127511"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Solución de problemas del cliente de Escritorio remoto

En este artículo se describen los problemas comunes relacionados con el cliente de Escritorio remoto y cómo corregirlos.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>El cliente de Escritorio remoto para Windows 7 o Windows 10 deja de responder o no se puede abrir

Use los siguientes cmdlets de PowerShell para limpiar los registros de cliente fuera de banda (OOB).

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

Vaya a **%AppData%\RdClientRadc** y elimine todo el contenido.

Desinstale y vuelva a instalar el cliente de Escritorio remoto para Windows 7 y Windows 10.

## <a name="web-client-wont-open"></a>El cliente web no se abre

En primer lugar, pruebe la conexión a Internet. Para ello, abra otro sitio web en el explorador; por ejemplo, [www.bing.com](https://www.bing.com).

Use **nslookup** para confirmar que DNS puede resolver el FQDN:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Intente conectarse con otro cliente, como el cliente de Escritorio remoto para Windows 7 o Windows 10, y compruebe si puede abrir el cliente web.

### <a name="opening-another-site-fails"></a>error al abrir otro sitio

Esto se debe normalmente a problemas de conexión de red o a una interrupción de la red. Se recomienda que se ponga en contacto con el soporte técnico de red.

### <a name="nslookup-cannot-resolve-the-name"></a>Nslookup no puede resolver el nombre

Esto se debe normalmente a problemas de conexión de red o a una interrupción de la red. Se recomienda que se ponga en contacto con el soporte técnico de red.

### <a name="your-client-cant-connect-but-other-clients-on-your-network-can-connect"></a>El cliente no se puede conectar, pero otros clientes de la red sí pueden

Si el explorador comienza a dar problemas o deja de funcionar mientras usa el cliente web, siga estas instrucciones para solucionar el problema:

1. Reinicie el explorador.
2. Borre las cookies del explorador. Consulte [Cómo eliminar archivos de cookies en Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Borre la memoria caché del explorador Consulte [Borrar la memoria caché del explorador](https://binged.it/2RKyfdU).
4. Abra el explorador en modo privado.

## <a name="web-client-stops-responding-or-disconnects"></a>El cliente web deja de responder o se desconecta

Intente conectarse con otro explorador o cliente.

### <a name="other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Otros exploradores y clientes también funcionan incorrectamente o no se abren

Si los problemas continúan incluso después de haber cambiado de explorador, es posible que el problema no se deba al explorador, sino a la red. Se recomienda que se ponga en contacto con el soporte técnico de red.

## <a name="web-client-keeps-prompting-for-credentials"></a>El cliente web sigue solicitando credenciales

Si el cliente web sigue solicitando las credenciales, siga estas instrucciones:

1. Confirme que la dirección URL del cliente web es correcta.
2. Confirme que las credenciales que está usando son para el entorno de Windows Virtual Desktop asociado a la dirección URL.
3. Borre las cookies del explorador. Para obtener más detalles, consulte [Cómo eliminar archivos de cookies en Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Borre la memoria caché del explorador Para obtener más detalles, consulte [Borrar la memoria caché del explorador](https://binged.it/2RKyfdU).
5. Abra el explorador en modo privado.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Windows Virtual Desktop y las pistas de escalación, consulte [Introducción, comentarios y soporte técnico para solucionar problemas](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un grupo de inquilinos y de hosts en un entorno de Windows Virtual Desktop, consulte [Creación de los grupos de inquilinos y de host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Windows Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas al usar PowerShell con Windows Virtual Desktop, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).