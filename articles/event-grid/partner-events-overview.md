---
title: 'Azure Event Grid: Eventos de asociado'
description: Envíe eventos de los asociados PaaS y SaaS de Event Grid de terceros directamente a los servicios de Azure con Azure Event Grid.
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 87d1d40b3696229344b0b5c20d06d9d993a514a4
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93102754"
---
# <a name="partner-events-in-azure-event-grid-preview"></a>Eventos de asociado en Azure Event Grid (versión preliminar)
La característica **Eventos de asociado** permite que un proveedor de SaaS de terceros publique eventos desde sus servicios para ponerlos a disposición de los consumidores que pueden suscribirse a esos eventos. Ofrece una experiencia propia a orígenes de eventos de terceros mediante la exposición de un tipo de [tema](concepts.md#topics), un **tema de asociado** , que los suscriptores usan para consumir eventos. También ofrece un modelo de publicación/suscripción limpio al separar las preocupaciones y la propiedad de los recursos que los suscriptores y los publicadores de eventos usan.

> [!NOTE]
> Si es la primera vez que usa Event Grid, consulte la [información general](overview.md), los [conceptos](concepts.md) y los [controladores de eventos](event-handlers.md).

## <a name="what-is-partner-events-to-a-publisher"></a>¿Qué representa la característica Eventos de asociado para un publicador?
La característica Eventos de asociado permite que los publicadores hagan las tareas siguientes:

- Incorporar sus orígenes de eventos a Event Grid.
- Crear un espacio de nombres (punto de conexión) en el que puedan publicar eventos.
- Cree temas de asociado en Azure que sean propiedad de los suscriptores y que estos los usen para consumir eventos.

## <a name="what-is-partner-events-to-a-subscriber"></a>¿Qué representa la característica Eventos de asociado para un suscriptor?
La característica Eventos de asociado permite que los suscriptores creen temas de asociado en Azure para consumir eventos de orígenes de eventos de terceros. El consumo de eventos se realiza mediante la creación de suscripciones de eventos que envían (insertan) eventos al controlador de eventos de un suscriptor.

## <a name="why-should-i-use-partner-events"></a>¿Por qué debería usar Eventos de asociado?
Puede que quiera usar Eventos de asociado si tiene uno o varios de los requisitos siguientes.

### <a name="for-publishers"></a>Para los publicadores

- Quiere un mecanismo que permita que sus eventos estén disponibles en Azure. Los usuarios pueden filtrar y enrutar esos eventos mediante el uso de temas de asociado y suscripciones de eventos que poseen y administran. Puede usar otros enfoques de integración, como [temas](custom-topics.md) y [dominios](event-domains.md). Pero estos enfoques no permitirían una separación limpia de la propiedad, administración y facturación de los recursos (temas de asociado) entre los publicadores y los suscriptores. Además, este enfoque proporciona una experiencia de usuario más intuitiva que facilita la detección y el uso de temas de asociado.
- Quiere publicar eventos en un punto de conexión único, el punto de conexión del espacio de nombres. También quiere poder filtrar esos eventos para que solo esté disponible un subconjunto de ellos. 
- Quiere tener visibilidad de las métricas relacionadas con los eventos publicados.
- Quiere usar el esquema de [CloudEvents 1.0](https://cloudevents.io/) para sus eventos.

### <a name="for-subscribers"></a>Para los suscriptores

- Quiere suscribirse a eventos de [publicadores de terceros](#available-third-party-event-publishers) y controlar los eventos mediante controladores de eventos que estén en Azure o en otro lugar.
- Quiere aprovechar el amplio conjunto de características de enrutamiento y [destinos/controladores de eventos](overview.md#event-handlers) para procesar eventos de orígenes de terceros. 
- Quiere implementar arquitecturas de acoplamiento flexible en las que el suscriptor/controlador de eventos no está al tanto de la existencia del agente de mensajes que se usa. 
- Necesita un mecanismo de entrega de inserción resistente con compatibilidad de envío y reintento y semántica de "al menos una vez".
- Quiere usar el esquema de [CloudEvents 1.0](https://cloudevents.io/) para sus eventos. 


## <a name="available-third-party-event-publishers"></a>Publicadores de eventos de terceros disponibles
Un publicador de eventos de terceros debe pasar por un [proceso de incorporación](partner-onboarding-overview.md) antes de que un suscriptor pueda empezar a consumir sus eventos. 

Si es suscriptor y quiere que un servicio de terceros exponga sus eventos a través de Event Grid. 

### <a name="auth0"></a>Auth0
**Auth0** es el primer publicador asociado disponible. Puede crear un [tema de asociado Auth0](auth0-overview.md) para conectar las cuentas de Auth0 y de Azure. Esta integración permite reaccionar a eventos de Auth0 en tiempo real, así como registrarlos y supervisarlos. Para probarla, consulte [Integración de Azure Event Grid con Auto0](auth0-how-to.md).

Si quiere que un servicio de terceros exponga sus eventos a través de Event Grid, envíe la idea en el [portal User Voice](https://feedback.azure.com/forums/909934-azure-event-grid).
 
## <a name="resources-managed-by-event-publishers"></a>Recursos administrados por publicadores de eventos
Los publicadores de eventos crean y administran los recursos siguientes:

### <a name="partner-registration"></a>Registro de asociado
Un registro contiene información general relacionada con un publicador. Define un tipo de tema de asociado que se muestra en Azure Portal como opción cuando los usuarios intentan crear un tema de asociado. Un publicador puede exponer uno o varios tipos de temas de asociado para ajustarse a las necesidades de sus suscriptores. Es decir, un publicador puede crear registros independientes (tipos de temas de asociado) para eventos de servicios distintos. Por ejemplo, para el servicio de recursos humanos (RR. HH.), el publicador puede definir un tema de asociado para eventos como la incorporación, promoción y salida de un empleado de la empresa. 

Tenga en cuenta los siguientes puntos:

- Solo están visibles los registros de asociado aprobados por Azure. 
- Los registros son globales, es decir, no están asociados a una región de Azure determinada.
- Un registro es un recurso opcional, pero se recomienda que, como publicador, cree un registro. Permite que los usuarios detecten los temas en la página **Creación de un tema de asociado** en [Azure Portal](https://portal.azure.com/#create/Microsoft.EventGridPartnerTopic). A continuación, el usuario puede seleccionar tipos de eventos (por ejemplo, la incorporación o la salida de un empleado de la empresa, entre otras) mientras crea suscripciones a eventos.

### <a name="namespace"></a>Espacio de nombres
Como los [temas personalizados](custom-topics.md) y los [dominios](event-domains.md), un espacio de nombres de asociado es un punto de conexión regional para publicar eventos. Es a través de los espacios de nombres que los publicadores crean y administran canales de eventos. Un espacio de nombres también funciona como el recurso de contenedor para los canales de eventos.

### <a name="event-channels"></a>Canales de eventos
Un canal de eventos es un recurso reflejado a un tema de asociado. Cuando un publicador crea un canal de eventos en la suscripción de Azure del publicador, también crea un tema de asociado en la suscripción de Azure de un suscriptor. Las operaciones realizadas en un canal de eventos (excepto GET) se aplicarán al tema de asociado del suscriptor correspondiente, incluida la eliminación. Sin embargo, las suscripciones y la entrega de eventos solo se pueden configurar en los temas de asociado.

## <a name="resources-managed-by-subscribers"></a>Recursos administrados por los suscriptores 
Los suscriptores pueden usar temas de asociado definidos por un publicador y este es el único tipo de recurso que pueden ver y administrar. Una vez creado un tema de asociado, un suscriptor puede crear suscripciones de eventos que definan las reglas de filtro para [destinos/controladores de eventos](overview.md#event-handlers). En el caso de los suscriptores, un tema de asociado y sus suscripciones de eventos asociadas proporcionan las mismas funcionalidades enriquecidas que los [temas personalizados](custom-topics.md) y sus suscripciones relacionadas sí lo hacen, con una diferencia importante: los temas de asociado solo admiten el [esquema de CloudEvents 1.0](cloudevents-schema.md), que ofrece un conjunto de funcionalidades más completo que otros esquemas compatibles.

## <a name="pricing"></a>Precios
Los temas de asociado se cobran según el número de operaciones realizadas al usar Event Grid. Para más información sobre todos los tipos de operaciones que se usan como base para la facturación e información detallada de precios, consulte [Precios de Event Grid](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="limits"></a>Límites
Consulte los [límites del servicio Event Grid](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits) para información detallada sobre los límites establecidos para los temas de asociado.


## <a name="next-steps"></a>Pasos siguientes

- [Tema del asociado Auth0](auth0-overview.md)
- [Cómo usar el tema del asociado Auth0](auth0-how-to.md)
- [Convertirse en asociado de Event Grid](partner-onboarding-overview.md)