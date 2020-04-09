---
title: Descripción de la dirección IP de la instancia de IoT Hub Device Provisioning Service (DPS) | Microsoft Docs
description: Aprenda a consultar la dirección de su instancia de IoT Hub Device Provisioning Service (DPS) y sus propiedades. La dirección IP de su instancia de DPS puede cambiar durante determinados escenarios, como la recuperación ante desastres o la conmutación por error regional.
author: wesmc7777
ms.author: wesmc
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: f6afd5c4cc5aa0215f943979ae91389b39d449f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79237456"
---
# <a name="iot-hub-dps-ip-addresses"></a>Direcciones IP de IoT Hub DPS

Los prefijos de dirección IP de los puntos de conexión de una instancia de IoT Hub Device Provisioning Service (DPS) se publican periódicamente con la [etiqueta de servicio](../virtual-network/service-tags-overview.md) _AzureIoTHub_. Puede usar estos prefijos de dirección IP para controlar la conectividad entre una instancia de IoT Hub DPS y los dispositivos o recursos de red para implementar diversos objetivos de aislamiento de red:

| Objetivo | Enfoque |
|------|----------|
| Comprobar que los dispositivos y servicios solo se comunican con puntos de conexión de IoT Hub DPS | Use la etiqueta de servicio _AzureIoTHub_ para detectar instancias de IoT Hub DPS. Configure las reglas de permiso en la configuración del firewall de los dispositivos y los servicios de esos prefijos de direcciones IP en consecuencia. Configure reglas para derivar el tráfico a otras direcciones IP de destino con las que no quiera que se comuniquen los dispositivos o servicios. |
| Comprobar que el punto de conexión de IoT Hub DPS solo recibe conexiones de sus dispositivos y recursos de red | Use la [característica de filtro de IP](iot-dps-ip-filtering.md) de IoT DPS para crear reglas de filtro para las API del dispositivo y del servicio DPS. Estas reglas de filtro se pueden usar para permitir conexiones solo desde las direcciones IP de sus dispositivos y recursos de red (consulte la sección de [limitaciones](#limitations-and-workarounds)). | 




## <a name="best-practices"></a>Procedimientos recomendados

* Al agregar reglas de permiso en la configuración del firewall de los dispositivos, es mejor proporcionar [puertos específicos utilizados por los protocolos aplicables](../iot-hub/iot-hub-devguide-protocols.md#port-numbers).

* Los prefijos de dirección IP de IoT Hub DPS están sujetos a cambios. Estos cambios se publican periódicamente mediante etiquetas de servicio antes de entrar en vigor. Por lo tanto, es importante que desarrolle procesos para recuperar y usar con regularidad las etiquetas de servicio más recientes. Este proceso se puede automatizar mediante la [API de detección de etiquetas de servicio](../virtual-network/service-tags-overview.md#service-tags-on-premises). La API de detección de etiquetas de servicio todavía está en versión preliminar y, en algunos casos, es posible que no genere la lista completa de etiquetas y direcciones IP. Hasta que la API de detección esté disponible con carácter general, considere la posibilidad de usar las [etiquetas de servicio en formato JSON descargable](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files). 

* Use la etiqueta *AzureIoTHub.[nombre de la región]* para identificar los prefijos de dirección IP que se usan en los puntos de conexión de DPS de una región determinada. Para tener en cuenta la recuperación ante desastres de un centro de datos o la [conmutación por error regional](../iot-hub/iot-hub-ha-dr.md), asegúrese de que también esté habilitada la conectividad a los prefijos de dirección IP de la región del par de replicación geográfica de su instancia de DPS.

* La configuración de reglas de firewall para una instancia de DPS puede bloquear la conectividad necesaria para ejecutar la CLI de Azure y comandos de PowerShell en esta. Para evitar estos problemas de conectividad, puede agregar reglas de permiso para los prefijos de dirección IP de los clientes para volver a habilitar la CLI o para que los clientes de PowerShell se comuniquen con su instancia de DPS.  


## <a name="limitations-and-workarounds"></a>Limitaciones y soluciones alternativas

* La característica de filtro de IP de DPS tiene un límite de 100 reglas. Este límite se puede aumentar mediante solicitudes a través del servicio de soporte al cliente de Azure. 

* Las [reglas de filtrado de IP](iot-dps-ip-filtering.md) configuradas solo se aplican en los puntos de conexión de DPS y no en los puntos de conexión de IoT Hub vinculados. El filtrado de IP para las instancias de IoT Hub vinculadas debe configurarse por separado. Para más información, consulte [Reglas de filtrado de IP de IoT Hub](../iot-hub/iot-hub-ip-filtering.md).

## <a name="support-for-ipv6"></a>Compatibilidad con IPv6 

Actualmente, IPv6 no se admite en IoT Hub ni DPS.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de las configuraciones de direcciones IP con DPS, consulte:

* [Configuración de filtrado de IP](iot-dps-ip-filtering.md)
