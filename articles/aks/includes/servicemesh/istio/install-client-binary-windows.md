---
author: paulbouwer
ms.topic: include
ms.date: 10/02/2020
ms.author: pabouwer
ms.openlocfilehash: a02c17013a205ccc0da85536b491d467ef72fa48
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91666737"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Descargar e instalar el binario de cliente istioctl de Istio

En un shell basado en PowerShell en Windows, use `Invoke-WebRequest` para descargar la versión de Istio y, a continuación, realice la extracción con `Expand-Archive`, como se indica a continuación:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.7.3"

[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istioctl-$ISTIO_VERSION-win.zip" -OutFile "istioctl-$ISTIO_VERSION.zip"
Expand-Archive -Path "istioctl-$ISTIO_VERSION.zip" -DestinationPath .
```

El archivo binario cliente `istioctl` se ejecuta en el equipo cliente y permite instalar y administrar istio en el clúster de AKS. Use los siguientes comandos para instalar el binario de cliente `istioctl` de Istio en un shell basado en PowerShell en Windows. Estos comandos copian los archivos binarios del cliente `istioctl` en una carpeta de Istio y, luego, permiten que estén disponible de inmediato (en el shell actual) y de forma permanente (entre reinicios del shell) mediante `PATH`. No es necesario tener privilegios elevados (administrador) para ejecutar estos comandos y no hace falta reiniciar el shell.

```powershell
# Copy istioctl.exe to C:\Istio
New-Item -ItemType Directory -Force -Path "C:\Istio"
Move-Item -Path .\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH", "User") + ";C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\Istio\"
```