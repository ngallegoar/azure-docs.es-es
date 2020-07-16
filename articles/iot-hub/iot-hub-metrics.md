---
title: Uso de métricas para supervisar Azure IoT Hub | Microsoft Docs
description: Describe cómo usar las métricas de IoT Hub de Azure para evaluar y supervisar el estado general de los centros de IoT Hub.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: 808320f89c4dbeca835fc5a710ea1566199f6884
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84791850"
---
<!--for build: for each metric, if you understand what it is, it's ok. otw add more info.  -->
# <a name="understand-iot-hub-metrics"></a>Comprender las métricas de IoT Hub

Las métricas de IoT Hub ofrecen información sobre el estado de los recursos de Azure en la suscripción de Azure. Las métricas de IoT Hub ayudan a evaluar el estado general del servicio IoT Hub y de los dispositivos conectados a él. Estas estadísticas orientadas al usuario ayudan a ver lo que está ocurriendo con su centro de IoT y ayudan a realizar el análisis de la causa raíz de los problemas sin necesidad de ponerse en contacto con el soporte técnico de Azure.

Las métricas están habilitadas de forma predeterminada. Puede ver las métricas de IoT Hub desde Azure Portal.

> [!NOTE]
> Puede usar métricas de IoT Hub para ver información acerca de los dispositivos IoT Plug and Play conectados a su centro de IoT. Los dispositivos IoT Plug and Play forman parte de la [versión preliminar pública de IoT Plug and Play](../iot-pnp/overview-iot-plug-and-play.md).

## <a name="how-to-view-iot-hub-metrics"></a>Cómo ver las métricas de IoT Hub

1. Cree un Centro de IoT. Puede encontrar instrucciones sobre cómo crear una instancia de IoT hub en la guía [Enviar datos de telemetría desde un dispositivo a IoT Hub](quickstart-send-telemetry-dotnet.md).

2. Abra la hoja de su centro de IoT. Desde aquí, haga clic en **Métricas**.
   
    ![Captura de pantalla que muestra dónde está la opción de métricas en la página de recursos de IoT Hub](./media/iot-hub-metrics/enable-metrics-1.png)

3. En la hoja de métricas, puede ver las métricas para su centro de IoT y crear vistas personalizadas de ellas. 
   
    ![Captura de pantalla que muestra la página de métricas de IoT Hub](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. Puede elegir entre enviar los datos de las métricas a un punto de conexión de Event Hubs o a una cuenta de Azure Storage haciendo clic en **Configuración de diagnóstico** y luego en **Agregar configuración de diagnóstico**.

   ![Captura de pantalla muestra dónde está el botón de configuración de diagnóstico](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>Las métricas de IoT Hub y su uso

IoT Hub proporciona varias métricas para ofrecerle una visión general del estado de su centro y el número total de dispositivos conectados. Puede combinar información de varias métricas para conseguir una imagen más amplia del estado del Centro de IoT. La tabla siguiente describe las métricas de las que cada Centro de IoT realiza un seguimiento y cómo se relaciona cada métrica con el estado general del Centro de IoT.

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|RoutingDeliveries | Intentos de entrega de enrutamiento (versión preliminar) | Count | Total |Esta es la métrica de entrega de enrutamiento. Use las dimensiones para identificar el estado de entrega para un punto de conexión específico o para un origen de enrutamiento específico.| ResourceID,<br>Result,<br>RoutingSource,<br>EndpointType,<br>FailureReasonCategory,<br>EndpointName<br>*Puede encontrar más detalles sobre las dimensiones [**aquí**](#dimensions)* . |
|RoutingDeliveryLatency| Latencia de enrutamiento (versión preliminar) | Milisegundos | Average |Esta es la métrica de latencia de entrega de enrutamiento. Use las dimensiones para identificar la latencia para un punto de conexión específico o para un origen de enrutamiento específico.| ResourceID,<br>RoutingSource,<br>EndpointType,<br>EndpointName<br>*Puede encontrar más detalles sobre las dimensiones [**aquí**](#dimensions)* .|
|RoutingDataSizeInBytesDelivered| Tamaño de los datos de entrega de enrutamiento en bytes (versión preliminar)| Bytes | Total |Número total de bytes que IoT Hub enruta al punto de conexión personalizado y al punto de conexión integrado. Use las dimensiones para identificar el tamaño de los datos enrutados a un punto de conexión específico o para un origen de enrutamiento específico.| ResourceID,<br>RoutingSource,<br>EndpointType<br>EndpointName<br>*Puede encontrar más detalles sobre las dimensiones [**aquí**](#dimensions)* .|
|d2c.telemetry.ingress.<br>allProtocol|Intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|None|
|d2c.telemetry.ingress.<br>success|Mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|None|
|c2d.commands.egress.<br>complete.success|C2D message deliveries completed (Entregas de mensajes de C2D completadas)|Count|Total|Número de entregas de mensajes de la nube al dispositivo que el dispositivo ha completado correctamente.|None|
|c2d.commands.egress.<br>abandon.success|C2D messages abandoned (Mensajes de C2D abandonados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha abandonado.|None|
|c2d.commands.egress.<br>reject.success|C2D messages rejected (Mensajes de C2D rechazados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha rechazado.|None|
|C2DMessagesExpired|C2D Messages Expired (preview) [Mensajes de C2D expirados (versión preliminar)]|Count|Total|Número de mensajes de la nube al dispositivo expirados|None|
|devices.totalDevices|Total de dispositivos (en desuso)|Count|Total|Número de dispositivos registrados en IoT Hub|None|
|devices.connectedDevices.<br>allProtocol|Dispositivos conectados (en desuso) |Count|Total|Número de dispositivos conectados a IoT Hub|None|
|d2c.telemetry.egress.<br>success|Enrutamiento: mensajes de telemetría entregados|Count|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|None|
|d2c.telemetry.egress.<br>dropped|Enrutamiento: mensajes de telemetría quitados |Count|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|None|
|d2c.telemetry.egress.<br>orphaned|Enrutamiento: mensajes de telemetría huérfanos |Count|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con ninguna consulta de enrutamiento, cuando la ruta de reserva está deshabilitada.|None|
|d2c.telemetry.egress.<br>no válido|Enrutamiento: mensajes de telemetría incompatibles|Count|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Un mensaje es incompatible con un punto de conexión cuando IoT Hub intenta entregar el mensaje a un punto de conexión y se produce un error no transitorio. Los mensajes no válidos no se vuelven a intentar. Este valor no incluye los reintentos.|None|
|d2c.telemetry.egress.<br>fallback|Enrutamiento: mensajes entregados a la reserva|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|None|
|d2c.endpoints.egress.<br>eventHubs|Enrutamiento: mensajes entregados al centro de eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión personalizados de tipo Centro de eventos. Esto no incluye las rutas de mensajes a un punto de conexión integrado (eventos).|None|
|d2c.endpoints.latency.<br>eventHubs|Enrutamiento: latencia de mensajes para el centro de eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a puntos de conexión personalizados de tipo Centro de eventos. Esto no incluye las rutas de mensajes a un punto de conexión integrado (eventos).|None|
|d2c.endpoints.egress.<br>serviceBusQueues|Enrutamiento: mensajes entregados a la cola de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|None|
|d2c.endpoints.latency.<br>serviceBusQueues|Enrutamiento: latencia de mensajes de la cola de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión de la cola de Service Bus.|None|
|d2c.endpoints.egress.<br>serviceBusTopics|Enrutamiento: mensajes entregados al tema de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|None|
|d2c.endpoints.latency.<br>serviceBusTopics|Enrutamiento: latencia de mensajes del tema de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del tema de Service Bus.|None|
|d2c.endpoints.egress.<br>builtIn.events|Enrutamiento: mensajes entregados a mensajes/eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos) y a la ruta de reserva.|None|
|d2c.endpoints.latency.<br>builtIn.events|Enrutamiento: latencia de mensajes para mensajes/eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes al punto de conexión integrado (mensajes/eventos) y a la ruta de reserva.|None|
|d2c.endpoints.egress.<br>storage|Enrutamiento: mensajes entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|None|
|d2c.endpoints.latency.<br>storage|Enrutamiento: latencia de mensajes para almacenamiento|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión de almacenamiento.|None|
|d2c.endpoints.egress.<br>storage.bytes|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|None|
|d2c.endpoints.egress.<br>storage.blobs|Enrutamiento: blobs entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|None|
|EventGridDeliveries|Event Grid deliveries(preview) [Entregas de Event Grid (versión preliminar)]|Count|Total|El número de eventos de IoT Hub publicados en Event Grid. Use la dimensión de resultado para ver el número de solicitudes correctas e incorrectas. La dimensión EventType muestra el tipo de evento (https://aka.ms/ioteventgrid).|ResourceId,<br/>Result,<br/>EventType|
|EventGridLatency|Latencia de Event Grid (versión preliminar)|Milisegundos|Average|La latencia media (milisegundos) desde el momento en que se generó el evento de IoT Hub hasta el momento en que se publicó el evento en Event Grid. Este número es un promedio entre todos los tipos de evento. Use la dimensión EventType para ver la latencia de un tipo específico de evento.|ResourceId,<br/>EventType|
|d2c.twin.read.success|Lecturas gemelas correctas de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.read.failure|Lecturas gemelas con error de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|None|
|d2c.twin.read.size|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Average|El número de todas las lecturas gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.update.success|Actualizaciones gemelas correctas de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.update.failure|Actualizaciones gemelas con error de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|None|
|d2c.twin.update.size|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Average|El tamaño total de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|None|
|c2d.methods.success|Invocaciones correctas al método directo|Count|Total|El número de todas las llamadas correctas al método directo.|None|
|c2d.methods.failure|Invocaciones al método directo con error|Count|Total|El número de todas las llamadas al método directo con error.|None|
|c2d.methods.requestSize|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Average|El número de todas las solicitudes correctas del método directo.|None|
|c2d.methods.responseSize|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Average|El número de todas las respuestas correctas del método directo.|None|
|c2d.twin.read.success|Lecturas gemelas correctas del back-end|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.read.failure|Lecturas gemelas con error del back-end|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|None|
|c2d.twin.read.size|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Average|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.update.success|Actualizaciones gemelas correctas del back-end|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.update.failure|Actualizaciones gemelas con error del back-end|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|None|
|c2d.twin.update.size|Tamaño de las actualizaciones gemelas del back-end|Bytes|Average|El tamaño total de todas las actualizaciones gemelas correctas iniciadas por el back-end.|None|
|twinQueries.success|Consultas gemelas correctas|Count|Total|El recuento de todas las consultas gemelas correctas.|None|
|twinQueries.failure|Consultas gemelas con error|Count|Total|El recuento de todas las consultas gemelas con error.|None|
|twinQueries.resultSize|Tamaño de resultado de consultas gemelas|Bytes|Average|El total del tamaño del resultado de todas las consultas gemelas correctas.|None|
|jobs.createTwinUpdateJob.<br>success|Creaciones correctas de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|None|
|jobs.createTwinUpdateJob.<br>failure|Creaciones con error de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|None|
|jobs.createDirectMethodJob.<br>success|Creaciones correctas de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|None|
|jobs.createDirectMethodJob.<br>failure|Creaciones con error de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|None|
|jobs.listJobs.success|Llamadas correctas para enumerar trabajos|Count|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|None|
|jobs.listJobs.failure|Llamadas con error para enumerar trabajos|Count|Total|El recuento de todas las llamadas con error para enumerar trabajos.|None|
|jobs.cancelJob.success|Cancelaciones de trabajos correctas|Count|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|None|
|jobs.cancelJob.failure|Cancelaciones de trabajos con error|Count|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|None|
|jobs.queryJobs.success|Consultas de trabajo correctas|Count|Total|El recuento de todas las llamadas correctas para consultar trabajos.|None|
|jobs.queryJobs.failure|Consultas de trabajo con error|Count|Total|El recuento de todas las llamadas con error para consultar trabajos.|None|
|jobs.completed|Trabajos completados|Count|Total|El recuento de todos los trabajos completados.|None|
|jobs.failed|Trabajos con error|Count|Total|El recuento de todos los trabajos con error.|None|
|d2c.telemetry.ingress.<br>sendThrottle|Número de errores de limitación|Count|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|None|
|dailyMessageQuotaUsed|Número total de mensajes usados|Count|Average|Número de mensajes totales usados hoy. Se trata de un valor acumulado que se restablece en cero a las 00:00 UTC cada día.|None|
|deviceDataUsage|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|None|
|deviceDataUsageV2|Uso total de datos del dispositivo (versión preliminar)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|None|
|totalDeviceCount|Total de dispositivos (versión preliminar)|Count|Average|Número de dispositivos registrados en IoT Hub|None|
|connectedDeviceCount|Dispositivos conectados (versión preliminar)|Count|Average|Número de dispositivos conectados a IoT Hub|None|
|configuraciones|Métricas de configuración|Count|Total|Número total de operaciones CRUD realizadas para la configuración de dispositivos y la implementación de IoT Edge en un conjunto de dispositivos de destino. Esto también incluye el número de operaciones que modifican el dispositivo gemelo o el módulo gemelo debido a estas configuraciones.|None|

### <a name="dimensions"></a>Dimensions
Las dimensiones ayudan a identificar más detalles sobre las métricas. Algunas de las métricas de enrutamiento proporcionan información por punto de conexión. En la tabla siguiente se enumeran los valores posibles para estas dimensiones.

|Dimensión|Valores|
|---|---|
|ResourceID|su recurso de IoT Hub|
|Resultado|success<br>failure|
|RoutingSource|Mensajes del dispositivo<br>Eventos de cambio de gemelo<br>Eventos de ciclo de vida del dispositivo|
|EndpointType|eventHubs<br>serviceBusQueues<br>cosmosDB<br>serviceBusTopics<br>builtin<br>blobStorage|
|FailureReasonCategory|no válido<br>dropped<br>orphaned<br>null|
|EndpointName|nombre del punto de conexión|

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha visto la información general sobre las métricas de IoT Hub, siga este vínculo para más información sobre la administración de IoT Hub de Azure:

* [Configuración de registros de diagnósticos](iot-hub-monitor-resource-health.md)

* [Información sobre cómo solucionar problemas de enrutamiento de mensajes](troubleshoot-message-routing.md)

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Guía para desarrolladores de IoT Hub](iot-hub-devguide.md)

* [Implementación de IA en dispositivos perimetrales con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
