---
author: baanders
description: 'Inclusión de archivos en Azure Digital Twins: Configuración de Cloud Shell y la extensión de IoT'
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: c1d1efce656bea02385cb20ac11ab90ca5664e60
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025596"
---
Para empezar a trabajar con Azure Digital Twins en una ventana abierta de [Azure Cloud Shell](https://shell.azure.com), primero es necesario iniciar sesión y establecer el contexto de Shell en su suscripción para esta sesión. En Cloud Shell, ejecute estos comandos:

```azurecli-interactive
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> En el comando anterior, también puede utilizar el nombre de la suscripción en lugar del identificador. 

Si esta es la primera vez que usa esta suscripción con Azure Digital Twins, ejecute este comando para registrarse con el espacio de nombres de Azure Digital Twins. (Si no está seguro, es correcto volver a ejecutarlo aunque lo haya hecho en algún momento del pasado).

```azurecli-interactive
az provider register --namespace 'Microsoft.DigitalTwins'
```

A continuación, agregará la [**Extensión de Microsoft Azure IoT para la CLI de Azure**](/cli/azure/ext/azure-iot/iot?preserve-view=true&view=azure-cli-latest) a la instancia de Cloud Shell, para habilitar los comandos para interactuar con Azure Digital Twins y otros servicios IoT. 

[!INCLUDE [digital-twins-cloud-shell-extensions.md](digital-twins-cloud-shell-extensions.md)]

Ahora ya está listo para trabajar con Azure Digital Twins en Cloud Shell.

Para comprobarlo, puede ejecutar `az dt -h` en cualquier momento para ver una lista de los comandos de nivel superior de Azure Digital Twins que están disponibles.