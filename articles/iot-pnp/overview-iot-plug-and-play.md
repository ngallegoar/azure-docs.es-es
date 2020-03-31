---
title: Introducción a IoT Plug and Play (versión preliminar) | Microsoft Docs
description: Más información acerca de IoT Plug and Play (versión preliminar). IoT Plug and Play se basa en un lenguaje de modelado abierto que permite a los dispositivos IoT declarar sus funcionalidades. Los dispositivos IoT presentan esa declaración, denominada modelo de funcionalidad del dispositivo, al conectarse a soluciones en la nube como Azure IoT Central o aplicaciones de asociados. Luego, la solución en la nube puede comprender automáticamente el dispositivo y empezar a interactuar con él (y todo ello sin necesidad de escribir código).
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/23/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 0399e1659fb7cc6a650c6b3c1d0189c8802d4904
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80064308"
---
# <a name="what-is-iot-plug-and-play-preview"></a>¿Qué es IoT Plug and Play (versión preliminar)?

IoT Plug and Play (versión preliminar) permite a los desarrolladores de soluciones integrar dispositivos en sus soluciones sin necesidad de escribir código para insertar. En el núcleo de IoT Plug and Play se encuentra un esquema de _modelo de funcionalidad de dispositivo_ que describe las funcionalidades del dispositivo. Este esquema es un documento JSON estructurado como un conjunto de interfaces que incluyen definiciones de:

- _Propiedades_ que representan el estado de solo lectura y de lectura y escritura de un dispositivo, o de otra entidad. Por ejemplo, el número de serie de un dispositivo puede ser una propiedad de solo lectura y la temperatura objetivo de un termostato puede ser una propiedad de lectura y escritura.
- _Datos de telemetría_, que son los datos que emite un dispositivo, independientemente de que sean una secuencia normal de lecturas de un sensor, un error ocasional o un mensaje informativo.
- _Comandos_ que describen una función u operación que se puede realizar en un dispositivo. Por ejemplo, un comando puede reiniciar una puerta de enlace o tomar una imagen mediante una cámara remota.

Las interfaces se pueden reutilizar en los distintos modelos de funcionalidad del dispositivo para facilitar la colaboración y acelerar el desarrollo.

Para que IoT Plug and Play funcione sin problemas con [Azure Digital Twins](../digital-twins/about-digital-twins.md), el esquema de IoT Plug and Play se define mediante el [lenguaje de definición de gemelos digitales (DTDL)](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL). IoT Plug and Play y DTDL están abiertos a la comunidad y Microsoft agradece la colaboración de clientes, asociados y del sector. Ambos se basan en estándares de W3C abiertos, como JSON-LD y RDF, lo que facilita la adopción entre servicios y herramientas. Además, no hay costo adicional por el uso de IoT Plug and Play y DTDL. Las tarifas estándar de [Azure IoT Hub](../iot-hub/about-iot-hub.md), [Azure IoT Central](../iot-central/core/overview-iot-central.md) y otros servicios de Azure siguen siendo las mismas.

Las soluciones basadas en IoT Hub o IoT Central pueden beneficiarse de IoT Plug and Play.

En este artículo se describen:

- Los roles típicos asociados a un proyecto que usa IoT Plug and Play.
- Cómo usar dispositivos de IoT Plug and Play en una aplicación.
- Cómo desarrollar una aplicación de dispositivo IoT que admita IoT Plug and Play.
- Cómo certificar un dispositivo IoT Plug and Play y publicarlo en el [catálogo de dispositivos Azure Certified for IoT](https://catalog.azureiotsolutions.com/).

## <a name="user-roles"></a>Roles de usuario

IoT Plug and Play es útil para dos tipos de desarrolladores:

- Los _desarrolladores de soluciones_ es responsable del desarrollo de una solución de IoT mediante Azure IoT y otros recursos de Azure, y para identificar los dispositivos IoT que se van a integrar.
- Un _desarrollador de dispositivos_, que crea el código que se ejecuta en un dispositivo conectado a la solución.

## <a name="use-iot-plug-and-play-devices"></a>Uso de dispositivos IoT Plug and Play

Los desarrolladores de soluciones pueden desarrollar soluciones de IoT hospedadas en la nube que usen dispositivos IoT Plug and Play. Puede usar cualquiera de los siguientes servicios de Azure:

- [IoT Central](../iot-central/core/overview-iot-central.md):una solución de software como servicio de IoT completamente administrada que facilita la creación de productos que conectan los mundos físico y digital.
- [IoT Hub](../iot-hub/about-iot-hub.md): un servicio en la nube administrado que actúa como centro de mensajes para que haya una comunicación segura bidireccional segura entre la aplicación de IoT y los dispositivos.

Puede encontrar dispositivos IoT Plug and Play en el catálogo de dispositivos Azure Certified for IoT. Todos los dispositivos IoT Plug and Play del catálogo se han validado y tienen un modelo de funcionalidad del dispositivo. Si ve el modelo de funcionalidad del dispositivo, conocerá la funcionalidad del dispositivo o úselo para simular el dispositivo en Azure IoT Central.

Al conectar un dispositivo IoT Plug and Play, puede ver su modelo de funcionalidad del dispositivo, las interfaces incluidas en el modelo y los datos de telemetría, las propiedades y los comandos definidos en esas interfaces.

## <a name="develop-an-iot-device-application"></a>Desarrollo de una aplicación de dispositivo IoT

Los desarrolladores de dispositivos pueden desarrollar un producto de hardware de IoT que admita IoT Plug and Play. El proceso implica dos pasos clave:

1. Defina el modelo de funcionalidad del dispositivo y las interfaces. Cree un conjunto de archivos JSON que declaren las funcionalidades del dispositivo mediante el [DTDL](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL). Un modelo de funcionalidad del dispositivo describe una entidad completa, como un producto físico, y define el conjunto de interfaces que implementa esa entidad. Las interfaces son contratos compartidos que identifican de forma única la telemetría, las propiedades y los comandos que admite un dispositivo. Las interfaces se pueden reutilizar en distintos modelos de funcionalidad del dispositivo.

1. Cree el software o firmware del dispositivo que implemente las funcionalidades declaradas en el modelo de funcionalidad del dispositivo y las interfaces. El SDK de Azure IoT incluye las API necesarias para implementar modelos de funcionalidad del dispositivo.

El paquete de extensiones [Azure IoT Tools para VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) proporciona muchas características que le ayudarán. Por ejemplo, los desarrolladores de dispositivos pueden usar una extensión para generar un proyecto en C base a partir de un modelo de funcionalidad. Sin embargo, pueden usar cualquier IDE para crear e implementar modelos de funcionalidad del dispositivo.

## <a name="certify-an-iot-plug-and-play-device"></a>Certificación de un dispositivo IoT Plug and Play

Los desarrolladores de dispositivos pueden enviar productos de hardware de IoT para su certificación. Los dispositivos certificados se pueden publicar en el catálogo de dispositivos Certified for IoT. Estos son los pasos del proceso de certificación:

- Conectar con [Microsoft Partner Network](https://partner.microsoft.com).
- Incorporarse al portal de Certified for Azure IoT.
- Enviar un modelo de funcionalidad del dispositivo IoT Plug and Play e información de marketing para crear un registro de dispositivo.
- Pasar un conjunto automatizado de pruebas de validación para el dispositivo.
- Realizar la publicación en el catálogo de dispositivos Certified for IoT.

## <a name="regional-availability"></a>Disponibilidad regional

Durante la versión preliminar pública, IoT Plug and Play está disponible en todas las regiones.

## <a name="message-quotas-in-iot-hub"></a>Cuotas de mensajes en IoT Hub
Durante la versión preliminar pública, los dispositivos IoT Plug and Play envían mensajes independientes por interfaz, lo que puede aumentar el número de mensajes considerados en la [cuota de mensajes](../iot-hub/iot-hub-devguide-quotas-throttling.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una visión general de IoT Plug and Play, el siguiente paso que se recomienda es probar uno de los tutoriales de inicio rápido:

- [Uso de un modelo de funcionalidad de dispositivo para un dispositivo IoT Plug and Play](./quickstart-create-pnp-device-windows.md)
- [Conexión de un dispositivo a IoT Hub](./quickstart-connect-pnp-device-c-windows.md)
- [Conexión a un dispositivo en la solución](./quickstart-connect-pnp-device-solution-node.md)
