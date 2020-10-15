---
title: archivo de inclusión
description: archivo de inclusión
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 164f5803b6e9e62447423735e98f6e4c36c73f13
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876728"
---
### <a name="add-relationships"></a>Adición de relaciones

En la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge), en **Modules/LVA Edge Gateway Module** (Módulos/Módulo de puerta de enlace LVA Edge), seleccione **Relationships** (Relaciones). Seleccione **+ Add relationship** (+ Agregar relación) y agregue las dos relaciones siguientes:

|Display Name (Nombre para mostrar)               |Nombre          |Destino |
|-------------------------- |------------- |------ |
|LVA Edge Motion Detector (Detector de movimiento LVA Edge)   |Use el valor predeterminado   |LVA Edge Motion Detector Device (Dispositivo de detección de movimiento LVA Edge) |
|LVA Edge Object Detector (Detector de objetos LVA Edge)   |Use el valor predeterminado   |LVA Edge Object Detector Device (Dispositivo de detección de objetos LVA Edge) |

Después, seleccione **Guardar**.

:::image type="content" source="media/iot-central-video-analytics-part4/relationships.png" alt-text="Agregar relaciones":::

### <a name="add-views"></a>Adición de vistas

La plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge) no incluye definiciones de vistas.

Para agregar una vista a la plantilla de dispositivo:

1. En la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge), vaya a **Views** (Vistas) y seleccione el icono **Visualizing the device** (Visualización del dispositivo).

1. Escriba *LVA Edge Gateway device* (Dispositivo de puerta de enlace LVA Edge) como nombre de la vista.

1. Agregue los siguientes iconos a la vista:

    * Un icono con las propiedades de **Información del dispositivo**: **Device model** (Modelo del dispositivo), **Manufacturer** (Fabricante), **Operating system** (Sistema operativo), **Processor architecture** (Arquitectura del procesador), **Software version** (Versión de software), **Total memory** (Memoria total), and **Total storage** (Almacenamiento total).
    * Un icono de gráfico de líneas con los valores de telemetría **Free Memory** (Memoria libre) y **System Heartbeat** (Latido del sistema).
    * Un icono de historial de eventos con los siguientes eventos: **Create Camera** (Crear cámara), **Delete Camera** (Eliminar cámara), **Module Restart** (Módulo reiniciado), **Module Started** (Módulo iniciado), **Module Stopped** (Módulo detenido).
    * Un icono de último valor conocido de tamaño 2x1 que muestre el dato de telemetría **IoT Central Client State** (Estado del cliente de IoT Central).
    * Un icono de último valor conocido de tamaño 2x1 que muestre el dato de telemetría **Module State** (Estado del módulo).
    * Un icono de último valor conocido de tamaño 1x1 que muestre el dato de telemetría **System Heartbeat** (Latido del sistema).
    * Un icono de último valor conocido de tamaño 1x1 que muestre el dato de telemetría **Connected Cameras** (Cámaras conectadas).

    :::image type="content" source="media/iot-central-video-analytics-part4/gateway-dashboard.png" alt-text="Agregar relaciones":::

1. Seleccione **Guardar**.

### <a name="publish-the-device-template"></a>Publicación de la plantilla de dispositivo

Para poder agregar un dispositivo a la aplicación, debe publicar la plantilla de dispositivo:

1. En la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge), seleccione **Publish** (Publicar).

1. En la página **Publish this device template to the application** (Publicar esta plantilla de dispositivo en la aplicación), seleccione **Publicar** (Publicar).

**LVA Edge Gateway** (Puerta de enlace LVA Edge) ahora está disponible como un tipo de dispositivo que se puede usar en la página **Devices** (Dispositivos) de la aplicación.

## <a name="add-a-gateway-device"></a>Adición de un dispositivo de puerta de enlace

Para agregar un dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge) a la aplicación:

1. Vaya a la página **Devices** (Dispositivos) y seleccione la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge).

1. Seleccione **+ Nuevo**.

1. En el cuadro de diálogo **Create a new device** (Crear un dispositivo), cambie el nombre de dispositivo a *LVA Gateway 001* y cambie el identificador de dispositivo a *lva-gateway-001*.

    > [!NOTE]
    > El identificador de dispositivo debe ser único en la aplicación.

1. Seleccione **Crear**.

El estado del dispositivo es **Registered** (Registrado).

### <a name="get-the-device-credentials"></a>Obtención de las credenciales del dispositivo

Necesita las credenciales que permiten que el dispositivo se conecte a la aplicación de IoT Central. Para obtener las credenciales del dispositivo:

1. En la página **Devices** (Dispositivos), seleccione el dispositivo **lva-gateway-001** que acaba de crear.

1. Seleccione **Conectar**.

1. En la página **Device connection** (Conexión del dispositivo), anote en el archivo *scratchpad.txt* los valores de **ID Scope** (Ámbito de id.), **Device ID** (Id. de dispositivo) y **Primary Key** (Clave principal) del dispositivo. Utilizará estos valores más adelante.

1. Asegúrese de que el método de conexión se establece en **Shared access signature** (Firma de acceso compartido).

1. Seleccione **Cerrar**.

## <a name="next-steps"></a>Pasos siguientes

Ahora ha creado una aplicación de IoT Central con la plantilla de aplicación **Video analytics - object and motion detection** (Análisis de vídeo: detección de objetos y movimiento), ha creado una plantilla de dispositivo para el dispositivo de puerta de enlace y ha agregado un dispositivo de puerta de enlace a la aplicación.

Si desea probar la aplicación de análisis de vídeo con detección de objetos y movimiento con módulos IoT Edge que ejecutan una máquina virtual en la nube con secuencias de vídeo simuladas:

> [!div class="nextstepaction"]
> [Creación de una instancia de IoT Edge para análisis de vídeo (máquina virtual Linux)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-vm.md)

Si desea probar la aplicación de análisis de vídeo con detección de objetos y movimiento con módulos IoT Edge que ejecutan un dispositivo real con una cámara **ONVIF** real:

> [!div class="nextstepaction"]
> [Creación de una instancia de IoT Edge para análisis de vídeo (Intel NUC)](../articles/iot-central/retail/tutorial-video-analytics-iot-edge-nuc.md)
