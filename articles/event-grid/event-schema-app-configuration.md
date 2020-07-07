---
title: Azure App Configuration como origen de Event Grid
description: En este artículo se describe cómo utilizar Azure App Configuration como origen de eventos de Event Grid. Proporciona el esquema y los vínculos a los artículos de procedimientos y tutorial.
services: event-grid
author: femila
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: femila
ms.openlocfilehash: e233b5d27df3e25c2d7c1464aea9a1e80dfbffb0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84553153"
---
# <a name="azure-app-configuration-as-an-event-grid-source"></a>Azure App Configuration como origen de Event Grid
En este artículo se proporcionan las propiedades y los esquemas de los eventos de Azure App Configuration. Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md). También proporciona una lista de inicios rápidos y tutoriales para usar Azure App Configuration como origen de eventos.

## <a name="event-grid-event-schema"></a>Esquema de eventos de Event Grid

### <a name="available-event-types"></a>Tipos de eventos disponibles

La configuración de Azure App Configuration emite los siguientes tipos de evento:

| Tipo de evento | Descripción |
| ---------- | ----------- |
| Microsoft.AppConfiguration.KeyValueModified | Se genera cuando se crea o se sustituye un valor de clave. |
| Microsoft.AppConfiguration.KeyValueDeleted | Se genera cuando se elimina un valor de clave. |

### <a name="example-event"></a>Evento de ejemplo

En el siguiente ejemplo se muestra el esquema de un evento modificado de valor clave: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

El esquema para un evento de eliminación de valor clave es similar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
### <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| topic | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| subject | string | Ruta al asunto del evento definida por el anunciante. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| datos | object | Datos del evento de App Configuration. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

El objeto data tiene las siguientes propiedades:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| key | string | Clave del valor clave que se ha modificado o eliminado. |
| etiqueta | string | Etiqueta, si procede, del valor clave que se ha modificado o eliminado. |
| ETag | string | Para `KeyValueModified`, la etag del nuevo valor clave. Para `KeyValueDeleted`, la etag del valor clave que se ha eliminado. |

## <a name="tutorials-and-how-tos"></a>Tutoriales y procedimientos

|Título | Descripción |
|---------|---------|
| [Reacción a eventos de Azure App Configuration con Event Grid](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Información general de la integración de Azure App Configuration con Event Grid. |
| [Inicio rápido: Enrutamiento de eventos de Azure App Configuration a un punto de conexión web personalizado con la CLI de Azure](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar la CLI de Azure para enviar eventos de Azure App Configuration a un webhook. |

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
* Para una introducción a cómo trabajar con eventos de Azure App Configuration, consulte [Enrutamiento de eventos de Azure App Configuration con la CLI de Azure](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 