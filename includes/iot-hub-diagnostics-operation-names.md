---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 04/28/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 362c13771e7382ead1ba5aebd99a69fd86cd718c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84793276"
---
<!-- operation names for the diag logs for IoT Hub -->

|Nombre de la operación|Nivel|Descripción|
|------------- |-----|-----------|
|UndefinedRouteEvaluation|Información|El mensaje no se puede evaluar con una condición dada. Por ejemplo, si una propiedad de la condición de consulta de ruta está ausente en el mensaje. Más información sobre la [sintaxis de consulta de enrutamiento](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).|
|RouteEvaluationError|Error|Se produjo un error al evaluar el mensaje debido a un problema con el formato de mensaje. Por ejemplo, este error se registrará si no se especifica la codificación de contenido o si el tipo de contenido no es válido en el mensaje. Se deben establecer en las [propiedades del sistema](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties).|
|DroppedMessage|Error|El mensaje se ha quitado y no se ha enrutado. Esto puede deberse a motivos como que el mensaje no coincidía con ninguna consulta de enrutamiento o punto de conexión inactivo y no se pudo entregar el mensaje después de varios reintentos. Se recomienda obtener más detalles sobre el punto de conexión mediante la API REST [get endpoint health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointUnhealthy|Error|El punto de conexión no ha aceptado mensajes de IoT Hub e IoT Hub está intentando reenviar los mensajes. Se recomienda observar el último error conocido a través de la API REST [get endpoint health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointDead|Error|El punto de conexión no ha aceptado mensajes de IoT Hub durante más de una hora. Se recomienda observar el último error conocido a través de la API REST [get endpoint health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointHealthy|Información|El punto de conexión es correcto y recibe mensajes de IoT Hub. Este mensaje no se registra continuamente, sino que solo se registra cuando el punto de conexión vuelve a tener un estado correcto. Este mensaje significa que IoT Hub no pudo enviar mensajes al punto de conexión, pero el punto de conexión ahora es correcto.|
|OrphanedMessage|Información|El mensaje no coincide con ninguna ruta.|
|InvalidMessage|Error|El mensaje no es válido debido a la incompatibilidad con el punto de conexión. Se recomienda comprobar las configuraciones del punto de conexión.|


Las operaciones *UndefinedRouteEvaluation*, *RouteEvaluationError* y *OrphanedMessage* se limitan y se registran en un máximo de una vez por minuto por IoT Hub.