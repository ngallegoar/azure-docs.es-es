---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: d88d166e001e71cabdabc3a3d344adc4da19aa8c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "75453689"
---
## <a name="clean-up-resources"></a>Limpieza de recursos

Si planea leer artículos de IoT Plug and Play adicionales, puede conservar los recursos que usó en este inicio rápido y reutilizarlos. De lo contrario, puede eliminar los recursos que creó para este inicio rápido para evitar cargos adicionales.

Puede eliminar el centro y el dispositivo registrado a la vez si elimina todo el grupo de recursos con el siguiente comando de la CLI de Azure. (No obstante, no lo use si estos recursos comparten un grupo de recursos con otros recursos que tenga para otros fines).

```azurecli-interactive
az group delete --name <YourResourceGroupName>
```
Para eliminar solo la instancia de IoT Hub, ejecute el siguiente comando con la CLI de Azure:

```azurecli-interactive
az iot hub delete --name <YourIoTHubName>
```

Para eliminar solo la identidad del dispositivo que registró en IoT Hub, ejecute el comando siguiente mediante la CLI de Azure:

```azurecli-interactive
az iot hub device-identity delete --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

También puede que quiera quitar los archivos de ejemplo clonados de la máquina de desarrollo.