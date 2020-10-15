---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d03579f704879bd8d012bb0bb326659d1f778dee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84793296"
---
[Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) en la API REST proporciona el estado de mantenimiento de los puntos de conexión, así como el último error conocido, para identificar el motivo por el que un punto de conexión no es correcto. En la tabla siguiente se enumeran los errores más comunes.

|Último error conocido|Descripción/cuándo se produce|Posible mitigación|
|-----|-----|-----|
|Transitorio|Se ha producido un error transitorio e IoT Hub reintentará la operación.|Observe los [registros de diagnóstico](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes) de las rutas.|
|InternalError|Se produjo un error al entregar un mensaje a un punto de conexión.|Se trata de una excepción interna, pero también observe los [registros de diagnóstico](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes) de las rutas.|
|No autorizado|IoT Hub no está autorizado para enviar mensajes al punto de conexión especificado.|Compruebe que la cadena de conexión está actualizada para el punto de conexión. Si ha cambiado, considere la posibilidad de realizar una actualización en IoT Hub. Si el punto de conexión usa identidad administrada, compruebe que la entidad de seguridad de IoT Hub tenga los permisos necesarios en el destino.|
|Limitado|IoT Hub se está limitando al escribir mensajes en el punto de conexión.|Revise la limitación para el punto de conexión afectado. Modifique las configuraciones del punto de conexión para escalar verticalmente si es necesario.|
|Tiempo de espera|Tiempo de espera de la operación.|Vuelva a intentar la operación.|
|No encontrado|El recurso de destino no existe.|Asegúrese de que el recurso de destino existe.|
|No se ha encontrado el contenedor|El contenedor de almacenamiento no existe.|Asegúrese de que el contenedor de almacenamiento existe.|
|Contenedor deshabilitado|El contenedor de almacenamiento está deshabilitado.|Asegúrese de que el contenedor de almacenamiento está habilitado.|
|MaxMessageSizeExceeded|El enrutamiento de mensajes tiene un límite de tamaño de mensaje de 256 KB. El tamaño del mensaje que se está enrutando superó este límite.|Compruebe si el tamaño del mensaje se puede reducir con menos propiedades de la aplicación o menos enriquecidas.|
|PartitioningAndDuplicateDetectionNotSupported|Es posible que Service Bus no tenga habilitada la detección de duplicados.|Deshabilite la detección de duplicados de Service Bus o considere la posibilidad de usar una entidad sin detección de duplicados.|
|SessionfulEntityNotSupported|Es posible que Service Bus no tenga habilitadas las sesiones.|Deshabilite la sesión de Service Bus o considere la posibilidad de usar una entidad sin sesiones.|
|NoMatchingSubscriptionsForMessage|No hay ninguna suscripción para escribir el mensaje en el tema de Service Bus.|Cree una suscripción para los mensajes de IoT Hub a los que se va a redirigir.|
|EndpointExternallyDisabled|El punto de conexión no está en un estado activo, por lo IoT Hub puede enviarle mensajes.|Habilite el punto de conexión para volver al estado activo.|
|DeviceMaximumQueueDepthExceeded|Se ha alcanzado el límite de tamaño de Service Bus.|Considere la posibilidad de quitar los mensajes de Event Hubs de destino para permitir que se ingieran nuevos mensajes en Event Hubs.|