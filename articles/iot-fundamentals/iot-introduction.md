---
title: Introducción a Internet de las cosas (IoT) de Azure
description: Esta introducción explica los fundamentos de IoT de Azure y los servicios de IoT e incluye ejemplos que ayudan a ilustrar el uso de IoT.
author: dominicbetts
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 01/15/2020
ms.author: dobett
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 73eb0b3164a386bb270e42ceba56d5dc7045af1c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81729002"
---
# <a name="what-is-azure-internet-of-things-iot"></a>¿Qué es Internet de las cosas (IoT) de Azure?

Internet de las cosas (IoT) de Azure es un conjunto de servicios en la nube administrados por Microsoft que permiten conectar, supervisar y controlar miles de millones de recursos de IoT. En términos más sencillos, una solución de IoT se compone de uno o varios dispositivos IoT y uno o varios servicios de back-end que se ejecutan en la nube y se comunican entre sí. 

## <a name="iot-devices"></a>Dispositivos IoT

Habitualmente, los dispositivos IoT están compuestos por una placa de circuitos con sensores acoplados que usan WiFi para conectarse a Internet. Por ejemplo:

* Un sensor de presión de una bomba de petróleo remota.
* Los sensores de temperatura y humedad de una unidad de aire acondicionado.
* Un acelerómetro de un ascensor.
* Los sensores de presencia de una sala.

Hay disponibles una amplia variedad de dispositivos de distintos fabricantes para compilar la solución. Para ver una lista de los dispositivos certificados para trabajar con Azure IoT Hub, consulte el [catálogo de dispositivos Azure Certified for IoT](https://catalog.azureiotsolutions.com/alldevices). Para prototipos se pueden usar dispositivos como [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) o [Raspberry Pi](https://www.raspberrypi.org/). Devkit tiene sensores integrados de temperatura, presión y humedad, así como un giroscopio, un acelerómetro y un magnetómetro. Raspberry Pi permite conectar muchos tipos diferentes de sensores. 

Microsoft proporciona varios [SDK de dispositivo](../iot-hub/iot-hub-devguide-sdks.md) de código abierto que se pueden usar para compilar las aplicaciones que se ejecutan en los dispositivos. Estos [SDK simplifican y aceleran](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) el desarrollo de soluciones de IoT.

## <a name="communication"></a>Comunicación

Habitualmente, los dispositivos IoT envían datos de telemetría desde sus sensores a los servicios back-end en la nube. Sin embargo, hay otros tipos de comunicación posibles, como un servicio back-end que envía comandos a los dispositivos. A continuación, encontrará ejemplos de comunicación de dispositivo a la nube y de la nube a dispositivo:

* Un camión refrigerador móvil envía datos de temperatura cada 5 minutos a un centro de IoT. 

* El servicio back-end envía un comando a un dispositivo para cambiar la frecuencia con la que envía los datos de telemetría que ayudan a diagnosticar un problema. 

* Un dispositivo envía alertas en función de los valores que se leen de sus sensores. Por ejemplo, un dispositivo que supervisa un reactor por lotes en una planta química envía una alerta cuando la temperatura supera un valor determinado.

* Los dispositivos envían información, que se muestra en un panel para que puedan verla los operadores humanos. Por ejemplo, una sala de control de una refinería puede mostrar la temperatura, presión y volúmenes de flujo de cada canalización, lo que permite que los operadores supervisen la instalación. 

Los [SDK de dispositivo IoT](../iot-hub/iot-hub-devguide-sdks.md) e IoT Hub admiten [protocolos de comunicación](../iot-hub/iot-hub-devguide-protocols.md) comunes, como HTTP, MQTT y AMQP.

Los dispositivos IoT tienen características diferentes cuando se comparan con otros clientes, como los exploradores y las aplicaciones móviles. Los SDK de dispositivo ayudan a solucionar los problemas relacionados con la conexión de dispositivos de forma segura y confiable a un servicio back-end.  En concreto, los dispositivos de IoT:

* A menudo son sistemas insertados sin operador humano (a diferencia de un teléfono).
* Se pueden implementar en ubicaciones remotas, donde el acceso físico resulta costoso.
* Es posible que solo sean accesibles a través del back-end de soluciones.
* Es posible que tengan limitaciones de recursos de procesamiento y alimentación.
* Es posible que tengan conectividad de red intermitente, lenta o costosa.
* Es posible que necesiten usar protocolos de aplicación propios, personalizados o específicos de determinados sectores.

## <a name="back-end-services"></a>Servicios de back-end 

En las solución de IoT, el servicio back-end proporciona funciones como:

* Recibir datos de telemetría a escala desde los dispositivos y determinar cómo procesar y almacenar esos datos.
* Analizar los datos de telemetría para proporcionar información detallada, ya sea en tiempo real o después de que se produzcan los hechos.
* Enviar comandos desde la nube a un dispositivo específico. 
* Aprovisionar dispositivos y controlar qué dispositivos se pueden conectar a una infraestructura.
* Controlar el estado de los dispositivos y supervisar sus actividades.
* Administrar el firmware instalado en los dispositivos.

Por ejemplo, en una solución de supervisión remota de una estación de bombeo de petroleo, el back-end de la nube usa los datos de telemetría de las bombas para identificar cualquier comportamiento anómalo. Cuando el servicio back-end identifica una anomalía, puede enviar automáticamente un comando al dispositivo para que este realice alguna acción correctora. Este proceso genera un bucle de realimentación automatizado entre el dispositivo y la nube que aumenta en gran medida la eficacia de la solución.

## <a name="azure-iot-examples"></a>Ejemplos de Azure IoT

Para ver ejemplos reales de cómo utilizan Azure IoT las organizaciones, consulte los [casos de estudio técnicos de Microsoft para IoT](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured). 

## <a name="next-steps"></a>Pasos siguientes

Para algunos casos de negocio reales y la arquitectura utilizada, consulte [Estudios de casos técnicos para IoT de Microsoft Azure](https://microsoft.github.io/techcasestudies/#technology=IoT&sortBy=featured).

Para algunos proyectos de ejemplo que puede probar con IoT DevKit, consulte el [Catálogo de proyectos de IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/). 

Para obtener una explicación más completa de los distintos servicios, así como de su uso, consulte [Tecnologías y servicios de Azure para crear soluciones de IoT](iot-services-and-technologies.md).

Para ver un análisis detallado de la arquitectura de IoT, consulte [Microsoft Azure IoT Reference Architecture](https://aka.ms/iotrefarchitecture) (Arquitectura de referencia de IoT de Microsoft Azure).
