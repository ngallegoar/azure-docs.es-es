---
title: Guía de solución de problemas para Azure Event Hubs | Microsoft Docs
description: En este artículo se proporciona una lista de las excepciones de mensajería y acciones sugeridas de Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: ab3cbdf938409f4eacffa10ae5cb20f8c36b5856
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124676"
---
# <a name="azure-event-hubs---troubleshooting-guide"></a>Azure Event Hubs: guía de solución de problemas
En este artículo se proporcionan sugerencias y recomendaciones para la solución de algunos problemas que pueden aparecer al usar Azure Event Hubs.

## <a name="connectivity-certificate-or-timeout-issues"></a>Problemas de conectividad, certificados o tiempo de espera
Los pasos siguientes pueden ayudarle a solucionar problemas de conectividad, certificados y tiempo de espera para todos los servicios de *.servicebus.windows.net. 

- Desplácese a `https://<yournamespacename>.servicebus.windows.net/` o use [wget](https://www.gnu.org/software/wget/) ir allí. Le ayudará a comprobar si tiene problemas de cadena de certificados (lo más común al usar el SDK de Java), filtrado IP o red virtual.

    Un ejemplo de mensaje correcto:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Un ejemplo de mensaje de error:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Ejecute el siguiente comando para comprobar si hay algún puerto bloqueado en el firewall. Los puertos usados son 443 (HTTPS), 5671 (AMQP) y 9093 (Kafka). En función de la biblioteca que use, también se usan otros puertos. Este es el comando de ejemplo que comprueba si el puerto 5671 está bloqueado.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    En Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Si hay problemas de conectividad intermitentes, ejecute el siguiente comando para comprobar si hay paquetes descartados. Este comando intentará establecer 25 conexiones TCP diferentes cada segundo con el servicio. A continuación, puede comprobar cuántas de ellas se han realizado correctamente y cuántas han fallado y, además, ver la latencia de conexión TCP. Puede descargar la herramienta `psping` desde [aquí](/sysinternals/downloads/psping).

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    Puede usar comandos equivalentes si utiliza otras herramientas como `tnc`, `ping`, etc. 
- Realice un seguimiento de red si los pasos anteriores no ayudan y analícelo con herramientas como [Wireshark](https://www.wireshark.org/). Si lo necesita, póngase en contacto con el [soporte técnico de Microsoft](https://support.microsoft.com/). 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Problemas que se pueden producir con las actualizaciones o reinicios de servicios
Las actualizaciones y los reinicios de servicios back-end pueden tener el siguiente impacto en las aplicaciones:

- Puede que las solicitudes se limiten momentáneamente.
- Puede haber una caída en la llegada de mensajes o solicitudes entrantes.
- El archivo de registro puede contener mensajes de error.
- Puede que las aplicaciones se desconecten del servicio durante unos segundos.

Si el código de aplicación usa SDK, la directiva de reintento ya está integrada y activa. La aplicación se volverá a conectar sin un impacto significativo en la aplicación o el flujo de trabajo.


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Event Hubs, visite los vínculos siguientes:

* [Información general de Event Hubs](event-hubs-what-is-event-hubs.md)
* [Creación de un Centro de eventos](event-hubs-create.md)
* [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)
