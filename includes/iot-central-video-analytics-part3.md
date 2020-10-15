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
ms.openlocfilehash: 60a62733a17d1a3dcc4ba80ed7ceb1c37c8ac5d6
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876727"
---
## <a name="create-the-azure-iot-edge-gateway-device"></a>Creación del dispositivo de puerta de enlace IoT Edge de Azure

La aplicación de análisis de vídeo con detección de objetos y movimiento incluye la plantilla de dispositivo **LVA Edge Object Detector** (Detector de objetos LVA Edge) y la plantilla de dispositivo **LVA Edge Motion Detection** (Detección de movimiento LVA Edge). En esta sección, creará una plantilla de dispositivo de puerta de enlace mediante el manifiesto de implementación y agregará el dispositivo de puerta de enlace a la aplicación de IoT Central.

### <a name="create-a-device-template-for-the-lva-edge-gateway"></a>Creación de la plantilla de dispositivo LVA Edge Gateway (Puerta de enlace LVA Edge)

Para importar el manifiesto de implementación y crear la plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge):

1. En la aplicación de IoT Central, vaya a **Device Templates** (Plantillas de dispositivo) y seleccione **+ New** (+ Nueva).

1. En la página **Seleccionar tipo de plantilla**, seleccione el icono **Azure IoT Edge**. Después, seleccione **Next: Customize** (Personalizar)

1. En la página **Upload an Azure IoT Edge deployment manifest** (Cargar un manifiesto de implementación de Azure IoT Edge), escriba *LVA Edge Gateway* (Puerta de enlace LVA Edge) como nombre de la plantilla y active **Gateway device with downstream devices** (Dispositivo de puerta de enlace con dispositivos de bajada).

    No busque aún el manifiesto de implementación. Si lo hace, el asistente para la implementación espera una interfaz para cada módulo, pero solo se necesita exponer la interfaz para **LvaEdgeGatewayModule**. Cargue el manifiesto en un paso posterior.

    :::image type="content" source="./media/iot-central-video-analytics-part3/upload-deployment-manifest.png" alt-text="No cargar el manifiesto de implementación":::

    Seleccione **Siguiente: Review** (Siguiente: revisar).

1. En la página **Revisar**, seleccione **Crear**.

### <a name="import-the-device-capability-model"></a>Importación del modelo de funcionalidad del dispositivo

La plantilla de dispositivo debe incluir un modelo de funcionalidad del dispositivo. En la página **LVA Edge Gateway** (Puerta de enlace LVA Edge), seleccione el icono **Import capability model** (Importar modelo de funcionalidad). Vaya a la carpeta *lva-configuration* que creó anteriormente y seleccione el archivo *LvaEdgeGatewayDcm.json*.

La plantilla de dispositivo **LVA Edge Gateway** (Puerta de enlace LVA Edge) ahora incluye el módulo **LVA Edge Gateway Module** (Módulo de puerta de enlace LVA Edge) junto con tres interfaces: **Device information** (Información del dispositivo), **LVA Edge Gateway Settings** (Configuración de puerta de enlace LVA Edge) y **LVA Edge Gateway Interface** (Interfaz de puerta de enlace LVA Edge).
