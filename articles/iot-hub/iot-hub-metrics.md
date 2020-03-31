---
title: Uso de métricas para supervisar Azure IoT Hub | Microsoft Docs
description: Describe cómo usar las métricas de IoT Hub de Azure para evaluar y supervisar el estado general de los centros de IoT Hub.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: jlian
ms.openlocfilehash: ec8a00460b4a750339f929eb6879ac6eb63cac8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79237304"
---
# <a name="understand-iot-hub-metrics"></a>Comprender las métricas de IoT Hub

Las métricas de IoT Hub ofrecen mejores datos sobre el estado de los recursos de Azure en la suscripción de Azure. Las métricas de IoT Hub permiten evaluar el estado general del servicio IoT Hub y de los dispositivos conectados a él. Las estadísticas orientadas al usuario son importantes porque ayudan a ver lo que está ocurriendo con su Centro de IoT y ayudan a determinar la causa raíz de los problemas sin necesidad de ponerse en contacto con el soporte técnico de Azure.

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
|d2c.telemetry.ingress.allProtocol|Intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|None|
|d2c.telemetry.ingress.success|Mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|None|
|c2d.commands.egress.complete.success|C2D message deliveries completed (Entregas de mensajes de C2D completadas)|Count|Total|Número de entregas de mensajes de la nube al dispositivo que el dispositivo ha completado correctamente.|None|
|c2d.commands.egress.abandon.success|C2D messages abandoned (Mensajes de C2D abandonados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha abandonado.|None|
|c2d.commands.egress.reject.success|C2D messages rejected (Mensajes de C2D rechazados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha rechazado.|None|
|C2DMessagesExpired|C2D Messages Expired (preview) [Mensajes de C2D expirados (versión preliminar)]|Count|Total|Número de mensajes de la nube al dispositivo expirados|None|
|devices.totalDevices|Total de dispositivos (en desuso)|Count|Total|Número de dispositivos registrados en IoT Hub|None|
|devices.connectedDevices.allProtocol|Dispositivos conectados (en desuso) |Count|Total|Número de dispositivos conectados a IoT Hub|None|
|d2c.telemetry.egress.success|Enrutamiento: mensajes de telemetría entregados|Count|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|None|
|d2c.telemetry.egress.dropped|Enrutamiento: mensajes de telemetría quitados |Count|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|None|
|d2c.telemetry.egress.orphaned|Enrutamiento: mensajes de telemetría huérfanos |Count|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con las reglas de enrutamiento (incluida la regla de reserva). |None|
|d2c.telemetry.egress.invalid|Enrutamiento: mensajes de telemetría incompatibles|Count|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Este valor no incluye los reintentos.|None|
|d2c.telemetry.egress.fallback|Enrutamiento: mensajes entregados a la reserva|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|None|
|d2c.endpoints.egress.eventHubs|Enrutamiento: mensajes entregados al centro de eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del centro de eventos.|None|
|d2c.endpoints.latency.eventHubs|Enrutamiento: latencia de mensajes para el centro de eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del centro de eventos.|None|
|d2c.endpoints.egress.serviceBusQueues|Enrutamiento: mensajes entregados a la cola de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|None|
|d2c.endpoints.latency.serviceBusQueues|Enrutamiento: latencia de mensajes de la cola de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de la cola de Service Bus.|None|
|d2c.endpoints.egress.serviceBusTopics|Enrutamiento: mensajes entregados al tema de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|None|
|d2c.endpoints.latency.serviceBusTopics|Enrutamiento: latencia de mensajes del tema de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión del tema de Service Bus.|None|
|d2c.endpoints.egress.builtIn.events|Enrutamiento: mensajes entregados a mensajes/eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos).|None|
|d2c.endpoints.latency.builtIn.events|Enrutamiento: latencia de mensajes para mensajes/eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría al punto de conexión integrado (mensajes/eventos).|None|
|d2c.endpoints.egress.storage|Enrutamiento: mensajes entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|None|
|d2c.endpoints.latency.storage|Enrutamiento: latencia de mensajes para almacenamiento|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de almacenamiento.|None|
|d2c.endpoints.egress.storage.bytes|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|None|
|d2c.endpoints.egress.storage.blobs|Enrutamiento: blobs entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|None|
|EventGridDeliveries|Event Grid deliveries(preview) [Entregas de Event Grid (versión preliminar)]|Count|Total|El número de eventos de IoT Hub publicados en Event Grid. Use la dimensión de resultado para ver el número de solicitudes correctas e incorrectas. La dimensión EventType muestra el tipo de evento (https://aka.ms/ioteventgrid).|ResourceId,<br/>Result,<br/>EventType|
|EventGridLatency|Latencia de Event Grid (versión preliminar)|Milisegundos|Average|La latencia media (milisegundos) desde el momento en que se generó el evento de IoT Hub hasta el momento en que se publicó el evento en Event Grid. Este número es un promedio entre todos los tipos de evento. Use la dimensión EventType para ver la latencia de un tipo específico de evento.|ResourceId,<br/>EventType|
|d2c.twin.read.success|Lecturas gemelas correctas de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.read.failure|Lecturas gemelas con error de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|None|
|d2c.twin.read.size|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.update.success|Actualizaciones gemelas correctas de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.update.failure|Actualizaciones gemelas con error de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|None|
|d2c.twin.update.size|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|None|
|c2d.methods.success|Invocaciones correctas al método directo|Count|Total|El número de todas las llamadas correctas al método directo.|None|
|c2d.methods.failure|Invocaciones al método directo con error|Count|Total|El número de todas las llamadas al método directo con error.|None|
|c2d.methods.requestSize|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las solicitudes correctas de método directo.|None|
|c2d.methods.responseSize|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las respuestas correctas de método directo.|None|
|c2d.twin.read.success|Lecturas gemelas correctas del back-end|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.read.failure|Lecturas gemelas con error del back-end|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|None|
|c2d.twin.read.size|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.update.success|Actualizaciones gemelas correctas del back-end|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.update.failure|Actualizaciones gemelas con error del back-end|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|None|
|c2d.twin.update.size|Tamaño de las actualizaciones gemelas del back-end|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el back-end.|None|
|twinQueries.success|Consultas gemelas correctas|Count|Total|El recuento de todas las consultas gemelas correctas.|None|
|twinQueries.failure|Consultas gemelas con error|Count|Total|El recuento de todas las consultas gemelas con error.|None|
|twinQueries.resultSize|Tamaño de resultado de consultas gemelas|Bytes|Average|El valor medio, mínimo y máximo del tamaño del resultado de todas las consultas gemelas correctas.|None|
|jobs.createTwinUpdateJob.success|Creaciones correctas de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|None|
|jobs.createTwinUpdateJob.failure|Creaciones con error de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|None|
|jobs.createDirectMethodJob.success|Creaciones correctas de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|None|
|jobs.createDirectMethodJob.failure|Creaciones con error de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|None|
|jobs.listJobs.success|Llamadas correctas para enumerar trabajos|Count|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|None|
|jobs.listJobs.failure|Llamadas con error para enumerar trabajos|Count|Total|El recuento de todas las llamadas con error para enumerar trabajos.|None|
|jobs.cancelJob.success|Cancelaciones de trabajos correctas|Count|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|None|
|jobs.cancelJob.failure|Cancelaciones de trabajos con error|Count|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|None|
|jobs.queryJobs.success|Consultas de trabajo correctas|Count|Total|El recuento de todas las llamadas correctas para consultar trabajos.|None|
|jobs.queryJobs.failure|Consultas de trabajo con error|Count|Total|El recuento de todas las llamadas con error para consultar trabajos.|None|
|jobs.completed|Trabajos completados|Count|Total|El recuento de todos los trabajos completados.|None|
|jobs.failed|Trabajos con error|Count|Total|El recuento de todos los trabajos con error.|None|
|d2c.telemetry.ingress.sendThrottle|Número de errores de limitación|Count|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|None|
|dailyMessageQuotaUsed|Número total de mensajes usados|Count|Average|Número de mensajes totales usados hoy. Se trata de un valor acumulado que se restablece en cero a las 00:00 UTC cada día.|None|
|deviceDataUsage|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|None|
|deviceDataUsageV2|Uso total de datos del dispositivo (versión preliminar)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|None|
|totalDeviceCount|Total de dispositivos (versión preliminar)|Count|Average|Número de dispositivos registrados en IoT Hub|None|
|connectedDeviceCount|Dispositivos conectados (versión preliminar)|Count|Average|Número de dispositivos conectados a IoT Hub|None|
|configuraciones|Métricas de configuración|Count|Total|Métricas de las operaciones de configuración|None|

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha visto la información general sobre las métricas de IoT Hub, siga este vínculo para más información sobre la administración de IoT Hub de Azure:

* [Configuración de registros de diagnósticos](iot-hub-monitor-resource-health.md)

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Guía para desarrolladores de IoT Hub](iot-hub-devguide.md)

* [Implementación de IA en dispositivos perimetrales con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
