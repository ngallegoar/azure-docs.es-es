---
author: baanders
description: 'Inclusión de archivos en Azure Digital Twins: Configuración de Cloud Shell y la extensión de IoT'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 8be070826de0334483f4150925c05cb4dfb73f2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85805432"
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

A continuación, agregará la [**Extensión de Microsoft Azure IoT para la CLI de Azure**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) a la instancia de Cloud Shell, para habilitar los comandos para interactuar con Azure Digital Twins y otros servicios IoT. 

En primer lugar, ejecute este comando para ver una lista de todas las extensiones que ya tenga instaladas.

```azurecli-interactive
az extension list
```

En la salida, busque el campo `"name"` de cada entrada de la lista para ver los nombres de las extensiones.

Use la salida para determinar cuál de los siguientes comandos se ejecutará para la instalación de la extensión (puede ejecutar más de uno).
* Si la lista contiene `azure-iot`: Ya tiene la extensión. Ejecute este comando para asegurarse de que tiene la actualización más reciente:

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

* Si la lista **no** contiene `azure-iot`: Debe instalar la extensión. Use este comando:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

* Si la lista contiene `azure-iot-cli-ext`: Se trata de la versión heredada de la extensión. Solo se debe instalar una versión de la extensión a la vez, por lo que debe desinstalar la heredada. Use este comando:

   ```azurecli-interactive
   az extension remove --name azure-cli-iot-ext
   ```

Ahora ya está listo para trabajar con Azure Digital Twins en Cloud Shell.

Para comprobarlo, puede ejecutar `az dt -h` en cualquier momento para ver una lista de los comandos de nivel superior de Azure Digital Twins que están disponibles.