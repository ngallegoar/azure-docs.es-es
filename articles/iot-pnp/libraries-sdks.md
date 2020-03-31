---
title: SDK y bibliotecas de IoT Plug and Play
description: Información sobre las bibliotecas de dispositivos y servicios disponibles para el desarrollo de soluciones compatibles con IoT Plug and Play.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 394edc24cfeefb96cb2f412985cb990a2306bb09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064326"
---
# <a name="iot-plug-and-play-libraries-and-sdks"></a>SDK y bibliotecas de IoT Plug and Play

Los SDK y las bibliotecas de IoT Plug and Play permiten a los desarrolladores crear soluciones de IoT con diversos lenguajes de programación en varias plataformas. En la tabla siguiente se incluyen vínculos a ejemplos y guías de inicio rápido para ayudarle a empezar:

## <a name="microsoft-supported-libraries-and-sdks"></a>Bibliotecas y SDK compatibles con Microsoft

| Plataforma | Biblioteca/Paquete | Código fuente | Muestra | Guía de inicio rápido | Referencia |
| -------- | ------- | ----------- | ------ | ---------- | --------- |
| C/Linux  | [SDK de dispositivos en apt-get](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#aptgetpackage) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Ejemplos del cliente de Digital Twin](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [Conexión a IoT Hub](./quickstart-connect-pnp-device-c-linux.md) | [Referencia](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Windows  | [SDK de dispositivos en Vcpkg](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/doc/setting_up_vcpkg.md#setup-c-sdk-vcpkg-for-windows-development-environment) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Ejemplos del cliente de Digital Twin](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [Conexión a IoT Hub](./quickstart-connect-pnp-device-c-windows.md) | [Referencia](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/mbed  | [SDK de dispositivos en EMBED](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#mbed) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Ejemplos del cliente de Digital Twin](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Referencia](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Arduino  | [SDK de dispositivos en el IDE de Arduino](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#arduino) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Ejemplos del cliente de Digital Twin](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Referencia](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/iOS  | [SDK de dispositivos en CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Ejemplos del cliente de Digital Twin](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Referencia](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C#    | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.DigitalTwin.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [Ejemplos de Digital Twins](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/digitaltwin/Samples) | [Conexión a IoT Hub](./quickstart-connect-pnp-device-csharp.md) | [Referencia](https://docs.microsoft.com/dotnet/api/overview/azure/iot/client?view=azure-dotnet) |
| Java   | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/digital-twin-device-client-preview/1.0.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Ejemplos de Digital Twins](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/digital-twin/Samples) | [Conexión a IoT Hub](./quickstart-connect-pnp-device-java.md) | [Referencia](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device) |
| Node.js | [NPM](https://www.npmjs.com/package/azure-iot-digitaltwins-device) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview) | [Ejemplos de Digital Twins](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview/digitaltwins/samples) | [Conexión a IoT Hub](./quickstart-connect-pnp-device-node.md) | [Referencia](https://docs.microsoft.com/javascript/api/azure-iot-device/) |

## <a name="iot-hub-support"></a>Compatibilidad con IoT Hub

Las funcionalidades de dispositivo de IoT Plug and Play solo se admiten en [las instancias de nivel estándar y gratis de IoT Hub](../iot-hub/iot-hub-scaling.md).

## <a name="next-steps"></a>Pasos siguientes

Además de las bibliotecas y los SDK de dispositivos, puede usar las API REST para interactuar con los repositorios de modelos.