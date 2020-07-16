---
author: paulbouwer
ms.topic: include
ms.date: 11/15/2019
ms.author: pabouwer
ms.openlocfilehash: 34a393e06d91e5f60e622667a47e833e910e7228
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86244178"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>Descargar e instalar el binario de cliente istioctl de Istio

En un shell basado en Bash en Linux o en el [Subsistema de Windows para Linux][install-wsl], use `curl` para descargar la versión de Istio y, a continuación, realice la extracción con `tar`, tal como se indica a continuación:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.4.0

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

El binario de cliente `istioctl` se ejecuta en su equipo cliente y le permite interactuar con la malla de servicio de Istio. Use los comandos siguientes para instalar el binario de cliente `istioctl` de Istio en un shell basado en Bash en Linux o en el [Subsistema de Windows para Linux][install-wsl]. Estos comandos copian el binario de cliente `istioctl` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Si desea completar la línea de comandos del binario de cliente `istioctl` de Istio, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

<!-- LINKS - external -->
[install-wsl]: /windows/wsl/install-win10
