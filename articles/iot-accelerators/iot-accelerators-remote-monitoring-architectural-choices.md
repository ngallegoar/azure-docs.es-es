---
title: 'Opciones de la arquitectura de la solución de supervisión remota: Azure | Microsoft Docs'
description: En este artículo se describen las opciones técnicas y de la arquitectura de la supervisión remota
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/20/2018
ms.topic: conceptual
ms.openlocfilehash: 1bd08596a30db7322a72b4269fddfe0b9df19119
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "61447188"
---
# <a name="remote-monitoring-architectural-choices"></a>Opciones de arquitectura de supervisión remota

El acelerador de soluciones de supervisión remota de Azure IoT es un acelerador de soluciones con licencia MIT y de código abierto. Para ayudar a acelerar el proceso de desarrollo de IoT, muestra escenarios comunes de IoT, como:

- Conectividad de dispositivos
- Administración de dispositivos
- Procesamiento de flujos

La solución de Supervisión remota sigue la [arquitectura de referencia recomendada de Azure IoT](https://aka.ms/iotrefarchitecture).

En este artículo, se describen las opciones de arquitectura y técnicas clave realizadas en cada uno de los subsistemas de supervisión remota. Sin embargo, las opciones técnicas de Microsoft en la solución de Supervisión remota no son la única forma de implementar una solución de IoT de supervisión remota. Se debe considerar la implementación técnica como una línea de base para la compilación de una aplicación correcta y debe modificarla para:

- Adaptar las habilidades y experiencia disponibles en su organización.
- Satisfacer las necesidades de aplicación vertical.

## <a name="architectural-choices"></a>Opciones de la arquitectura

La arquitectura que Microsoft recomienda para una aplicación de IoT es nativa en la nube, basada en microservicios y sin servidor. Debe compilar los diferentes subsistemas de una aplicación de IoT como servicios discretos que se pueden implementar y escalar de forma independiente. Estos atributos permiten mayor escala, más flexibilidad para actualizar los subsistemas individuales y proporcionan la flexibilidad para elegir la tecnología apropiada para cada subsistema.

Puede implementar microservicios con más de una tecnología. Por ejemplo, puede elegir cualquiera de las siguientes opciones para implementar un microservicio:

- Usar tecnología de contenedor, como Docker, con tecnología sin servidor, como Azure Functions.
- Hospedar sus microservicios en servicios de PaaS como Azure App Services.

## <a name="technology-choices"></a>Opciones de tecnología

En esta sección, se detallan las opciones de tecnología realizadas en la solución de supervisión remota para cada subsistema principal.

![Diagrama principal](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png)

### <a name="cloud-gateway"></a>Puerta de enlace en la nube

Azure IoT Hub se usa como la puerta de enlace en la nube de la solución de supervisión remota. [IoT Hub](https://azure.microsoft.com/services/iot-hub/) ofrece una comunicación segura y bidireccional con los dispositivos.

Para la conectividad de dispositivos IoT, puede usar:

- [SDK de dispositivos de IoT Hub](../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) para implementar una aplicación cliente nativa para el dispositivo. Los SDK ofrecen contenedores en torno a la API REST de IoT Hub y controlan escenarios como los reintentos.
- La integración con Azure IoT Edge para implementar y administrar módulos personalizados que se ejecutan en contenedores de los dispositivos.
- La integración con la administración de dispositivos automática en IoT Hub para administrar los dispositivos conectados de forma masiva.

### <a name="stream-processing"></a>Procesamiento de flujos

Para el procesamiento de flujos, la solución de supervisión remota usa Azure Stream Analytics para el procesamiento de reglas complejas. Si quiere usar reglas más sencillas, existe un microservicio personalizado con compatibilidad para el procesamiento de reglas sencillas, aunque esta configuración no forma parte de la implementación predefinida. La arquitectura de referencia recomienda usar Azure Functions para el procesamiento de reglas sencillas y Azure Stream Analytics para el procesamiento de reglas complejas.

### <a name="storage"></a>Storage

Para el almacenamiento, el acelerador de soluciones de supervisión remota usa tanto Azure Time Series Insights como Azure Cosmos DB. Azure Time Series Insights almacena los mensajes entrantes a través de IoT Hub desde los dispositivos conectados. El acelerador de soluciones usa Azure Cosmos DB con el resto del almacenamiento, como almacenamiento en frío, definiciones de reglas, alertas y valores de configuración.

Azure Cosmos DB es la solución de almacenamiento intermedio de uso general recomendada para aplicaciones IoT. Sin embargo, las soluciones como Azure Time Series Insights y Azure Data Lake son adecuadas para muchos casos de uso. Con Azure Time Series Insights, puede obtener conclusiones más detalladas de los datos de los sensores de serie temporal mediante una detección de tendencias y anomalías. Esta característica le permite realizar análisis de causa principal y evitar costosos tiempos de inactividad.

> [!NOTE]
> Time Series Insights actualmente no está disponible en la nube de Azure China. Las nuevas implementaciones del acelerador de soluciones de supervisión remota en la nube de Azure China usan Cosmos DB para todo el almacenamiento.

### <a name="business-integration"></a>Integración de negocios

La integración de negocios en la solución de supervisión remota se limita a la generación de alertas, que se colocan en el almacenamiento intermedio. Conecte la solución con Azure Logic Apps para implementar escenarios de integración empresarial más profundos.

### <a name="user-interface"></a>Interfaz de usuario

La interfaz de usuario web se compila con JavaScript React. React ofrece un marco de interfaz de usuario web de uso común en el sector y es similar a otros marcos populares como Angular.

### <a name="runtime-and-orchestration"></a>Automatización y orquestación

La solución de Supervisión remota usa contenedores de Docker para ejecutar subsistemas con Kubernetes como el orquestador del escalado horizontal. Esta arquitectura permite definiciones de escala individual para cada subsistema. Sin embargo, esta arquitectura incurre en costos de DevOps para mantener las máquinas virtuales y contenedores actualizados y seguros.

Las alternativas a Docker incluyen el hospedaje de microservicios en servicios de PaaS como Azure App Service. Las alternativas a Kubernetes incluyen orquestadores como Service Fabric, DC/OS o Swarm.

## <a name="next-steps"></a>Pasos siguientes

* Implemente la solución de supervisión remota [aquí](https://www.azureiotsolutions.com/).
* Explore el código de GitHub en [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) y [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* Obtenga más información sobre la arquitectura de referencia de IoT [aquí](https://aka.ms/iotrefarchitecture).
