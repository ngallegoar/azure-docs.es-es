---
title: 'Tutorial: Creación de una aplicación de análisis de vídeo con detección de objetos y movimiento en Azure IoT Central (OpenVINO)'
description: En este tutorial se muestra cómo crear una aplicación de análisis de vídeo en IoT Central. Se crea, se personaliza y se conecta a otros servicios de Azure. En este tutorial se usa el kit de herramientas de Intel OpenVINO&trade; para la detección de objetos en tiempo real.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 10/06/2020
ms.openlocfilehash: af967c58cdeb2c750178141193a711a66af7477c
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94426748"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central-openvinotrade"></a>Tutorial: Creación de una aplicación de análisis de vídeo con detección de objetos y movimiento en Azure IoT Central (OpenVINO&trade;)

Como creador de soluciones, aprenda a crear una aplicación de análisis de vídeo con la plantilla de aplicación *Video analytics - object and motion detection* (Análisis de vídeo: detección de objetos y movimiento) de IoT Central, dispositivos Azure IoT Edge, Azure Media Services y el sistema de hardware optimizado para la detección de objetos y movimiento OpenVINO&trade; de Intel. La solución usa un comercio minorista para mostrar cómo cumplir con la necesidad empresarial común de supervisar las cámaras de seguridad. La solución utiliza la detección automática de objetos de una fuente de vídeo para identificar y localizar rápidamente eventos de interés.

> [!TIP]
> Para usar YOLO v3 en lugar de OpenVINO&trade; para la detección de objetos y movimiento, consulte [Tutorial: Creación de una aplicación de análisis de vídeo con detección de objetos y movimiento en Azure IoT Central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md).

[!INCLUDE [iot-central-video-analytics-part1](../../../includes/iot-central-video-analytics-part1.md)]

- [Scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt)
- [deployment.openvino.amd64.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deployment.openvino.amd64.json)
- [LvaEdgeGatewayDcm.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [state.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json)

[!INCLUDE [iot-central-video-analytics-part2](../../../includes/iot-central-video-analytics-part2.md)]

## <a name="edit-the-deployment-manifest"></a>Edición del manifiesto de implementación

Un módulo IoT Edge se implementa con un manifiesto de implementación. En IoT Central, puede importar el manifiesto como una plantilla de dispositivo y, a continuación, dejar que IoT Central administre la implementación del módulo.

Para preparar el manifiesto de implementación:

1. Abra el archivo *deployment.openvino.amd64.json*, que guardó en la carpeta *lva-configuration*, con un editor de texto.

1. Busque la configuración `LvaEdgeGatewayModule` y cambie el nombre de la imagen como se muestra en el siguiente fragmento de código:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Agregue el nombre de la cuenta de Media Services en el nodo `env` de la sección `LvaEdgeGatewayModule`. Anotó este nombre de cuenta en el archivo *scratchpad.txt*:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_ACCOUNT_NAME>"
        }
    }
    ```

1. La plantilla no expone estas propiedades en IoT Central, por lo que debe agregar los valores de configuración de Media Services al manifiesto de implementación. Busque el módulo `lvaEdge` y reemplace los marcadores de posición por los valores que anotó en el archivo *scratchpad.txt* cuando creó la cuenta de Media Services.

    El elemento `azureMediaServicesArmId` es el **identificador de recurso** que anotó en el archivo *scratchpad.txt* cuando creó la cuenta de Media Services.

    Anotó los valores de `aadTenantId`, `aadServicePrincipalAppId` y `aadServicePrincipalSecret` en el archivo *scratchpad.txt* cuando creó la entidad de servicio para la cuenta de Media Services:

    ```json
    {
        "lvaEdge":{
        "properties.desired": {
            "applicationDataDirectory": "/var/lib/azuremediaservices",
            "azureMediaServicesArmId": "[Resource ID from scratchpad]",
            "aadTenantId": "[AADTenantID from scratchpad]",
            "aadServicePrincipalAppId": "[AadClientId from scratchpad]",
            "aadServicePrincipalSecret": "[AadSecret from scratchpad]",
            "aadEndpoint": "https://login.microsoftonline.com",
            "aadResourceId": "https://management.core.windows.net/",
            "armEndpoint": "https://management.azure.com/",
            "diagnosticsEventsOutputName": "AmsDiagnostics",
            "operationalMetricsOutputName": "AmsOperational",
            "logCategories": "Application,Event",
            "AllowUnsecuredEndpoints": "true",
            "TelemetryOptOut": false
            }
        }
    }
    ```

1. Guarde los cambios.

En este tutorial se configura la solución para usar el módulo OpenVINO&trade; para la detección de objetos y movimiento. El siguiente fragmento de código muestra la configuración del módulo:

```json
"OpenVINOModelServerEdgeAIExtensionModule": {
    "settings": {
        "image": "marketplace.azurecr.io/intel_corporation/open_vino",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"4000/tcp\":[{\"HostPort\":\"4000\"}]}},\"Cmd\":[\"/ams_wrapper/start_ams.py\",\"--ams_port=4000\",\"--ovms_port=9000\"]}"
    },
    "type": "docker",
    "version": "1.0",
    "status": "running",
    "restartPolicy": "always"
}
```

[!INCLUDE [iot-central-video-analytics-part3](../../../includes/iot-central-video-analytics-part3.md)]

### <a name="replace-the-manifest"></a>Reemplazo del manifiesto

En la página **LVA Edge Gateway v2** (Puerta de enlace LVA Edge v2), seleccione **+ Replace manifest** (+ Reemplazar manifiesto).

:::image type="content" source="./media/tutorial-video-analytics-create-app-openvino/replace-manifest.png" alt-text="Reemplazar manifiesto":::

Vaya a la carpeta *lva-configuration* y seleccione el archivo de manifiesto *deployment.openvino.amd64.json* que editó previamente. Seleccione **Cargar**. Una vez completada la validación, seleccione **Reemplazar**.

[!INCLUDE [iot-central-video-analytics-part4](../../../includes/iot-central-video-analytics-part4.md)]
