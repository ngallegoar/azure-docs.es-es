---
title: Azure App Service como origen de Event Grid
description: En este artículo se describe cómo utilizar Azure App Service como origen de eventos de Event Grid. Proporciona el esquema y los vínculos a los artículos de procedimientos y tutorial.
author: jasonfreeberg
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: jafreebe
ms.openlocfilehash: 2465b2f260ed6c174b762fcf64a71100a148254d
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86106718"
---
# <a name="azure-app-service-as-an-event-grid-source"></a>Azure App Service como origen de Event Grid

En este artículo se proporcionan las propiedades y los esquemas de los eventos de Azure App Service. Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md). También proporciona una lista de inicios rápidos y tutoriales para usar Azure App Service como origen de eventos.

## <a name="event-grid-event-schema"></a>Esquema de eventos de Event Grid

### <a name="available-event-types"></a>Tipos de eventos disponibles

Azure App Service emite los siguientes tipos de eventos:

|    Tipo de evento                                             |    Descripción                                                     |
|-----------------------------------------------------------|--------------------------------------------------------------------|
|    Microsoft.Web/sites.BackupOperationStarted             |    Se desencadena cuando se inicia una copia de seguridad.                             |
|    Microsoft.Web/sites.BackupOperationCompleted           |    Se desencadena cuando se completa una copia de seguridad.                           |
|    Microsoft.Web/sites.BackupOperationFailed              |    Se desencadena cuando una copia de seguridad devuelve un error.                              |
|    Microsoft.Web/sites.RestoreOperationStarted            |    Se desencadena cuando se inicia una restauración a partir de una copia de seguridad.        |
|    Microsoft.Web/sites.RestoreOperationCompleted          |    Se desencadena cuando se completa una restauración a partir de una copia de seguridad.      |
|    Microsoft.Web/sites.RestoreOperationFailed             |    Se desencadena cuando una restauración a partir de una copia de seguridad devuelve un error.         |
|    Microsoft.Web/sites.SlotSwapStarted                    |    Se desencadena cuando se inicia un cambio de espacio.                          |
|    Microsoft.Web/sites.SlotSwapCompleted                  |    Se desencadena cuando se completa un cambio de espacio.                      |
|    Microsoft.Web/sites.SlotSwapFailed                     |    Se desencadena cuando un cambio de espacio devuelve un error.                           |
|    Microsoft.Web/sites.SlotSwapWithPreviewStarted         |    Se desencadena cuando se inicia un cambio de espacio con una vista previa.           |
|    Microsoft.Web/sites.SlotSwapWithPreviewCancelled       |    Se desencadena cuando se cancela un cambio de espacio con una vista previa.    |
|    Microsoft.Web/sites.AppUpdated.Restarted               |    Se desencadena cuando se reinicia un sitio.                      |
|    Microsoft.Web/sites.AppUpdated.Stopped                 |    Se desencadena cuando se detiene un sitio.                          |
|    Microsoft.Web/sites.AppUpdated.ChangedAppSettings      |    Se desencadena cuando cambia la configuración de la aplicación de un sitio.             |
|    Microsoft.Web/serverfarms.AppServicePlanUpdated        |    Se desencadena cuando se actualiza un plan de App Service                 |

### <a name="the-contents-of-an-event-response"></a>El contenido de una respuesta de evento

Cuando se desencadena un evento, el servicio Event Grid envía datos sobre ese evento al punto de conexión correspondiente.
Esta sección contiene un ejemplo del aspecto que deben tener los datos para cada evento. Cada evento tiene los siguientes datos de nivel superior:

|     Propiedad          |     Tipo     |     Descripción                                                                                                                                |
|-----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|    topic              |    string    |    Ruta de acceso completa al origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor.                                      |
|    subject            |    string    |    Ruta de acceso al asunto del evento definido por el anunciante.                                                                                              |
|    eventType          |    string    |    Uno de los tipos de eventos registrados para este origen de eventos.                                                                                  |
|    eventTime          |    string    |    La hora de generación del evento en función de la hora UTC del proveedor.                                                                         |
|    id                 |    string    |    Identificador único para el evento                                                                                                            |
|    datos               |    object    |    Datos de eventos de Blob Storage.                                                                                                                    |
|    dataVersion        |    string    |    Versión del esquema del objeto de datos. El publicador define la versión del esquema.                                                          |
|    metadataVersion    |    string    |    Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor.    |

#### <a name="backupoperationstarted-backupoperationcompleted-backupoperationfailed"></a>BackupOperationStarted, BackupOperationCompleted, BackupOperationFailed

```js
{
    id:'7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject:'/Microsoft.Web/sites/<site-name>',
    eventType:'Microsoft.Web.BackupOperationStarted',
    eventTime:'2020-01-28T18:26:51.7194887Z',
    data: {
        "appEventTypeDetail": { "action": "Started" },
        "siteName": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "None"
    }
    topic:'/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion:'1',
    metaDataVersion:'1'
}
```

El objeto de datos contiene las siguientes propiedades:

|    Propiedad                |    Tipo      |    Descripción                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    object    |    Detalles de la acción en la aplicación                                                                                       |
|    action                  |    string    |    Tipo de acción de la operación                                                                                   |
|    name                    |    string    |    Nombre del sitio web que tenía este evento                                                                          |
|    clientRequestId         |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.         |
|    correlationRequestId    |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.    |
|    requestId               |    string    |    Identificador de solicitud generado por App Service para la operación de API de sitio que desencadenó este evento.                |
|    address                 |    string    |    Dirección URL de la solicitud HTTP de esta operación                                                                                |
|    Verbo                    |    string    |    Verbo URL de esta operación                                                                                       |

#### <a name="restoreoperationstarted-restoreoperationcompleted-restoreoperationfailed"></a>RestoreOperationStarted, RestoreOperationCompleted, RestoreOperationFailed

```js
{
    id: '7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.RestoreOperationStarted',
    eventTime: '2020-01-28T18:26:51.7194887Z',
    data: {
        appEventTypeDetail: { 
            action: "Started" 
        },
        siteName: "<site-name>",
        clientRequestId: "None",
        correlationRequestId: "None",
        requestId: "292f499d-04ee-4066-994d-c2df57b99198",
        address: "None",
        verb: "POST"
    }
    topic: '/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

El objeto de datos contiene las siguientes propiedades:

|    Propiedad                |    Tipo      |    Descripción                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    object    |    Detalles de la acción en la aplicación                                                                                       |
|    action                  |    string    |    Tipo de acción de la operación                                                                                   |
|    name                    |    string    |    Nombre del sitio web que tenía este evento                                                                          |
|    clientRequestId         |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.         |
|    correlationRequestId    |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.    |
|    requestId               |    string    |    Identificador de solicitud generado por App Service para la operación de API de sitio que desencadenó este evento.                |
|    address                 |    string    |    Dirección URL de la solicitud HTTP de esta operación                                                                                |
|    Verbo                    |    string    |    Verbo URL de esta operación                                                                                       |

#### <a name="slotswapstarted-slotswapcompleted-slotswapfailed"></a>SlotSwapStarted, SlotSwapCompleted, SlotSwapFailed

```js
{
    id: '7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.SlotSwapStarted',
    eventTime: '2020-01-28T18:26:51.7194887Z',
    data: {
        appEventTypeDetail: null,
        siteName: '<site-name>',
        clientRequestId: '922f4841-20d9-4dd6-8c5b-23f0d85e5592',
        correlationRequestId: '9ac46505-2b8a-4e06-834c-05ffbe2e8c3a',
        requestId: '765117aa-eaf8-4bd2-a644-1dbf69c7b0fd',
        address: '/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production',
        verb: 'POST'
        sourceSlot: "staging",
        targetSlot: "production"
    },
    topic: '/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

El objeto de datos contiene las siguientes propiedades:

|    Propiedad                |    Tipo      |    Descripción                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    object    |    Detalles de la acción en la aplicación                                                                                       |
|    action                  |    string    |    Tipo de acción de la operación                                                                                   |
|    name                    |    string    |    Nombre del sitio web que tenía este evento                                                                          |
|    clientRequestId         |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.         |
|    correlationRequestId    |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.    |
|    requestId               |    string    |    Identificador de solicitud generado por App Service para la operación de API de sitio que desencadenó este evento.                |
|    address                 |    string    |    Dirección URL de la solicitud HTTP de esta operación                                                                                |
|    Verbo                    |    string    |    Verbo URL de esta operación                                                                                       |
|    sourceSlot              |    string    |    Espacio de origen del cambio                                                                                       |

#### <a name="slotswapwithpreviewstarted-slotswapwithpreviewcancelled"></a>SlotSwapWithPreviewStarted, SlotSwapWithPreviewCancelled

```js
{
    id: '7c5d6de5-eb70-4de2-b788-c52a544e68b8',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.SlotSwapWithPreviewStarted',
    eventTime: '2020-01-28T18:26:51.7194887Z',
    data: {
        appEventTypeDetail: null,
        siteName: '<site-name>',
        clientRequestId: '922f4841-20d9-4dd6-8c5b-23f0d85e5592',
        correlationRequestId: '9ac46505-2b8a-4e06-834c-05ffbe2e8c3a',
        requestId: '765117aa-eaf8-4bd2-a644-1dbf69c7b0fd',
        address: '/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production',
        verb: 'POST'
        sourceSlot: "staging",
        targetSlot: "production"
    },
    topic: '/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

El objeto de datos contiene las siguientes propiedades:

|    Propiedad                |    Tipo      |    Descripción                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    object    |    Detalles de la acción en la aplicación                                                                                       |
|    action                  |    string    |    Tipo de acción de la operación                                                                                   |
|    name                    |    string    |    Nombre del sitio web que tenía este evento                                                                          |
|    clientRequestId         |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.         |
|    correlationRequestId    |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.    |
|    requestId               |    string    |    Identificador de solicitud generado por App Service para la operación de API de sitio que desencadenó este evento.                |
|    address                 |    string    |    Dirección URL de la solicitud HTTP de esta operación                                                                                |
|    Verbo                    |    string    |    Verbo URL de esta operación                                                                                       |

#### <a name="appupdatedrestarted-appupdatedstopped-appupdatedchangedappsettings"></a>AppUpdated.Restarted, AppUpdated.Stopped, AppUpdated.ChangedAppSettings

```js
{
    id: 'b74ea56b-2a3f-4de5-a5d7-38e60c81cf23',
    subject: '/Microsoft.Web/sites/<site-name>',
    eventType: 'Microsoft.Web.AppUpdated',
    eventTime: '2020-01-28T18:22:30.2760952Z',
    data: {
        appEventTypeDetail: {
            action: 'Stopped'
        },
        siteName: '<site-name>',
        clientRequestId: '64a5e0aa-7cee-4ff1-9093-b9197b820014',
        correlationRequestId: '25bb36a5-8f6c-4f04-b615-e9a0ee045756',
        requestId: 'f2e8eb3f-b190-42de-b99e-6acefe587374',
        address: '/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/stop',
        verb: 'POST'
    },
    topic: '/subscriptions/<id>/resourceGroups/<group>/providers/Microsoft.Web/sites/<site-name>',
    dataVersion: '1',
    metaDataVersion: '1'
}
```

El objeto data tiene las siguientes propiedades:

|    Propiedad                |    Tipo      |    Descripción                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appEventTypeDetail      |    object    |    Detalles de la acción en la aplicación                                                                                       |
|    action                  |    string    |    Tipo de acción de la operación                                                                                   |
|    name                    |    string    |    Nombre del sitio web que tenía este evento                                                                          |
|    clientRequestId         |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.         |
|    correlationRequestId    |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.    |
|    requestId               |    string    |    Identificador de solicitud generado por App Service para la operación de API de sitio que desencadenó este evento.                |
|    address                 |    string    |    Dirección URL de la solicitud HTTP de esta operación                                                                                |
|    Verbo                    |    string    |    Verbo URL de esta operación                                                                                       |

#### <a name="serverfarmsappserviceplanupdated"></a>Serverfarms.AppServicePlanUpdated

```js
{
   id: "56501672-9150-40e1-893a-18420c7fdbf7",
   subject: "/Microsoft.Web/serverfarms/<plan-name>",
   eventType: "Microsoft.Web.AppServicePlanUpdated",
   eventTime: "2020-01-28T18:22:23.5516004Z",
   data: {
        serverFarmEventTypeDetail: {
            stampKind: "Public",
            action: "Updated",
            status: "Started"
        },
        serverFarmId: "0",
        sku: {
            name: "P1v2",
            tier: "PremiumV2",
            size: "P1v2",
            family: "Pv2",
            capacity: 1
        },
        clientRequestId: "8f880321-a991-45c7-b743-6ff63fe4c004",
        correlationRequestId: "1995c3be-ba7f-4ccf-94af-516df637ec8a",
        requestId: "b973a8e6-6949-4783-b44c-ac778be831bb",
        address: "/websystems/WebSites/serverfarms/subscriptions/<id>/webspaces/<webspace-id>/serverfarms/<plan-name>/async",
        verb: "PUT"
   },
   topic: "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/serverfarms/<serverfarm-name>",
   dataVersion: "1",
   metaDataVersion: "1"
}
```

El objeto data tiene las siguientes propiedades:

|    Propiedad                         |    Tipo      |    Descripción                                                                                                       |
|-------------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    appServicePlanEventTypeDetail    |    object    |    Detalles de la acción del plan de App Service                                                                          |
|    stampKind                        |    string    |    Tipo de entorno en que se encuentra el plan de App Service                                                                     |
|    action                           |    string    |    Tipo de acción del plan de App Service                                                                            |
|    status                           |    string    |    Estado de la operación del plan de App Service                                                                   |
|    sku                              |    object    |    SKU del plan de App Service.                                                                                       |
|    name                             |    string    |    Nombre del plan de App Service                                                                                      |
|    Nivel                             |    string    |    Nivel del plan de App Service                                                                                      |
|    Size                             |    string    |    Tamaño del plan de App Service                                                                                      |
|    Familia                           |    string    |    Familia del plan de App Service                                                                                        |
|    Capacity                         |    string    |    Capacidad del plan de App Service                                                                                      |
|    action                           |    string    |    Tipo de acción de la operación                                                                                   |
|    name                             |    string    |    Nombre del sitio web que tenía este evento                                                                          |
|    clientRequestId                  |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.         |
|    correlationRequestId             |    string    |    Identificador de solicitud de cliente generado por App Service para la operación de API de sitio que desencadenó este evento.    |
|    requestId                        |    string    |    Identificador de solicitud generado por App Service para la operación de API de sitio que desencadenó este evento.                |
|    address                          |    string    |    Dirección URL de la solicitud HTTP de esta operación                                                                                |
|    Verbo                             |    string    |    Verbo URL de esta operación                                                                                       |
