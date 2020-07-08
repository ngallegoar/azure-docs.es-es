---
title: Dominios de eventos de Azure Event Grid
description: En este artículo se describe cómo usar dominios de eventos para administrar el flujo de eventos personalizados a sus diversas organizaciones empresariales, clientes o aplicaciones.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 02529ba770e636021cf9cec4ed555247e1c63d8c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86114370"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Dominios de eventos para administrar temas de Event Grid

En este artículo se describe cómo usar dominios de eventos para administrar el flujo de eventos personalizados a sus diversas organizaciones empresariales, clientes o aplicaciones. Use dominios de eventos para:

* Administrar arquitecturas de eventos multiinquilino a escala.
* Administrar la autenticación y la autorización.
* Crear particiones de los temas sin tener que administrar cada uno individualmente.
* Evitar la publicación individual en cada uno de los puntos de conexión del tema.

## <a name="event-domain-overview"></a>Información general sobre dominios de eventos

Un dominio de eventos es una herramienta de administración para un gran número de temas de Event Grid relacionados con la misma aplicación. Se puede considerar un metatema que puede contener miles de temas individuales.

Los dominios de eventos ponen a su disposición la misma arquitectura que usan los servicios de Azure (por ejemplo, Storage e IoT Hub) para publicar sus eventos. Le permiten publicar eventos en miles de temas. Los dominios también proporcionan control de autenticación y autorización sobre cada tema para que pueda dividir los inquilinos.

### <a name="example-use-case"></a>Ejemplo de caso de uso

Los dominios de eventos se explican más fácilmente con un ejemplo. Supongamos que dirige Contoso Construction Machinery, donde se encarga de fabricar tractores, equipos de excavación y otra maquinaria pesada. Entre las tareas de la empresa se incluye el envío de información en tiempo real a los clientes sobre el mantenimiento de los equipos, el estado de los sistemas y las actualizaciones de contratos. Toda esta información llega a diferentes puntos de conexión, entre los que están su aplicación, los puntos de conexión de los clientes y otras infraestructuras que estos han configurado.

Los dominios de eventos le permiten modelar Contoso Construction Machinery como una única entidad de creación de eventos. Cada uno de los clientes se representa como un tema dentro del dominio. La autenticación y la autorización se controlan mediante Azure Active Directory. Cada uno de los clientes puede suscribirse a su tema para recibir eventos relacionados. El acceso administrado a través del dominio de eventos garantiza que solo pueden acceder a su tema.

También le ofrece un punto de conexión único, en el que puede publicar todos sus eventos de cliente. Event Grid se encarga de asegurarse de que cada tema solo esté al tanto de los eventos asignados a su inquilino.

![Ejemplo de Contoso Construction](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Administración de acceso

Con un dominio, obtendrá un mayor control de la autenticación y la autorización sobre cada tema a través del Control de acceso basado en rol (RBAC) de Azure. Estos roles pueden usarse para restringir cada arrendatario de la aplicación únicamente a los temas a los que desea concederle acceso.

RBAC en dominios de eventos funciona del mismo modo que lo hace el [control de acceso administrado](security-authorization.md) en el resto de Event Grid y Azure. Use RBAC para crear y aplicar definiciones de roles personalizados en dominios de eventos.

### <a name="built-in-roles"></a>Roles integrados

Event Grid tiene dos definiciones de roles integradas que facilitan el trabajo de RBAC con dominios de eventos. Estos roles son los de **colaborador de EventGrid EventSubscription (versión preliminar)** y **lector de EventGrid EventSubscription (versión preliminar)** . Estos roles se asignan a los usuarios que necesitan suscribirse a temas en el dominio de eventos. El ámbito de la asignación de roles se limita a solo el tema al que necesitan suscribirse los usuarios.

Para información sobre estos roles, consulte [Roles integrados para Event Grid](security-authorization.md#built-in-roles).

## <a name="subscribing-to-topics"></a>Suscribirse a temas

Suscribirse a eventos de un tema dentro de un dominio de eventos es lo mismo que [crear una suscripción a eventos en un tema personalizado](./custom-event-quickstart.md) o suscribirse a un evento desde un servicio de Azure.

### <a name="domain-scope-subscriptions"></a>Suscripciones del ámbito de dominio

Los dominios de eventos también permiten suscripciones de ámbito de dominio. Una suscripción a eventos en un dominio de eventos recibirá todos los eventos enviados al dominio, independientemente del tema al que se envían los eventos. Las suscripciones del ámbito de dominio pueden ser útiles para la administración y la auditoría.

## <a name="publishing-to-an-event-domain"></a>Publicación en un dominio de eventos

Cuando se crea un dominio de eventos, se le asigna un punto de conexión de publicación igual que si hubiera creado un tema de Event Grid. 

Para publicar eventos en cualquier tema en un dominio de eventos, inserte los eventos en el punto de conexión del dominio como [haría para un tema personalizado](./post-to-custom-topic.md). La única diferencia es que debe especificar el tema en el que quiere que se entregue el evento.

Por ejemplo, la siguiente matriz de eventos de publicación enviará eventos con `"id": "1111"` al tema `foo`, mientras que el evento con `"id": "2222"` se enviaría al tema `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Los dominios de eventos controlan la publicación de temas por usted. En lugar de publicar los eventos en cada tema que administra individualmente, puede publicar todos los eventos en el punto de conexión del dominio. Event Grid garantiza que cada evento se envía al tema correcto.

## <a name="limits-and-quotas"></a>Límites y cuotas
Estos son los límites y cuotas relacionados con los dominios de eventos:

- 100 000 temas por dominio de eventos 
- 100 dominios de eventos por suscripción de Azure 
- 500 suscripciones a eventos por tema en un dominio de eventos
- 50 suscripciones del ámbito de dominio 
- 5000 eventos por tasa de ingesta por segundo (en un dominio)

Si estos límites no son los adecuados para usted, póngase en contacto con el equipo del producto. Para ello, abra una incidencia de soporte técnico o envíe un correo electrónico a [askgrid@microsoft.com](mailto:askgrid@microsoft.com). 

## <a name="pricing"></a>Precios
Los dominios de eventos usan el mismo [precio por operaciones](https://azure.microsoft.com/pricing/details/event-grid/) que el resto de características de Event Grid.

Las operaciones funcionan igual en dominios de eventos que en temas personalizados. Cada entrada de un evento a un dominio de eventos es una operación, y cada intento de entrega de un evento es una operación.

## <a name="next-steps"></a>Pasos siguientes

* Para información acerca de cómo configurar dominios de eventos, crear temas, crear suscripciones a eventos y publicar eventos, consulte [Administración de dominios de eventos](./how-to-event-domains.md).
