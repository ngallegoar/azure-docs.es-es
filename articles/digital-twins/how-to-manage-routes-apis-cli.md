---
title: Administración de puntos de conexión y rutas (API y CLI)
titleSuffix: Azure Digital Twins
description: Vea cómo configurar y administrar puntos de conexión y rutas de eventos para datos de Azure Digital Twins.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 10/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 0b8bd9006482daf7c9218f0f3dbb16d2e08359bf
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533759"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>Administración de puntos de conexión y rutas en Azure Digital Twins (API y CLI)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

En Azure Digital Twins, se pueden enrutar [notificaciones de eventos](how-to-interpret-event-data.md) a los servicios de bajada o recursos de proceso conectados. Para ello, primero se configuran los **puntos de conexión** que pueden recibir los eventos. Después, puede crear [**rutas de eventos**](concepts-route-events.md) que especifiquen qué eventos generados por Azure Digital Twins se entregan a los puntos de conexión.

Este artículo le guiará a través del proceso de creación de puntos de conexión y rutas con las [API de Event Routes](/rest/api/digital-twins/dataplane/eventroutes), el [SDK para .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) y la [CLI de Azure Digital Twins](how-to-use-cli.md).

También puede administrar los puntos de conexión y las rutas con [Azure Portal](https://portal.azure.com). Para obtener una versión de este artículo que usa el portal en su lugar, consulte [*Procedimiento: Administración de puntos de conexión y rutas (portal)*](how-to-manage-routes-portal.md).

## <a name="prerequisites"></a>Requisitos previos

* Necesitará una **cuenta de Azure** (puede configurar una de forma gratuita [aquí](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)).
* Necesitará una **instancia de Azure Digital Twins** en su suscripción de Azure. Si no dispone de una instancia, puede crear una con los pasos de [*Procedimiento: Configuración de una instancia y autenticación*](how-to-set-up-instance-cli.md). Tenga a mano los siguientes valores de configuración para usarlos más adelante en este artículo:
    - Nombre de instancia
    - Grupo de recursos
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>Creación de un punto de conexión para Azure Digital Twins

Estos son los tipos de puntos de conexión admitidos que puede crear para la instancia:
* [Event Grid](../event-grid/overview.md)
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Para obtener más información sobre los distintos tipos de puntos de conexión, vea [*Elección entre los distintos servicios de mensajería de Azure*](../event-grid/compare-messaging-services.md).

Para vincular un punto de conexión a Azure Digital Twins, es necesario que ya exista el tema de Event Grid, Event Hubs o Service Bus que esté usando para el punto de conexión. 

### <a name="create-an-event-grid-endpoint"></a>Creación de un punto de conexión de Event Grid

En el siguiente ejemplo se muestra cómo crear un punto de conexión de tipo Event Grid mediante la CLI de Azure. Puede usar [Azure Cloud Shell](https://shell.azure.com) o [instalar la CLI localmente](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).

Primero, cree un tema de Event Grid. Puede usar el siguiente comando o, para ver los pasos con más detalle, visite [la sección *Creación de un tema personalizado*](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) de la guía de inicio rápido de *eventos personalizados* de Event Grid.

```azurecli-interactive
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Para generar una lista de nombres de regiones de Azure que se pueden utilizar en los comandos de la CLI de Azure, ejecute este comando:
> ```azurecli-interactive
> az account list-locations -o table
> ```

Una vez creado el tema, se puede vincular a Azure Digital Twins con el [comando siguiente de la CLI de Azure Digital Twins](how-to-use-cli.md):

```azurecli-interactive
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Ahora, el tema de Event Grid está disponible como punto de conexión dentro de Azure Digital Twins, con el nombre especificado con el argumento `--endpoint-name`. Normalmente usará ese nombre como destino de una **ruta de evento**, que se creará [más tarde en este artículo](#create-an-event-route) con la API del servicio Azure Digital Twins.

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>Creación de un punto de conexión de Event Hubs o Service Bus

El proceso de creación de puntos de conexión de Event Hubs o Service Bus es similar al proceso de Event Grid que se muestra anteriormente.

En primer lugar, cree los recursos que usará como punto de conexión. A continuación, si indica lo que se necesita:
* Service Bus: un _espacio de nombres de Service Bus_, un _tema de Service Bus_ y una _regla de autorización_.
* Event Hubs: un _espacio de nombres de Event Hubs_, un _centro de eventos_ y una _regla de autorización_.

A continuación, use los siguientes comandos para crear los puntos de conexión en Azure Digital Twins: 

* Agregar punto de conexión del tema de Service Bus (requiere un recurso de Service Bus creado previamente).
```azurecli-interactive 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Agregar punto de conexión de Event Hubs (requiere un recurso de Event Hubs creado previamente).
```azurecli-interactive
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

### <a name="create-an-endpoint-with-dead-lettering"></a>Creación de un punto de conexión con colas de mensajes fallidos

Cuando un punto de conexión no puede entregar un evento en un período de tiempo determinado o después de haber intentado entregarlo un número determinado de veces, podrá enviar el evento sin entregar a una cuenta de almacenamiento. Este proceso se conoce como **colas de mensajes fallidos**.

Para crear un punto de conexión con la opción de poner en cola los mensajes fallidos, debe usar las [API de ARM](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) para crear el punto de conexión. 

Antes de establecer la ubicación de mensajes fallidos, debe tener una cuenta de almacenamiento con un contenedor. Tiene que proporcionar la dirección URL de este contenedor al crear el punto de conexión. La cola de mensajes fallidos se suministra como una dirección URL del contenedor con un token de SAS. Ese token solo necesita permiso `write` para el contenedor de destino dentro de la cuenta de almacenamiento. La dirección URL totalmente estructurada tendrá este formato: `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>`.

Para obtener más información sobre los tokens de SAS, consulte: [Grant limited access to Azure Storage resources using shared access signatures (SAS)](/azure/storage/common/storage-sas-overview) (Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido [SAS])

Para obtener más información acerca de la opción para poner en cola los mensajes fallidos, consulte [*Conceptos: rutas de eventos*](concepts-route-events.md#dead-letter-events).

#### <a name="configuring-the-endpoint"></a>Configuración del punto de conexión

Al crear un punto de conexión, agregue un valor `deadLetterSecret` al objeto `properties` en el cuerpo de la solicitud, que contiene una dirección URL de contenedor y un token de SAS para la cuenta de almacenamiento.

```json
{
  "properties": {
    "endpointType": "EventGrid",
    "TopicEndpoint": "https://contosoGrid.westus2-1.eventgrid.azure.net/api/events",
    "accessKey1": "xxxxxxxxxxx",
    "accessKey2": "xxxxxxxxxxx",
    "deadLetterSecret":"https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>"
  }
}
```

Para obtener más información, consulte la documentación de la API de REST de Azure Digital Twins: [Puntos de conexión: DigitalTwinsEndpoint CreateOrUpdate](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate).

### <a name="message-storage-schema"></a>Esquema de almacenamiento de mensajes

Los mensajes fallidos que están en cola se almacenarán con el formato siguiente en la cuenta de almacenamiento:

`{container}/{endpointName}/{year}/{month}/{day}/{hour}/{eventId}.json`

Los mensajes fallidos en cola coinciden con el esquema del evento original que se diseñó para entregarse al punto de conexión original.

A continuación se muestra un ejemplo de un mensaje fallido en cola de una [notificación de creación gemela](how-to-interpret-event-data.md#digital-twin-life-cycle-notifications):

```json
{
  "specversion": "1.0",
  "id": "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "<yourInstance>.api.<yourregion>.da.azuredigitaltwins-test.net",
  "data": {
    "$dtId": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "$etag": "W/\"xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "TwinData": "some sample",
    "$metadata": {
      "$model": "dtmi:test:deadlettermodel;1",
      "room": {
        "lastUpdateTime": "2020-10-14T01:11:49.3576659Z"
      }
    }
  },
  "subject": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "time": "2020-10-14T01:11:49.3667224Z",
  "datacontenttype": "application/json",
  "traceparent": "00-889a9094ba22b9419dd9d8b3bfe1a301-f6564945cb20e94a-01"
}
```

## <a name="create-an-event-route"></a>Crear una ruta de eventos

Para enviar datos de Azure Digital Twins a un punto de conexión, debe definir una **ruta de eventos**. Las **API EventRoutes** de Azure Digital Twins permiten a los desarrolladores conectar el flujo de eventos a lo largo del sistema y a los servicios de bajada. Obtenga más información sobre las rutas de eventos en [*Conceptos: Enrutamiento de eventos de Azure Digital Twins*](concepts-route-events.md).

Los ejemplos de esta sección usan el [SDK de .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true).

**Requisito previo**: Debe crear puntos de conexión como se describió anteriormente en este artículo para poder continuar con la creación de una ruta. Puede continuar con la creación de una ruta de evento una vez finalizada la configuración de los puntos de conexión.

>[!NOTE]
>Si ha implementado recientemente los puntos de conexión, compruebe que se haya completado la implementación **antes** de intentar usarlos para una nueva ruta de evento. Si se produce un error en la implementación de la ruta porque los puntos de conexión no están listos, espere unos minutos y vuelva a intentarlo.
>
> Si crea scripts de este flujo, puede que le interese dejar entre 2 y 3 minutos de tiempo de espera para que el servicio del punto de conexión termine de implementarse antes de pasar a la configuración de la ruta.

### <a name="creation-code-with-apis-and-the-c-sdk"></a>Código de creación con las API y el SDK de C#

Las rutas de eventos se definen mediante las [API de plano de datos](how-to-use-apis-sdks.md#overview-data-plane-apis). 

Una definición de ruta puede contener estos elementos:
* El nombre de ruta que quiere usar.
* Nombre del punto de conexión que quiere usar.
* Filtro que define los eventos que se envían al punto de conexión. 

Si no hay ningún nombre de ruta, no se enruta ningún mensaje fuera de Azure Digital Twins. Si hay un nombre de ruta y el filtro es `true`, todos los mensajes se enrutan al punto de conexión. Si hay un nombre de ruta y se agrega un filtro diferente, los mensajes se filtrarán de acuerdo con este.

Una ruta debe permitir que se seleccionen varias notificaciones y tipos de evento. 

`CreateOrReplaceEventRouteAsync` es la llamada de SDK que se usa para agregar una ruta de evento. A continuación se muestra un ejemplo del uso:

```csharp
string eventFilter = "$eventType = 'DigitalTwinTelemetryMessages' or $eventType = 'DigitalTwinLifecycleNotification'";
var er = new DigitalTwinsEventRoute("<your-endpointName>", eventFilter);
await CreateOrReplaceEventRouteAsync(client, "routeName", er);
```
    
> [!TIP]
> Todas las funciones del SDK cuentan con versiones sincrónicas y asincrónicas.

### <a name="event-route-sample-code"></a>Código de ejemplo de la ruta de evento

En el método de ejemplo siguiente se muestra cómo crear, enumerar y eliminar una ruta de eventos:
```csharp
private async static Task CreateEventRoute(DigitalTwinsClient client, String routeName, DigitalTwinsEventRoute er)
{
  try
  {
    Console.WriteLine("Create a route: testRoute1");
            
    // Make a filter that passes everything
    er.Filter = "true";
    await client.CreateOrReplaceEventRouteAsync(routeName, er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable<DigitalTwinsEventRoute> result = client.GetEventRoutes();
    foreach (DigitalTwinsEventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointName} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (DigitalTwinsEventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
  }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
    }
  }
```

## <a name="filter-events"></a>Filtrado de eventos

Sin realizar el filtrado, los puntos de conexión reciben una serie de eventos de Azure Digital Twins:
* Telemetría que desencadena [Digital Twins](concepts-twins-graph.md) mediante la API del servicio Azure Digital Twins.
* Notificaciones relativas a cambios de propiedades de gemelos, desencadenadas en los cambios de una propiedad de cualquier gemelo en la instancia de Azure Digital Twins.
* Eventos de ciclo de vida, desencadenados cuando se crean o se eliminan gemelos o relaciones.

Puede restringir los eventos que se envían mediante la incorporación de un **filtro** para un punto de conexión a una ruta de evento.

Para agregar un filtro, se puede usar una solicitud PUT en *https://{YourHost}/EventRoutes/myNewRoute?api-version=2020-10-31* con el cuerpo siguiente:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Estos son los filtros de ruta admitidos. Use los detalles de la columna *Filtrar esquema de texto* para reemplazar el marcador de posición `<filter-text>` en el cuerpo de la solicitud anterior.

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

## <a name="manage-endpoints-and-routes-with-cli"></a>Administración de puntos de conexión y rutas con la CLI

Los puntos de conexión y las rutas también se pueden administrar mediante la CLI de Azure Digital Twins. Para obtener más información sobre el uso de la CLI y los comandos disponibles, consulte [*Procedimiento: Uso de la CLI de Azure Digital Twins*](how-to-use-cli.md).

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Pasos siguientes

Lea acerca de los diferentes tipos de mensajes de evento que se pueden recibir:
* [*Procedimiento: Interpretación de los datos de evento*](how-to-interpret-event-data.md)