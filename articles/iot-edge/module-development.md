---
title: Desarrollo de módulos para Azure IoT Edge | Microsoft Docs
description: Desarrollo de módulos personalizados para Azure IoT Edge que pueden comunicarse con el tiempo de ejecución y IoT Hub
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: eae36f6b4baabdcc9831b084602d340a299a7bac
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047631"
---
# <a name="develop-your-own-iot-edge-modules"></a>Desarrollar sus propios módulos de IoT Edge

Los módulos de Azure IoT Edge pueden conectarse con otros servicios de Azure y contribuir a la mayor canalización de datos en la nube. En este artículo se describe cómo puede desarrollar módulos para comunicarse con el entorno de ejecución de IoT Edge e IoT Hub y, por lo tanto, el resto de la nube de Azure.

## <a name="iot-edge-runtime-environment"></a>Entorno de tiempo de ejecución de IoT Edge

El entorno de tiempo de ejecución de IoT Edge proporciona la infraestructura para integrar la funcionalidad de varios módulos de IoT Edge e implementarlos en dispositivos IoT Edge. Todos los programas se pueden empaquetar como un módulo de IoT Edge. Para aprovechar al máximo las funcionalidades de comunicación y administración de IoT Edge, un programa que se ejecute en un módulo puede usar el SDK de dispositivos IoT de Azure para conectarse al centro de IoT Edge local.

## <a name="using-the-iot-edge-hub"></a>Uso del centro de IoT Edge

El centro de IoT Edge proporciona dos funcionalidades principales: proxy a IoT Hub y comunicaciones locales.

### <a name="iot-hub-primitives"></a>Primitivos de IoT Hub

IoT Hub considera una instancia de módulo de forma análoga a un dispositivo, en el sentido de que:

* tiene un módulo gemelo, que es distinto y está aislado del [dispositivo gemelo](../iot-hub/iot-hub-devguide-device-twins.md) y de los otros módulos gemelos de ese dispositivo;
* puede enviar [mensajes del dispositivo a la nube](../iot-hub/iot-hub-devguide-messaging.md);
* puede recibir [métodos directos](../iot-hub/iot-hub-devguide-direct-methods.md) dirigidos específicamente a su identidad.

Actualmente, los módulos no pueden recibir mensajes de nube a dispositivo ni usar la característica de carga de archivos.

Al escribir un módulo, puede usar el [SDK de dispositivos IoT de Azure](../iot-hub/iot-hub-devguide-sdks.md) para conectarse al centro de IoT Edge y utilizar la funcionalidad anterior como lo haría al emplear IoT Hub con una aplicación de dispositivo. La única diferencia entre los módulos de IoT Edge y las aplicaciones de dispositivos IoT es que tiene que hacer referencia a la identidad de módulo en lugar de a la identidad del dispositivo.

### <a name="device-to-cloud-messages"></a>Mensajes de dispositivo a nube

Para permitir el procesamiento complejo de mensajes de dispositivo a nube, el centro de IoT Edge proporciona enrutamiento declarativo de mensajes entre los módulos, y entre los módulos e IoT Hub. Este enrutamiento declarativo permite a los módulos interceptar y procesar los mensajes enviados por otros módulos y propagarlos en canalizaciones complejas. Para obtener más información, vea [Implementar módulos y establecer rutas en IoT Edge](module-composition.md).

Un módulo de IoT Edge, a diferencia de una aplicación normal de dispositivos de IoT Hub, puede recibir mensajes de dispositivo a nube que se redirigen mediante proxy a través de su centro de IoT Edge local, con el fin de procesarlos.

El centro de IoT Edge propaga los mensajes al módulo en función de las rutas declarativas que se describen en el artículo [Manifiesto de implementación](module-composition.md). Al desarrollar un módulo IoT Edge, puede recibir estos mensajes mediante el establecimiento de los controladores de mensajes.

Para simplificar la creación de rutas, IoT Edge agrega el concepto de puntos de conexión de *entrada* y *salida* del módulo. Un módulo puede recibir todos los mensajes de dispositivo a nube enrutados a él sin especificar ninguna entrada y puede enviar mensajes de dispositivo a nube sin especificar ninguna salida. Sin embargo, el uso explícito de entradas y salidas facilita la comprensión de las reglas de enrutamiento.

Por último, los mensajes de dispositivo a nube que se gestionan mediante el centro de Edge están marcados con las siguientes propiedades del sistema:

| Propiedad | Descripción |
| -------- | ----------- |
| $connectionDeviceId | El identificador de dispositivo del cliente que envió el mensaje. |
| $connectionModuleId | El identificador de módulo del módulo que envió el mensaje. |
| $inputName | La entrada que recibió este mensaje. Puede estar vacía. |
| $outputName | La salida usada para enviar el mensaje. Puede estar vacía. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Conexión al centro de IoT Edge desde un módulo

La conexión al centro de IoT Edge local desde un módulo implica dos pasos:

1. Crear una instancia de ModuleClient en la aplicación.
2. Asegúrese de que la aplicación acepta el certificado presentado por el centro de IoT Edge en ese dispositivo.

Crear una instancia de ModuleClient para conectar el módulo al centro de IoT Edge que se ejecuta en el dispositivo, de forma similar a cómo las instancias de DeviceClient conectan dispositivos IoT a IoT Hub. Para más información sobre la clase ModuleClient y sus métodos de comunicación, consulte la referencia de API de su lenguaje preferido para el SDK: [C#](/dotnet/api/microsoft.azure.devices.client.moduleclient), [C](/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python](/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient), [Java](/java/api/com.microsoft.azure.sdk.iot.device.moduleclient) o [Node.js](/javascript/api/azure-iot-device/moduleclient).

## <a name="language-and-architecture-support"></a>Compatibilidad de idiomas y arquitecturas

IoT Edge admite varios sistemas operativos, arquitecturas de dispositivos y lenguajes de desarrollo para que pueda crear el escenario que más se ajuste a sus necesidades. Use esta sección para conocer las distintas opciones para desarrollar módulos de IoT Edge personalizados. Puede obtener más información acerca la compatibilidad de las herramientas y de los requisitos de cada lenguaje en [Preparación del entorno de desarrollo y prueba para IoT Edge](development-environment.md).

### <a name="linux"></a>Linux

En todos los idiomas de la tabla siguiente, IoT Edge admite el desarrollo para dispositivos Linux AMD64 y ARM32.

| Lenguaje de desarrollo | Herramientas de desarrollo |
| -------------------- | ----------------- |
| C | Visual Studio Code<br>Visual Studio 2017/2019 |
| C# | Visual Studio Code<br>Visual Studio 2017/2019 |
| Java | Visual Studio Code |
| Node.js | Visual Studio Code |
| Python | Visual Studio Code |

>[!NOTE]
>El soporte técnico del desarrollo y depuración para dispositivos Linux ARM64 está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Para más información, consulte [Desarrollo y depuración de módulos ARM64 IoT Edge en Visual Studio Code (versión preliminar)](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview).

### <a name="windows"></a>Windows

En todos los idiomas de la tabla siguiente, IoT Edge admite el desarrollo para dispositivos Windows AMD64.

| Lenguaje de desarrollo | Herramientas de desarrollo |
| -------------------- | ----------------- |
| C | Visual Studio 2017/2019 |
| C# | Visual Studio Code (sin funcionalidades de depuración)<br>Visual Studio 2017/2019 |

## <a name="next-steps"></a>Pasos siguientes

[Preparación del entorno de desarrollo y prueba para IoT Edge](development-environment.md)

[Uso de Visual Studio para desarrollar módulos de C# para IoT Edge](how-to-visual-studio-develop-module.md)

[Uso de Visual Studio Code para desarrollar módulos para IoT Edge](how-to-vs-code-develop-module.md)

[Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md)