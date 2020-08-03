---
author: baanders
description: 'Inclusión de archivos en Azure Digital Twins: Configuración de Cloud Shell y la extensión de IoT'
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: b7c91d648c06970d53799c6ff505919dea17b3c0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87032242"
---
Para empezar a trabajar con Azure Digital Twins en una ventana abierta de [Azure Cloud Shell](https://shell.azure.com), primero es necesario iniciar sesión y establecer el contexto de Shell en su suscripción para esta sesión. En Cloud Shell, ejecute estos comandos:

```azurecli
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> En el comando anterior, también puede utilizar el nombre de la suscripción en lugar del identificador. 

Si esta es la primera vez que usa esta suscripción con Azure Digital Twins, ejecute este comando para registrarse con el espacio de nombres de Azure Digital Twins. (Si no está seguro, es correcto volver a ejecutarlo aunque lo haya hecho en algún momento del pasado).

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

A continuación, agregará la [**Extensión de Microsoft Azure IoT para la CLI de Azure**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) a la instancia de Cloud Shell, para habilitar los comandos para interactuar con Azure Digital Twins y otros servicios IoT. 

En primer lugar, ejecute este comando para ver una lista de todas las extensiones que ya tenga instaladas.

```azurecli-interactive
az extension list
```

La salida es una matriz de todas las extensiones que tiene actualmente. Busque el campo `"name"` para cada entrada de la lista con el fin de ver los nombres de las extensiones.

Use la salida para determinar cuál de los siguientes comandos se ejecutará para la instalación de la extensión (puede ejecutar más de uno).
* Si la lista contiene `azure-iot`: Ya tiene la extensión. Ejecute este comando para asegurarse de que tiene la actualización más reciente y de que no hay otras actualizaciones disponibles:

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