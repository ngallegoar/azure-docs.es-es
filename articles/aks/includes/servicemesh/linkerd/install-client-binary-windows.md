---
author: paulbouwer
ms.topic: include
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: b7d832ba375925d30a976dfde63a776b5d0742bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593749"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>Descarga e instalación del binario de cliente linkerd de Linkerd

En un shell basado en PowerShell en Windows, use `Invoke-WebRequest` para descargar la versión de Linkerd como se indica a continuación:

```powershell
# Specify the Linkerd version that will be leveraged throughout these instructions
$LINKERD_VERSION="stable-2.6.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-windows.exe" -OutFile "linkerd2-cli-$LINKERD_VERSION-windows.exe"
```

El binario de cliente `linkerd` se ejecuta en la máquina cliente y permite interactuar con la malla de servicio de Linkerd. Use los siguientes comandos para instalar el binario de cliente `linkerd` de Linkerd en un shell basado en PowerShell en Windows. Estos comandos copian el binario de cliente `linkerd` en una carpeta de Linkerd y, luego, permiten que estén disponible de inmediato (en el shell actual) y de forma permanente (entre reinicios del shell) mediante `PATH`. No es necesario tener privilegios elevados (administrador) para ejecutar estos comandos y no hace falta reiniciar el shell.

```powershell
# Copy linkerd.exe to C:\Linkerd
New-Item -ItemType Directory -Force -Path "C:\Linkerd"
Copy-Item -Path ".\linkerd2-cli-$LINKERD_VERSION-windows.exe" -Destination "C:\Linkerd\linkerd.exe"

# Add C:\Linkerd to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Linkerd\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```