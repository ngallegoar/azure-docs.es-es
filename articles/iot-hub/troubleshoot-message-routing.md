---
title: Solución de problemas de enrutamiento de mensajes de Azure IoT
description: Cómo solucionar problemas de enrutamiento de mensajes de Azure IoT
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: asrastog
ms.openlocfilehash: 84be56ae372f8a902b12c06f9ce93c1f7210dc5b
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92150593"
---
# <a name="troubleshooting-message-routing"></a>Solución de problemas de enrutamiento de mensajes

En este artículo se proporcionan instrucciones para la supervisión y solución de problemas comunes y la resolución de [enrutamiento de mensajes de IoT Hub](iot-hub-devguide-messages-d2c.md). 

## <a name="monitoring-message-routing"></a>Supervisión del enrutamiento de mensajes

[Métricas de IoT Hub](iot-hub-metrics.md) enumera todas las métricas que están habilitadas de forma predeterminada para el centro de IoT. Se recomienda supervisar las métricas relacionadas con el enrutamiento de mensajes y los puntos de conexión para proporcionarle una visión general de los mensajes enviados. Active también los [registros de diagnóstico](iot-hub-monitor-resource-health.md) en la configuración de diagnóstico de Azure Monitor para realizar un seguimiento de las operaciones de **rutas**. Estos registros de diagnóstico se pueden enviar a los registros de Azure Monitor, Event Hubs o Azure Storage para su procesamiento personalizado. Aprenda a [configurar y usar métricas y registros de diagnóstico con una instancia de IoT Hub](tutorial-use-metrics-and-diags.md).

También se recomienda habilitar la [ruta de reserva](iot-hub-devguide-messages-d2c.md#fallback-route) si quiere mantener los mensajes que no coinciden con la consulta en ninguna de las rutas. Estos se pueden conservar en el [punto de conexión integrado](iot-hub-devguide-messages-read-builtin.md) durante la cantidad de días de retención configurados. 

## <a name="top-issues"></a>Principales problemas

A continuación se muestran los problemas más comunes que se observan con el enrutamiento de mensajes. Para iniciar la solución de problemas, haga clic en el problema para obtener pasos detallados.

* [Los mensajes de mis dispositivos no se están redirigiendo según lo esperado](#messages-from-my-devices-are-not-being-routed-as-expected)
* [Repentinamente, he dejado de recibir mensajes en el punto de conexión de Event Hubs integrado](#i-suddenly-stopped-getting-messages-at-the-built-in-endpoint)

### <a name="messages-from-my-devices-are-not-being-routed-as-expected"></a>Los mensajes de mis dispositivos no se están redirigiendo según lo esperado

Para solucionar este problema, analice lo siguiente:

#### <a name="the-routing-metrics-for-this-endpoint"></a>Las métricas de enrutamiento para este punto de conexión
Todas las [métricas de IoT Hub](iot-hub-devguide-endpoints.md) relacionadas con el enrutamiento incluyen el término *enrutamiento*. Puede combinar información de varias métricas para identificar la causa principal de los problemas. Por ejemplo, use la métrica **Intentos de entrega de enrutamiento** para identificar el número de mensajes que se entregaron a un punto de conexión o que se eliminaron por no coincidir con las consultas en ninguna de las rutas y porque la ruta de reserva estaba deshabilitada. Compruebe la métrica **Latencia de enrutamiento** para observar si la latencia de la entrega de mensajes es constante o va en aumento. Una latencia cada vez mayor puede indicar un problema con un punto de conexión específico y se recomienda comprobar [el estado del punto de conexión](#the-health-of-the-endpoint). Estas métricas de enrutamiento también tienen [dimensiones](iot-hub-metrics.md#dimensions) que proporcionan detalles sobre la métrica, como el tipo de punto de conexión, el nombre del punto de conexión específico y un motivo por el que no se entregó el mensaje.

#### <a name="the-diagnostic-logs-for-any-operational-issues"></a>Registros de diagnóstico para cualquier problema operativo 
Observe los **registros de diagnóstico** de las [rutas](iot-hub-monitor-resource-health.md#routes) para obtener más información sobre las [operaciones](#operation-names) de enrutamiento y de punto de conexión o identificar los errores y el [código de error](#common-error-codes) correspondiente para comprender mejor el problema. Por ejemplo, el nombre de la operación **RouteEvaluationError** en el registro indica que no se pudo evaluar la ruta debido a un problema con el formato de mensaje. Use las sugerencias proporcionadas para los [nombres de operaciones](#operation-names) específicos para mitigar el problema. Cuando se registra un evento como un error, el registro también proporcionará más información sobre el motivo del error en la evaluación. Por ejemplo, si el nombre de la operación es **EndpointUnhealthy**, un [Código de error](#common-error-codes) 403004 indica que el punto de conexión se ha quedado sin espacio.

#### <a name="the-health-of-the-endpoint"></a>Mantenimiento del punto de conexión
Use la API REST [Get Endpoint Health](/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) para obtener el [estado de mantenimiento](iot-hub-devguide-endpoints.md#custom-endpoints) de los puntos de conexión. La API *Get Endpoint Health* también proporciona información sobre la última vez que un mensaje se envió correctamente al punto de conexión, el [último error conocido](#last-known-errors-for-iot-hub-routing-endpoints), la última hora de error conocida y la última vez que se realizó un intento de envío para este punto de conexión. Use la posible mitigación que proporciona el [último error conocido](#last-known-errors-for-iot-hub-routing-endpoints) específico.

### <a name="i-suddenly-stopped-getting-messages-at-the-built-in-endpoint"></a>Repentinamente, he dejado de recibir mensajes en el punto de conexión integrado

Para solucionar este problema, analice lo siguiente:

#### <a name="was-a-new-route-created"></a>¿Se ha creado una nueva ruta?
Una vez que se crea una ruta, los datos dejan de fluir al punto de conexión integrado, a menos que se cree una ruta a ese punto de conexión. Configure una ruta al punto de conexión de *eventos* para asegurarse de que los mensajes continúen fluyendo hasta el punto de conexión integrado si se agrega una nueva ruta. 

#### <a name="was-the-fallback-route-disabled"></a>¿Se ha deshabilitado la ruta de reserva?
La ruta de reserva envía todos los mensajes que no cumplen las condiciones de la consulta en cualquiera de las rutas existentes al punto de conexión de [Event Hubs integrado](iot-hub-devguide-messages-read-builtin.md) (mensajes y eventos), que es compatible con [Event Hubs](../event-hubs/index.yml). Si el enrutamiento de mensajes está activado, puede habilitar la funcionalidad de ruta de reserva. Si no hay ninguna ruta al punto de conexión integrado y está habilitada una ruta de reserva, solo se enviarán al punto de conexión integrado los mensajes que no coinciden con las condiciones de la consulta sobre rutas. Además, si se eliminan todas las rutas existentes, se debe habilitar la ruta de reserva para recibir todos los datos en el punto de conexión integrado.

Puede habilitar o deshabilitar la ruta de reserva en Azure Portal -> hoja Enrutamiento de mensajes. También puede usar Azure Resource Manager para [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) para usar un punto de conexión personalizado para la ruta de reserva.

## <a name="last-known-errors-for-iot-hub-routing-endpoints"></a>Últimos errores conocidos de los puntos de conexión de enrutamiento de IoT Hub

<a id="last-known-errors"></a>
[!INCLUDE [iot-hub-include-last-known-errors](../../includes/iot-hub-include-last-known-errors.md)]

## <a name="routes-diagnostic-logs"></a>Enrute los registros de diagnóstico

A continuación, se muestran los nombres de las operaciones y los códigos de error registrados en los [registros de diagnóstico](iot-hub-monitor-resource-health.md#routes).

<a id="diagnostics-operation-names"></a>
### <a name="operation-names"></a>Nombres de operación

[!INCLUDE [iot-hub-diagnostics-operation-names](../../includes/iot-hub-diagnostics-operation-names.md)]

<a id="diagnostics-error-codes"></a>
### <a name="common-error-codes"></a>Códigos comunes de error

[!INCLUDE [iot-hub-diagnostics-error-codes](../../includes/iot-hub-diagnostics-error-codes.md)]

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de Soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.