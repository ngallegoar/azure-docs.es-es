---
title: Desarrollar contenido para dispositivos móviles mediante los SDK de Azure IoT | Microsoft Docs
description: 'Guía para desarrolladores: aprenda a desarrollar contenido para dispositivos móviles mediante los SDK de Azure IoT Hub.'
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: robinsh
ms.openlocfilehash: 63fade17d10189cbe8e775b2a859902f1c2bb004
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151633"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Desarrollar contenido para dispositivos móviles mediante los SDK de Azure IoT

El contenido que puede encontrar en Internet de las cosas puede hacer referencia a una amplia gama de dispositivos con distintas capacidades: sensores, microcontroladores, dispositivos inteligentes, puertas de enlace industriales e incluso dispositivos móviles.  Un dispositivo móvil puede ser un dispositivo de IoT, donde se envíe telemetría del dispositivo a la nube y que administre la nube.  Asimismo, también puede ser el dispositivo que se encargue de ejecutar una aplicación de servicio de back-end, para administrar otros dispositivos de IoT.  En ambos casos, los [SDK de Azure IoT Hub](./iot-hub-devguide-sdks.md) se pueden usar para desarrollar aplicaciones que funcionen en dispositivos móviles.  

## <a name="develop-for-native-ios-platform"></a>Desarrollar contenido para la plataforma nativa iOS

Los SDK de Azure IoT Hub proporcionan compatibilidad con la plataforma de iOS nativa a través del SDK de C de Azure IoT Hub.  Se puede considerar como un SDK de iOS que se puede incorporar en el proyecto de Swift u Objective C XCode.  Hay dos maneras de utilizar el SDK de C en iOS:

* Usar las bibliotecas de CocoaPod en el proyecto de XCode directamente.  
* Descargar el código fuente para el SDK de C y la compilación para la plataforma de iOS referentes a las [instrucciones de compilación](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) para MacOS.  

El SDK de C de Azure IoT Hub se escribe en C99 para obtener la máxima portabilidad en diferentes plataformas.  El proceso de portabilidad implica la escritura de una fina capa de adopción para los componentes específicos de la plataforma que se pueden encontrar en este enlace para [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx).  Se pueden aprovechar las características del SDK de C en la plataforma de iOS, incluyendo las primitivas de Azure IoT Hub admitidas y las características específicas del SDK como la directiva de reintentos para la confiabilidad de la red.  La interfaz del SDK para iOS también es similar a la interfaz del SDK de C para Azure IoT Hub.  

Estos documentos le indicarán cómo desarrollar una aplicación de dispositivo o de servicio en dispositivo de iOS:

* [Inicio rápido: Enviar telemetría desde un dispositivo a IoT Hub](quickstart-send-telemetry-ios.md)  
* [Envío de mensajes de nube a dispositivo con IoT Hub](iot-hub-ios-swift-c2d.md) 

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Desarrollar bibliotecas de CocoaPod con Azure IoT Hub

Los SDK de Azure IoT Hub liberan un conjunto de bibliotecas de CocoaPod del proyecto Objective-C para desarrollar contenido de iOS.  Para ver la lista más reciente de bibliotecas de CocoaPod, consulte [CocoaPods for Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md) (Bibliotecas de CocoaPod para Microsoft Azure IoT).  Una vez que se incorporen las bibliotecas relevantes en el proyecto de XCode, hay dos maneras de escribir el código relacionado con IoT Hub:

* Función para Objective C: si el proyecto se ha escrito en Objective-C, se pueden llamar directamente las API del SDK de C de Azure IoT Hub.  Si el proyecto está escrito en Swift, puede llamar a `@objc func` antes de crear la función y escribir todas las lógicas relacionadas con Azure IoT Hub mediante el código de C u Objective-C.  Puede encontrar un conjunto de ejemplos que muestran ambos elementos en el [repositorio de ejemplos](https://github.com/Azure-Samples/azure-iot-samples-ios).  

* Ejemplos de Incorporate C: si ha escrito una aplicación de dispositivo de C, puede hacer referencia a esa aplicación directamente en el proyecto de XCode:
    * Agregue el archivo sample.c al proyecto de XCode desde XCode.  
    * Agregue el archivo de encabezado a la dependencia.  Se incluye un archivo de encabezado en el [repositorio de ejemplos](https://github.com/Azure-Samples/azure-iot-samples-ios) como ejemplo. Para obtener más información, visite la página de documentación de Apple referente a [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="develop-for-android-platform"></a>Desarrollo para la plataforma Android
El SDK de Java de Azure IoT Hub es compatible con la plataforma Android.  Para la versión más reciente de la API específica probada, visite la [página de soporte técnico de la plataforma](iot-hub-device-sdk-platform-support.md).

Estos documentos indican cómo desarrollar una aplicación de dispositivo o de servicio en dispositivo de Android mediante Gradle y Android Studio:

* [Inicio rápido: Enviar telemetría desde un dispositivo a IoT Hub](quickstart-send-telemetry-android.md)  
* [Inicio rápido: Control de un dispositivo](quickstart-control-device-android.md) 

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de la API REST de IoT Hub](/rest/api/iothub/)
* [Código fuente del SDK de Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c)