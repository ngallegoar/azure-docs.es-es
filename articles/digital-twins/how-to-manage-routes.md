---
title: Administración de puntos de conexión y rutas
titleSuffix: Azure Digital Twins
description: Vea cómo configurar y administrar puntos de conexión y rutas de eventos para datos de Azure Digital Twins.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 923ae652872246916b2a4c5e8be95871983dbe95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559832"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins"></a>Administración de puntos de conexión y rutas en Azure Digital Twins

En Azure Digital Twins, se pueden enrutar [notificaciones de eventos](how-to-interpret-event-data.md) a los servicios de bajada o conectarse a los recursos de proceso. Para ello, en primer lugar se configuran los **puntos de conexión** que pueden recibir los eventos, seguidos de las [**rutas de eventos**](concepts-route-events.md) que especifican qué eventos generados por Azure Digital Twins se entregan a qué puntos de conexión.

Entre los tipos de puntos de conexión admitidos se incluyen los siguientes:
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Para obtener más información sobre los distintos puntos de conexión, vea [Elección entre los distintos servicios de mensajería de Azure](https://docs.microsoft.com/azure/event-grid/compare-messaging-services).

Los puntos de conexión y las rutas se administran con las [**API EventRoutes**](how-to-use-apis-sdks.md), el [SDK de .NET (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core) o la [CLI de Azure Digital Twins](how-to-use-cli.md). También se pueden administrar a través de [Azure Portal](https://portal.azure.com).

> [!NOTE]
> La administración de rutas de eventos a través de Azure Portal actualmente solo está disponible para los usuarios de Azure en cuentas de dominio corporativo. 
>
>Si usa una [cuenta de Microsoft (MSA)](https://account.microsoft.com/account/Account) personal como, por ejemplo, una cuenta de @outlook.com, use las API de Azure Digital Twins o la CLI para administrar las rutas de eventos, tal como se describe en este artículo.

## <a name="create-an-endpoint-for-azure-digital-twins"></a>Creación de un punto de conexión para Azure Digital Twins

Para vincular un punto de conexión a Azure Digital Twins, es necesario que ya exista el tema de Event Grid, Event Hubs o Service Bus que esté usando para el punto de conexión. 

En el siguiente ejemplo se muestra cómo crear un tema de Event Grid mediante la CLI de Azure:

```azurecli
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Para generar una lista de nombres de regiones de Azure que se pueden utilizar en los comandos de la CLI de Azure, ejecute este comando:
> ```azurecli
> az account list-locations -o table
> ```

Una vez creado el tema, se puede vincular a Azure Digital Twins con el comando siguiente:

```azurecli
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Esto hace que el tema de Event Grid esté disponible dentro de Azure Digital Twins, con el nombre especificado con el argumento `--endpoint-name`. Normalmente usará ese nombre como destino de una **ruta de evento**, que se creará en la sección siguiente con la API del servicio Azure Digital Twins.

Existen comandos equivalentes para los puntos de conexión de Event Hubs y Service Bus:

* Agregar punto de conexión del tema de Service Bus (requiere un recurso de Service Bus creado previamente).
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Agregar punto de conexión de Event Hubs (requiere un recurso de Event Hubs creado previamente).
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

## <a name="manage-event-routes-with-apis"></a>Administración de rutas de eventos con las API

Para enviar datos de Azure Digital Twins a un punto de conexión, debe definir una ruta de eventos. Las **API EventRoutes** de Azure Digital Twins permiten a los desarrolladores conectar el flujo de eventos a lo largo del sistema y a los servicios de bajada. Obtenga más información sobre las rutas de eventos en [Conceptos: Enrutamiento de eventos de Azure Digital Twins](concepts-route-events.md).

Los ejemplos de este artículo usan el SDK de C#.

Las rutas de eventos se definen mediante las API de plano de datos. Una definición de ruta puede contener estos elementos:
* Id. de ruta que quiere usar.
* Nombre del punto de conexión que quiere usar.
* Filtro que define los eventos que se envían al punto de conexión. 

Si no hay ningún id. de ruta, no se enruta ningún mensaje fuera de Azure Digital Twins. Si hay un id. de ruta y el filtro es `true`, todos los mensajes se enrutan al punto de conexión. Si hay un id. de ruta y se agrega un filtro diferente, los mensajes se filtrarán de acuerdo con este.

Una ruta debe permitir que se seleccionen varias notificaciones y tipos de evento. 

`CreateEventRoute` es la llamada de SDK que se usa para agregar una ruta de evento. A continuación se muestra un ejemplo del uso:

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

> [!TIP]
> Todas las funciones del SDK cuentan con versiones sincrónicas y asincrónicas.

### <a name="event-route-sample-code"></a>Código de ejemplo de la ruta de evento

En el código de ejemplo siguiente se muestra cómo crear, enumerar y eliminar una ruta de evento:
```csharp
try
{
    Console.WriteLine("Create a route: testRoute1");
    EventRoute er = new EventRoute("< your - endpoint - name >");
    // Make a filter that passes everything
    er.Filter = "true";
    client.CreateEventRoute("< your - route - name >", er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable <EventRoute> result = client.GetEventRoutes();
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointId} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
}
```

### <a name="filter-events"></a>Filtrado de eventos

Sin realizar el filtrado, los puntos de conexión reciben una serie de eventos de Azure Digital Twins:
* Telemetría que desencadena [Digital Twins](concepts-twins-graph.md) mediante la API del servicio Azure Digital Twins.
* Notificaciones relativas a cambios de propiedades de gemelos, desencadenadas en los cambios de una propiedad de cualquier gemelo en la instancia de Azure Digital Twins.
* Eventos de ciclo de vida, desencadenados cuando se crean o se eliminan gemelos o relaciones.
* Eventos de cambio de modelo, desencadenados cuando se agregan o eliminan [modelos](concepts-models.md) en una instancia de Azure Digital Twins.

Puede restringir los eventos que se envían mediante la incorporación de un filtro a un punto de conexión.

Para agregar un filtro, se puede usar una solicitud PUT en *https://{YourHost}/EventRoutes/myNewRoute?api-version=2020-05-31-preview* con el cuerpo siguiente:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Estos son los filtros de ruta admitidos.

| Nombre de filtro | Descripción | Esquema de filtro | Valores admitidos | 
| --- | --- | --- | --- |
| Tipo | [Tipo de evento](./concepts-route-events.md#types-of-event-messages) que fluye a través de la instancia de Digital Twins. | `"filter" : "type = '<eventType>'"` | `Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| Source | Nombre de la instancia de Azure Digital Twins. | `"filter" : "source = '<hostname>'"`|  **Para notificaciones**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net` <br> **Para telemetría**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net/digitaltwins/<twinId>`|
| Asunto | Descripción del evento en el contexto del origen del evento anterior. | `"filter": " subject = '<subject>'"` | **Para notificaciones**: el asunto es `<twinid>` <br> o un formato de URI para los asuntos, que se identifican de forma única mediante varias partes o id.:<br>`<twinid>/relationships/<relationshipid>`<br> **Para telemetría**: el asunto es la ruta de acceso del componente (si la telemetría se emite desde un componente gemelo), como `comp1.comp2`. Si no se emite la telemetría desde un componente, el campo de asunto está vacío. |
| Esquema de datos | Id. del modelo de DTDL. | `"filter": "dataschema = 'dtmi:example:com:floor4;2'"` | **Para telemetría**: el esquema de datos es el id. del modelo del gemelo o el componente que emite la telemetría. <br>**Para notificaciones**: no se admite el esquema de datos.|
| Tipo de contenido | Tipo de contenido del valor de datos. | `"filter": "datacontenttype = '<contentType>'"` | `application/json` |
| Versión de especificación | Versión del esquema de evento que se usa. | `"filter": "specversion = '<version>'"` | Debe ser `1.0`. Esto indica la versión de esquema de CloudEvents es la 1.0. |
| True o False | Permite crear una ruta sin filtrado o deshabilitar una ruta. | `"filter" : "<true/false>"` | `true`: la ruta está habilitada sin filtrado. <br> `false`: la ruta está deshabilitada. |
<!--
| ID | *Not implemented for public preview* | "filter": "id = '…'" | |
| Schema | *Not implemented for public preview*  | "filter": "dataschema = '…'" | |
-->

También es posible combinar filtros mediante las operaciones siguientes:

| Nombre de filtro | Esquema de filtro | Ejemplo | 
| --- | --- | --- |
| Y/O | `"filter": "<filter1> AND <filter2>"` | `"filter": "type != 'microsoft.iot.telemetry' AND datacontenttype = 'application/json'"` |
| Y/O | `"filter": "<filter1> OR <filter2>"` | `"filter": "type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json'"` |
| Operaciones anidadas | `"filter": "(<Comparison1>) AND (<Comparison2>)"` | `"filter": "(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')"` |

> [!NOTE]
> En la versión preliminar, solo se admite la igualdad de cadenas (=, !=).

Al implementar o actualizar un filtro, puede que el cambio tarde unos minutos en reflejarse en la canalización de datos.

## <a name="manage-endpoints-and-routes-with-cli"></a>Administración de puntos de conexión y rutas con la CLI

Los puntos de conexión y las rutas también se pueden administrar mediante la CLI de Azure Digital Twins. Los comandos se pueden encontrar en [Procedimiento: Uso de la CLI de Azure Digital Twins](how-to-use-cli.md).

## <a name="monitor-event-routes"></a>Supervisión de rutas de eventos

Las métricas de enrutamiento, como el recuento, la latencia y la tasa de errores, se pueden ver en [Azure Portal](https://portal.azure.com/). 

En la página principal del portal, busque la instancia de Azure Digital Twins para ver sus detalles. Seleccione la opción **Métricas** en el menú de la instancia de Azure Digital Twins para abrir la página *Métricas*.

:::image type="content" source="media/how-to-manage-routes/metrics.png" alt-text="Página de métricas de una instancia de Azure Digital Twins en Azure Portal":::

Desde aquí, se pueden ver las métricas de la instancia y crear vistas personalizadas.

## <a name="next-steps"></a>Pasos siguientes

Lea acerca de los diferentes tipos de mensajes de evento que se pueden recibir:
* [Procedimiento: Interpretación de los datos de evento](how-to-interpret-event-data.md)
