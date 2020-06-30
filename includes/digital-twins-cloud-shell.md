---
author: baanders
description: 'Inclusión de archivos en Azure Digital Twins: Configuración de Cloud Shell y la extensión de IoT'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 4aa016294f0ef3bd26f7f3ef6fa374e9367b672d
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85296974"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Configuración de una sesión de Cloud Shell

Después de abrir una ventana de Cloud Shell, lo primero que hay que hacer es iniciar sesión y establecer el contexto de Shell en la suscripción para esta sesión. En Cloud Shell, ejecute estos comandos:

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Si esta es la primera vez que usa esta suscripción con Azure Digital Twins, ejecute este comando para registrarse con el espacio de nombres de Azure Digital Twins. (Si no está seguro, es correcto volver a ejecutarlo aunque lo haya hecho en algún momento del pasado).

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

A continuación, agregará la [**Extensión de Microsoft Azure IoT para la CLI de Azure**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) a la instancia de Cloud Shell, para habilitar los comandos para interactuar con Azure Digital Twins y otros servicios IoT. Use este comando para agregar la extensión:

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

Si ha instalado la extensión en el pasado, la salida podría indicar "La extensión 'azure-iot' ya está instalada". Si ocurre esto, ejecute lo siguiente para asegurarse de que tiene la actualización más reciente: 

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

Ahora ya está listo para trabajar con Azure Digital Twins en Cloud Shell.

Para comprobarlo, puede ejecutar `az dt -h` en cualquier momento para ver una lista de los comandos de nivel superior de Azure Digital Twins que están disponibles.