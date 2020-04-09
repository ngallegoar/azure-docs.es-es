---
title: Información general del acelerador de la solución de supervisión remota | Microsoft Docs
description: Este artículo proporciona información general de algunos de los elementos clave de la solución de supervisión remota para que pueda entender cómo funciona.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: f501eb55f72811063ddf1d8e02a0ce2137d598f3
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546308"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Información general sobre el acelerador de la solución de supervisión remota

El [acelerador de la solución](../iot-accelerators/about-iot-accelerators.md) de supervisión remota implementa una solución de supervisión integral para varias máquinas en ubicaciones remotas. La solución combina servicios clave de Azure para proporcionar una implementación genérica del escenario de negocio. Puede usar la solución como punto de partida para su propia implementación, así como [personalizarla](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) para cumplir sus requisitos empresariales específicos.

Este artículo le guiará a través de algunos de los elementos clave de la solución de supervisión remota para que pueda entender cómo funciona. Esta información le ayuda a:

* Solucionar problemas de la solución.
* Planear cómo personalizar la solución para satisfacer sus propios requisitos específicos.
* Diseñar una solución de IoT propia que utilice servicios de Azure.

El código del acelerador de soluciones de supervisión remota está disponible en GitHub:

* [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

## <a name="logical-architecture"></a>Arquitectura lógica

El siguiente diagrama muestra los componentes lógicos del acelerador de la solución de supervisión remota superpuesta en la [arquitectura de IoT](../iot-fundamentals/iot-introduction.md):

![Arquitectura lógica](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>¿Por qué microservicios?

La arquitectura en la nube ha evolucionado desde el lanzamiento de Microsoft de los primeros aceleradores de soluciones. Los [microservicios](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) han surgido como un procedimiento probado para alcanzar el escalado y la flexibilidad, sin sacrificar la velocidad de desarrollo. Este modelo de arquitectura se usa internamente en varios servicios de Microsoft con grandes resultados con respecto a la escalabilidad y la confiabilidad. Los aceleradores de soluciones actualizados ponen estos conocimientos en práctica para que también pueda beneficiarse de ellos.

> [!TIP]
> Para obtener más información acerca de las arquitecturas de microservicios, consulte [arquitectura de aplicaciones .NET](https://www.microsoft.com/net/learn/architecture) y [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservicios: Una revolución en las aplicaciones con la tecnología de la nube).

## <a name="device-connectivity"></a>Conectividad de dispositivos

La solución incluye los componentes siguientes en la parte de la conectividad de dispositivos de la arquitectura lógica:

### <a name="real-devices"></a>Dispositivos reales

Puede conectar dispositivos reales a la solución. Puede implementar el comportamiento de los dispositivos simulados con los SDK de dispositivo IoT de Azure.

Puede aprovisionar dispositivos reales desde el panel en el portal de la solución.

### <a name="device-simulation-microservice"></a>Microservicio de simulación de dispositivo

La solución incluye el [microservicio de simulación de dispositivo](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-simulation) que le permite administrar un grupo de dispositivos simulados a partir del portal de la solución para probar el flujo de un extremo a otro en la solución. Los dispositivos simulados:

* Generan datos de telemetría del dispositivo a la nube.
* Responder a llamadas a métodos de la nube al dispositivo desde IoT Hub.

El microservicio proporciona un punto de conexión REST para permitirle crear, iniciar y detener simulaciones. Cada simulación consta de un conjunto de dispositivos virtuales de diferentes tipos que envían datos de telemetría y responden a llamadas a métodos.

Puede aprovisionar dispositivos simulados desde el panel en el portal de la solución.

### <a name="iot-hub"></a>IoT Hub

El [centro de IoT](../iot-hub/index.yml) ingiere datos de telemetría enviados desde los dispositivos reales y simulados a la nube. El centro de IoT pone los datos de telemetría a disposición de los servicios en el back-end de la solución de IoT para su procesamiento.

La instancia de IoT Hub en la solución también:

* Mantiene un registro de identidades que almacena los identificadores y las claves de autenticación de todos los dispositivos que se pueden conectar al portal.
* Invoca los métodos de los dispositivos en nombre del acelerador de soluciones.
* Mantiene los dispositivos gemelos para todos los dispositivos registrados. Un dispositivo gemelo almacena los valores de la propiedad notificados por un dispositivo. Un dispositivo gemelo también almacena las propiedades deseadas, establecidas en el portal de la solución, para que el dispositivo las recupere la siguiente vez que se conecte.
* Programa trabajos para establecer las propiedades de varios dispositivos o invocar métodos en varios dispositivos.

## <a name="data-processing-and-analytics"></a>Procesamiento de datos y análisis

La solución incluye los componentes siguientes en la parte de la proceso de datos y análisis de la arquitectura lógica:

### <a name="iot-hub-manager-microservice"></a>Microservicio del administrador de IoT Hub

La solución incluye el [microservicio de administrador de IoT Hub](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) para controlar las interacciones con el centro de IoT, como:

* Crear y administrar dispositivos de IoT.
* Administrar dispositivos gemelos.
* Invocar métodos en los dispositivos.
* Administrar credenciales de IoT.

Este servicio también ejecuta consultas de IoT Hub para recuperar los dispositivos que pertenecen a grupos definidos por el usuario.

El microservicio proporciona un punto de conexión REST para administrar dispositivos y dispositivos gemelos, invocar métodos y ejecutar consultas de IoT Hub.

### <a name="device-telemetry-microservice"></a>Microservicio de telemetría de dispositivo

El [microservicio de telemetría de dispositivo](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-telemetry) proporciona un punto de conexión RESTful para acceso de lectura a los datos de telemetría de un dispositivo almacenados en Time Series Insights. El punto de conexión RESTful también permite operaciones CRUD en las reglas y acceso de lectura/escritura para las definiciones de alarma desde el almacenamiento.

### <a name="storage-adapter-microservice"></a>Microservicio de adaptador de almacenamiento

El [microservicio de adaptador de almacenamiento](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/storage-adapter) administra los pares de clave-valor, abstrayendo la semántica del servicio de almacenamiento y presentando una interfaz sencilla para almacenar datos de cualquier formato mediante Azure Cosmos DB.

Los valores se organizan en colecciones. Puede trabajar con valores individuales o recuperar colecciones completas. Los clientes serializan las estructuras de datos complejas, y estas se administran como una carga de texto simple.

El servicio proporciona un punto de conexión RESTful para operaciones CRUD en pares clave-valor. valores

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Las implementaciones del acelerador de soluciones usan [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) para almacenar reglas, alarmas, valores de configuración y todo el almacenamiento en frío adicional.

### <a name="azure-stream-analytics-manager-microservice"></a>Microservicio de administrador de Azure Stream Analytics

El [microservicio de administrador de Azure Stream Analytics](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/asa-manager) administra trabajos de Azure Stream Analytics (ASA), incluida la definición de su configuración, su inicio y detención, y la supervisión de su estado.

El trabajo de ASA es compatible con dos conjuntos de datos de referencia. Un conjunto de datos define las reglas y otro define grupos de dispositivos. Los datos de referencia de las reglas se generan a partir de la información administrada por el microservicio de telemetría de dispositivo. El microservicio de administrador de Azure Stream Analytics transforma las reglas de telemetría en lógica de procesamiento de streaming.

Los datos de referencia de grupos de dispositivos se usan para identificar qué grupo de reglas aplicar a un mensaje de telemetría entrante. El microservicio de configuración administra los grupos de dispositivos, y usa consultas de dispositivos gemelos de Azure IoT Hub.

Los trabajos de ASA entregan los datos de telemetría de los dispositivos conectados a Time Series Insights para su almacenamiento y análisis.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) es un motor de procesamiento de eventos que permite examinar grandes volúmenes de streaming de datos procedentes de dispositivos.

### <a name="azure-time-series-insights"></a>Azure Time Series Insights

[Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/) almacena los datos de telemetría de los dispositivos conectados al acelerador de soluciones. También permite visualizar y consultar la telemetría del dispositivo en la interfaz de usuario web de la solución.

### <a name="configuration-microservice"></a>Microservicio de configuración

El [microservicio de configuración](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config) proporciona un punto de conexión RESTful para operaciones CRUD en grupos de dispositivos, configuración de la solución y configuración del usuario en el acelerador de soluciones. Funciona con el microservicio de adaptador de almacenamiento para conservar los datos de configuración.

### <a name="authentication-and-authorization-microservice"></a>Microservicio de autenticación y autorización

El [microservicio de autenticación y autorización](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) administra los usuarios autorizados para acceder al acelerador de soluciones. La administración de usuarios puede realizarse con cualquier proveedor de servicios de identidad que admita [OpenId Connect](https://openid.net/connect/).

### <a name="azure-active-directory"></a>Azure Active Directory

Las implementaciones del acelerador de soluciones usan [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) como proveedor de OpenID Connect. Azure Active Directory almacena la información de los usuarios y proporciona certificados para validar las firmas de tokens JWT.

## <a name="presentation"></a>Presentación

La solución incluye los componentes siguientes en la parte de presentación de la arquitectura lógica:

La [interfaz de usuario web es una aplicación React de Javascript](https://github.com/Azure/pcs-remote-monitoring-webui). La aplicación:

* Solo utiliza Javascript React y se ejecuta completamente en el explorador.
* El estilo se realiza con CSS.
* Interactúa con los microservicios de acceso público a través de llamadas AJAX.

La interfaz de usuario presenta toda la funcionalidad del acelerador de soluciones e interactúa con otros microservicios, como:

* El microservicio de autenticación y autorización para proteger los datos del usuario.
* El microservicio de administrador de IoT Hub para enumerar y administrar los dispositivos de IoT.

La interfaz de usuario integra el explorador de Azure Time Series Insights para habilitar consultas y análisis de telemetría del dispositivo.

El microservicio de configuración permite que la interfaz de usuario almacene y recupere valores de configuración.

## <a name="next-steps"></a>Pasos siguientes

Si quiere explorar el código fuente y la documentación para desarrolladores, comience con uno los dos repositorios de GitHub:

* [Acelerador de la solución de supervisión remota con Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).
* [Acelerador de la solución de supervisión remota con Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Diagramas detallados de la arquitectura de la solución:
* [Acelerador de la solución de supervisión remota (arquitectura)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Para obtener información conceptual sobre el acelerador de la solución de supervisión remota, consulte [Personalizar el acelerador de la solución](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
