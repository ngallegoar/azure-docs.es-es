---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: a3340eb9e53afa83c35109bad7d22f81413dd644
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80234222"
---
## <a name="prepare-an-iot-hub"></a>Preparación de un centro de IoT

También necesitará un centro de IoT de Azure en la suscripción de Azure para completar este inicio rápido. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. Si no tiene una instancia de IoT Hub, siga [estas instrucciones para crear una](../articles/iot-hub/iot-hub-create-using-cli.md).

Si utiliza la CLI de Azure de forma local, inicie sesión primero en la suscripción de Azure mediante `az login`. Si está ejecutando estos comandos en Azure Cloud Shell, su sesión se iniciará automáticamente.

Si usa la CLI de Azure localmente, la versión de `az` debe ser la **2.0.73** o posterior. Azure Cloud Shell usa la versión más reciente. Use el comando `az --version` para comprobar la versión instalada en la máquina.

Ejecute el siguiente comando para agregar la extensión de IoT de Microsoft Azure para la CLI de Azure a la instancia:

```azurecli-interactive
az extension add --name azure-iot
```

Ejecute el siguiente comando la nueva identidad del dispositivo en su centro de IoT. Reemplace los marcadores de posición **YourIoTHubName** y **YourDeviceID** por su propio _nombre de IoT Hub_ y un _identificador de dispositivo_ de su elección.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Ejecute el siguiente comando para obtener la _cadena de conexión del dispositivo_ que acaba de registrar (anótela para usarla más adelante):

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```
