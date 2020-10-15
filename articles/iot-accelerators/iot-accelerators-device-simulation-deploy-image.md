---
title: 'Implementación de una imagen personalizada de simulación de dispositivo: Azure | Microsoft Docs'
description: En esta guía, obtendrá información sobre cómo implementar una imagen de Docker personalizada de la solución de simulación de dispositivo en Azure.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: c1f321f452b65016c11cb66d08ebab108509cc62
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "61448414"
---
# <a name="deploy-a-custom-device-simulation-docker-image"></a>Implementación de una imagen de Docker personalizada de simulación de dispositivo

Puede modificar la solución de simulación de dispositivo para agregar características personalizadas. Por ejemplo, el artículo [Serialize telemetry using Protocol Buffers](iot-accelerators-device-simulation-protobuf.md) (Serializar telemetría con búferes de protocolo) muestra cómo agregar un dispositivo personalizado a la solución que utiliza búferes de protocolo (Protobuf) para enviar telemetría. Después de probar localmente los cambios, el siguiente paso es implementarlos en la instancia de simulación de dispositivo en Azure. Para completar esta tarea, debe crear e implementar una imagen de Docker que contenga el servicio modificado.

Los pasos de esta guía le muestran cómo:

1. Preparar un entorno de desarrollo
1. Generar una imagen de Docker nueva
1. Configurar la simulación de dispositivo para usar la nueva imagen de Docker
1. Ejecutar una simulación con la nueva imagen

## <a name="prerequisites"></a>Prerequisites

Necesitará lo siguiente para completar los pasos de esta guía:

* Una instancia implementada de [simulación de dispositivo](quickstart-device-simulation-deploy.md).
* Docker. Descargue [Docker Community Edition](https://www.docker.com/products/docker-engine#/download) para su plataforma.
* Una [cuenta de Docker Hub](https://hub.docker.com/) donde pueda cargar las imágenes de Docker. En la cuenta de Docker Hub, cree un repositorio público denominado **devide-simulation**.
* Una [solución de simulación de dispositivo](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) modificada y probada en el equipo local. Por ejemplo, puede modificar la solución para [serializar telemetría con búferes de protocolo](iot-accelerators-device-simulation-protobuf.md).
* Un shell que pueda ejecutar SSH. Si instala Git para Windows, puede usar el shell **bash** que forma parte de la instalación de th. También puede usar [Azure Cloud Shell](https://shell.azure.com/).

En las instrucciones de este artículo se asume que está usando Windows. Si usa otro sistema operativo, deberá ajustar algunas de las rutas de acceso de archivos y los comandos, para que se adapten a su entorno.

## <a name="create-a-new-docker-image"></a>Crear una imagen de Docker nueva

Para implementar sus propios cambios en el servicio de simulación de dispositivo, debe editar los scripts de compilación e implementación de la carpeta **scripts\docker** para cargar los contenedores en la cuenta de Docker Hub

### <a name="modify-the-docker-scripts"></a>Modificar los scripts de Docker

Modifique los scripts **build.cmd**, **publish.cmd** y **run.cmd** de Docker en la carpeta **scripts\docker** con la información del repositorio de Docker Hub. En estos pasos se supone que creó en un repositorio público denominado **device-simulation**:

`DOCKER_IMAGE={your-docker-hub-username}/device-simulation`

Actualice el archivo **docker-compose.yml** de la manera siguiente:

`image: {your-docker-hub-username}/device-simulation`

### <a name="configure-the-solution-to-include-any-new-files"></a>Configurar la solución para que incluya los archivos nuevos

Si ha agregado nuevos archivos de modelo de dispositivo, debe incluirlos explícitamente en la solución. Agregue una entrada a **services/services.csproj** para cada archivo adicional que vaya a incluir. Por ejemplo, si ha completado las instrucciones de [Serialize telemetry using Protocol Buffers](iot-accelerators-device-simulation-protobuf.md) (Serializar telemetría con búferes de protocolo), agregue las siguientes entradas:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

### <a name="generate-new-docker-images-and-push-to-docker-hub"></a>Generar nuevas imágenes de Docker e insertarlas en Docker Hub

Publique la nueva imagen de Docker en Docker Hub con la cuenta de Docker Hub:

1. Abra un símbolo del sistema y navegue hasta la copia local del repositorio de simulación de dispositivo.

1. Navegue hasta la carpeta **docker**:

    ```cmd
    cd scripts\docker
    ```

1. Ejecute el siguiente comando para compilar la imagen de Docker:

    ```cmd
    build.cmd
    ```

1. Ejecute el siguiente comando para publicar la imagen de Docker en el repositorio de Docker Hub. Inicie sesión en Docker con sus credenciales de Docker Hub:

    ```cmd
    docker login
    publish.cmd
    ```

<!-- TODO fix heading levels working include -->

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="update-the-service"></a>Actualizar el servicio

Para actualizar el contenedor de simulación de dispositivo a fin de que use la imagen personalizada, complete los pasos siguientes:

* Use SSH para conectarse a la máquina virtual que hospeda la instancia de simulación de dispositivo. Use la dirección IP y la contraseña que anotó en la sección anterior:

    ```sh
    ssh azureuser@{your vm ip address}
    ```

* Navegue hasta el directorio **/app**:

    ```sh
    cd /app
    ```

* Edite el archivo **docker-compose.yml**:

    ```sh
    sudo nano docker-compose.yml
    ```

    Modifique la **imagen** para que apunte a la imagen personalizada **device-simulation** que cargó en el repositorio de Docker Hub:

    ```yml
    image: {your-docker-hub-username}/device-simulation
    ```

    Guarde los cambios.

* Utilice el siguiente comando para reiniciar los microservicios:

    ```sh
    sudo start.sh
    ```

## <a name="run-your-simulation"></a>Ejecutar la simulación

Ahora puede ejecutar una simulación usando la solución personalizada de simulación de dispositivo:

1. Inicie la interfaz de usuario web de Simulación de dispositivo desde los [aceleradores de soluciones de Microsoft Azure IoT](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Use la interfaz de usuario web para configurar y ejecutar una simulación. Si ya ha completado previamente las instrucciones de [Serialize telemetry using Protocol Buffers](iot-accelerators-device-simulation-protobuf.md) (Serializar telemetría mediante búferes de protocolo), puede usar el modelo de dispositivo personalizado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a implementar una imagen personalizada de simulación de dispositivo, puede que desee información sobre el [Uso de una instancia de IoT Hub existente con el acelerador de soluciones Simulación de dispositivo](iot-accelerators-device-simulation-choose-hub.md).