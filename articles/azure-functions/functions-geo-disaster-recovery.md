---
title: Alta disponibilidad y recuperación ante desastres geográfica para Azure Functions
description: Uso de regiones geográficas para la redundancia y conmutar por error en Azure Functions.
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.topic: conceptual
ms.date: 08/29/2019
ms.openlocfilehash: 446a76b823ce3592a83d0c2f898041951361b47e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86506288"
---
# <a name="azure-functions-geo-disaster-recovery"></a>Recuperación ante desastres geográfica de Azure Functions

Cuando regiones o centros de datos enteros de Azure experimentan tiempo de inactividad, es esencial que el procesamiento de datos continúe en una región diferente.  En este artículo se explican algunas de las estrategias que puede usar para implementar funciones que permitan la recuperación ante desastres.

## <a name="basic-concepts"></a>Conceptos básicos

Azure Functions se ejecuta en una región específica.  Para obtener una mayor disponibilidad, puede implementar las mismas funciones en varias regiones.  En este caso, las funciones se pueden ejecutar en el patrón *activo/activo* o *activo/pasivo*.  

* Activo/activo. Ambas regiones están activas y recibiendo eventos (duplicados o de forma rotativa). Se recomienda el patrón activo/activo para las funciones HTTPS en combinación con Azure Front Door.
* Activo/pasivo. Una región está activa y recibiendo eventos, mientras que una secundaria está inactiva.  Cuando se requiere la conmutación por error, la región secundaria se activa y se encarga del procesamiento.  Se recomienda para funciones no HTTP como Service Bus y Event Hubs.

Para más información sobre las implementaciones en varias regiones, lea cómo [ejecutar aplicaciones en varias regiones](/azure/architecture/reference-architectures/app-service-web-app/multi-region).

## <a name="activeactive-for-https-functions"></a>Patrón activo/activo para funciones HTTPS

Para lograr implementaciones activo/activo de funciones, se necesitan algunos componentes que puedan coordinar los eventos entre ambas regiones.  En el caso de las funciones HTTPS, esta coordinación se logra mediante [Azure Front Door](../frontdoor/front-door-overview.md).  Azure Front Door puede enrutar y enviar por turnos las solicitudes HTTPS entre varias funciones regionales.  También comprueba periódicamente el estado de cada punto de conexión.  Si una función regional deja de responder a las comprobaciones de estado, Azure Front Door la saca de la rotación y solo reenvía el tráfico a las funciones correctas.  

![Arquitectura de Azure Front Door y Azure Functions](media/functions-geo-dr/front-door.png)  

## <a name="activeactive-for-non-https-functions"></a>Patrón activo/activo para funciones no HTTPS

Aunque puede lograr implementaciones activo/activo de funciones no HTTPS,  debe tener en cuenta cómo interactuarán las dos regiones o se coordinarán entre sí.  Si implementó la misma aplicación de función en dos regiones y cada una de ellas se desencadena en la misma cola de Service Bus, estas actuarán como consumidores rivales al quitar de la cola esa cola.  Aunque esto significa que solo una de las instancias procesa el mensaje, también significa que todavía hay un único punto de error en la instancia única de Service Bus.  Si implementa dos colas de Service Bus (una en una región primaria y otra en una región secundaria) y las dos aplicaciones de funciones apuntan a la cola de su región, la dificultad reside ahora en cómo se distribuyen los mensajes de la cola entre las dos regiones.  A menudo, esto significa que cada publicador intenta publicar un mensaje en *ambas* regiones, y ambas aplicaciones de función activas procesan cada mensaje.  Aunque esta situación crea un patrón activo/activo, presenta otras dificultades relacionadas con la duplicación del proceso y cuándo o cómo se consolidan los datos.  Por estos motivos, se recomienda usar el patrón activo/pasivo para los desencadenadores no HTTPS.

## <a name="activepassive-for-non-https-functions"></a>Patrón activo/pasivo para funciones no HTTPS

El patrón activo/pasivo proporciona una manera de que solo una sola función procese cada mensaje, pero ofrece un mecanismo para conmutar por error a una región secundaria en caso de desastre.  Azure Functions trabaja junto con la [recuperación geográfica de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md) y la [recuperación geográfica de Azure Event Hubs](../event-hubs/event-hubs-geo-dr.md).

Si usamos los desencadenadores de Azure Event Hubs como ejemplo, el patrón activo/pasivo implicaría lo siguiente:

* Azure Event Hub implementado en una región primaria y una secundaria.
* Recuperación ante desastres geográfica habilitada para emparejar el centro de eventos principal y el secundario.  Esto también crea un "alias" que puede usar para conectarse a los centros de eventos y cambiar del principal al secundario sin cambiar la información de conexión.
* Aplicaciones de función implementadas en una región primaria y una secundaria.
* Las aplicaciones de función se desencadenan en la cadena de conexión *directa* (sin alias) de su centro de eventos correspondiente. 
* Los publicadores del centro de eventos deben publicar en la cadena de conexión de alias. 

![Arquitectura de ejemplo activo-pasivo](media/functions-geo-dr/active-passive.png)

Antes de conmutar por error, los publicadores que envían al alias compartido enrutarán al centro de eventos principal.  La aplicación de función principal escucha exclusivamente al centro de eventos principal.  La aplicación de función secundaria será pasiva y estará inactiva.  En cuanto se inicia la conmutación por error, los publicadores que envíen al alias compartido ahora se enrutarán al centro de eventos secundario.  La aplicación de función secundaria pasará a estar activa y comenzará a desencadenarse automáticamente.  La conmutación por error efectiva a una región secundaria se puede controlar íntegramente desde el centro de eventos, donde las funciones solo se activan cuando lo hace el centro de eventos correspondiente.

Más información y consideraciones sobre la conmutación por error con [Service Bus](../service-bus-messaging/service-bus-geo-dr.md) y [Event Hubs](../event-hubs/event-hubs-geo-dr.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una instancia de Azure Front Door](../frontdoor/quickstart-create-front-door.md)
* [Consideraciones sobre la conmutación por error de Event Hubs](../event-hubs/event-hubs-geo-dr.md#considerations)
