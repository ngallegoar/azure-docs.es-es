---
title: Versión actual de IoT Plug and Play | Microsoft Docs
description: Obtenga información sobre lo que se incluye en la versión actual de IoT Plug and Play.
author: dominicbetts
ms.author: dobett
ms.date: 10/01/2020
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: c7ebcdac3174f7eb497ae3e976386ab92212e1ba
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91715567"
---
# <a name="whats-in-the-current-iot-plug-and-play-release"></a>Qué hay en la versión actual de IoT Plug and Play

En este artículo se resumen las herramientas, los SDK y las API que admiten la versión actual de IoT Plug and Play. Los números de versión que se muestran reflejan el número de versión en el momento en que IoT Plug and Play está disponible con carácter general. Los números de versión pueden incrementarse después del lanzamiento.

## <a name="modeling-language"></a>Lenguaje de modelado

[Lenguaje de definición de Digital Twins (DTDL) V2](https://github.com/Azure/opendigitaltwins-dtdl).

Para obtener más información sobre cómo funcionan los dispositivos de IoT Plug and Play con DTDL, consulte [Convenciones de IoT Plug and Play](concepts-convention.md).

## <a name="tools-and-utilities"></a>Herramientas y utilidades

- Azure IoT Explorer 0.12.0.

    Para obtener más información, consulte [Instalación y uso del explorador de Azure IoT](howto-use-iot-explorer.md).

- Extensión de VS Code 1.0.0.

    Para obtener más información, consulte [Instalación y uso de las herramientas de creación de DTDL](howto-use-dtdl-authoring-tools.md).

- Extensión 1.0.0 de Visual Studio 2019.

    Para obtener más información, consulte [Instalación y uso de las herramientas de creación de DTDL](howto-use-dtdl-authoring-tools.md).

- Extensión IoT 0.10.0 de la CLI de Azure.

    Para obtener más información, consulte [Instalación y uso de la extensión de Azure IoT para la CLI de Azure](howto-use-iot-pnp-cli.md).

    > [!TIP]
    > La extensión de Azure IoT incluye comandos que le ayudarán a certificar dispositivos. Vea `az iot product -h`.

## <a name="libraries-and-sdks"></a>Bibliotecas y SDK

Para obtener más información sobre las bibliotecas y los SDK, consulte [SDK de Microsoft para IoT Plug and Play](libraries-sdks.md).

- SDK de dispositivos de C [vcpkg 1.3.9](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/setting_up_vcpkg.md)
- SDK de dispositivos de C insertados [GitHub](https://github.com/Azure/azure-sdk-for-c/)
- SDK de dispositivos de .NET [NuGet 1.31.0](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client)
- SDK de dispositivos de Java [Maven 1.26.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-device-client)
- SDK de dispositivos de Python [pip 2.3.0](https://pypi.org/project/azure-iot-device/)
- SDK de dispositivos de Node.js [npm 1.17.2](https://www.npmjs.com/package/azure-iot-device)
- .NET: servicio IoT Hub [NuGet 1.27.1](https://www.nuget.org/packages/Microsoft.Azure.Devices )
- Java: servicio IoT Hub [Maven 1.26.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-service-client/1.26.0)
- Node.js: servicio IoT Hub [npm 1.13.0](https://www.npmjs.com/package/azure-iothub)
- Python: servicio IoT Hub/Digital Twins [pip 2.2.3](https://pypi.org/project/azure-iot-hub)
- [NuGet](https://www.nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser) del analizador de modelos DTDL.

## <a name="rest-apis"></a>API de REST

API de REST [2020-09-30](https://docs.microsoft.com/rest/api/iothub).

Para obtener más información, consulte la [Guía para desarrolladores de IoT Plug and Play](concepts-developer-guide-service.md).

## <a name="iot-hub"></a>IoT Hub

IoT Plug and Play es compatible con IoT Hub en todas las regiones. IoT Plug and Play solo se admite en centros de IoT estándar o de nivel gratuito.

## <a name="announcements"></a>Anuncios

En el caso de los anuncios de IoT Plug and Play actuales y anteriores, consulte las siguientes entradas de blog:

- [Actualización de la versión preliminar pública (entrada publicada el 29 de agosto de 2020)](https://techcommunity.microsoft.com/t5/internet-of-things/add-quot-plug-and-play-quot-to-your-iot-solutions/ba-p/1548531)
- [Preparación y certificación de los dispositivos de IoT Plug and Play (entrada publicada el 26 de agosto de 2020)](https://azure.microsoft.com/blog/prepare-and-certify-your-devices-for-iot-plug-and-play/)
- [IoT Plug and Play está disponible en versión preliminar (entrada publicada el 22 de agosto de 2019)](https://azure.microsoft.com/blog/iot-plug-and-play-is-now-available-in-preview/)
- [Compilación con Azure IoT Central e IoT Plug and Play (entrada publicada el 7 de mayo de 2019)](https://azure.microsoft.com/blog/build-with-azure-iot-central-and-iot-plug-and-play/)
