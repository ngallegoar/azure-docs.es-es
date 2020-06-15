---
author: baanders
description: 'Inclusión de archivos en Azure Digital Twins: Configuración de Cloud Shell y la extensión de IoT'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 76480959d94b9e2a2e04857c85b7359105e73e4f
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84611478"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Configuración de una sesión de Cloud Shell

Después de abrir una ventana de Cloud Shell, lo primero que hay que hacer es iniciar sesión y establecer el contexto de Shell en la suscripción para esta sesión. 

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Si esta es la primera vez que usa esta suscripción con Azure Digital Twins, ejecute este comando para registrarse con el espacio de nombres de Azure Digital Twins. (Si no está seguro, es correcto volver a ejecutarlo aunque lo haya hecho en algún momento del pasado).

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Ejecute el siguiente comando en la instancia de Cloud Shell para agregar la extensión de IoT de Microsoft Azure para la CLI de Azure.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

> [!NOTE]
> En este artículo se usa la versión más reciente de la extensión de Azure IoT, denominada `azure-iot`. La versión heredada se denomina `azure-iot-cli-ext`. Solo debe tener instalada una versión a la vez. Puede usar el comando `az extension list` para validar las extensiones instaladas actualmente.
> Use `az extension remove --name azure-cli-iot-ext` para eliminar la versión heredada de la extensión.
> Use `az extension add --name azure-iot` para agregar la nueva versión de la extensión. Para ver las extensiones que ha instalado, use `az extension list`.

> [!TIP]
> Puede ejecutar `az dt -h` para ver los comandos de nivel superior de Azure Digital Twins.