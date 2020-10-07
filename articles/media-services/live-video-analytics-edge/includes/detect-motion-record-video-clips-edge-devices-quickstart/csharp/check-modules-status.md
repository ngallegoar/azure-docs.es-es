---
ms.openlocfilehash: e46a56742ab8b98c53c1cd05e840e8ad4b8a73da
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "88682235"
---
En el paso [Generación e implementación del manifiesto de implementación de IoT Edge](../../../detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest), en Visual Studio Code, expanda el nodo **Iva-sample-device** en **AZURE IOT HUB** (en la sección situada en la parte inferior izquierda). Debería ver los siguientes módulos implementados:

* El módulo de Live Video Analytics, denominado `lvaEdge`.
* El módulo `rtspsim`, que simula un servidor RTSP, que actúa como el origen de una fuente de vídeo en directo.

  ![Módulos](../../../media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> Si usa su propio dispositivo perimetral en lugar del proporcionado por el script de instalación, vaya al dispositivo perimetral y ejecute los siguientes comandos con **derechos de administrador**, para extraer y almacenar el archivo de vídeo de ejemplo que se usa para este inicio rápido:  

```
mkdir /home/lvaadmin/samples
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```
