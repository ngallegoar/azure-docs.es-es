---
title: 'Tutorial: Creación de una geovalla y seguimiento de dispositivos en Azure Maps'
description: Obtenga información sobre cómo configurar una geovalla. Obtenga información sobre cómo realizar un seguimiento de los dispositivos con respecto a la geovalla mediante el servicio espacial de Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 8/11/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: b374bbe086281c7f7914334be6ca275f0fd05b7f
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056516"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Tutorial: Configuración de una geovalla con Azure Maps

Este tutorial le guía por los aspectos básicos de la creación y el uso de los servicios de geovalla de Azure Maps. Lo hará en el contexto del siguiente escenario:

*Un jefe de obras de una construcción debe realizar un seguimiento de las entradas y salidas del equipo en los perímetros de un área de construcción. Cada vez que un elemento del equipo sale o entra en estos perímetros, se envía una notificación por correo electrónico al responsable de operaciones.*

Azure Maps proporciona una serie de servicios que admiten el seguimiento de los equipos que entran y salen del área de construcción. En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Cargará los [datos GeoJSON de geovallas](geofence-geojson.md) que definen las áreas del sitio de construcción que desea supervisar. Usará [Data Upload API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) para cargar geovallas como coordenadas de polígonos en la cuenta de Azure Maps.
> * Configurará dos [aplicaciones lógicas](https://docs.microsoft.com/azure/event-grid/handler-webhooks#logic-apps) que, cuando se desencadenan, envían notificaciones de correo electrónico al administrador de operaciones del sitio de construcción, cuando el equipo entre y salga del área de la geovalla.
> * Usará [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) para suscribirse a los eventos de entrada y salida de la geovalla de Azure Maps. Configurará dos suscripciones a eventos de webhook que llaman a los puntos de conexión HTTP definidos en las dos aplicaciones lógicas. A continuación, las aplicaciones lógicas enviarán las notificaciones de correo electrónico adecuadas sobre los equipos que salen o entran de la geovalla.
> * Use [Search Geofence Get API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) para recibir notificaciones cuando una parte del equipo salga y entre de las áreas de la geovalla.

## <a name="prerequisites"></a>Requisitos previos

1. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).
2. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción.

En este tutorial se usa la aplicación [Postman](https://www.postman.com/), pero se puede elegir otro entorno de desarrollo de API.

## <a name="upload-geofencing-geojson-data"></a>Carga de datos GeoJSON de geovallas

En este tutorial, se cargarán datos GeoJSON de geovallas que contienen el elemento `FeatureCollection`. La colección `FeatureCollection` contiene dos geovallas que definen áreas poligonales dentro del sitio de construcción. La primera geovalla no tiene ninguna restricción de tiempo ni datos de expiración. La segunda solo se puede consultar en horario comercial (9:00 AM-5:00 PM en la zona horaria del Pacífico) y dejará de ser válida después del 1 de enero de 2022. Para más información sobre el formato GeoJSON, consulte [Datos GeoJSON de geovallas](geofence-geojson.md).

>[!TIP]
>Puede actualizar los datos de geovallas en cualquier momento. Para más información, consulte [Data Upload API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview).

1. Abra la aplicación Postman. Cerca de la parte superior, seleccione **New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Collection** (Colección). Asigne un nombre a la colección y seleccione **Create** (Crear).

2. Para crear la solicitud, seleccione **New** (Nuevo) otra vez. En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Seleccione la colección que creó en el paso anterior y haga clic en **Save** (Guardar).

3. Seleccione el método HTTP **POST** en la pestaña del generador y escriba la siguiente dirección URL para cargar los datos de geovallas en Azure Maps. Para esta solicitud y otras solicitudes mencionadas en este artículo, reemplace `{Azure-Maps-Primary-Subscription-key}` por su clave de suscripción principal.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={Azure-Maps-Primary-Subscription-key}&api-version=1.0&dataFormat=geojson
    ```

    El parámetro `geojson` en la ruta de acceso de la dirección URL representa el formato de los datos que se cargan.

4. Seleccione la pestaña **Cuerpo**. Seleccione **sin formato** y, a continuación, **JSON** como formato de entrada. Copie y pegue los siguientes datos de GeoJSON en el área de texto de **Cuerpo**:

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
            "expiredTime": "2022-01-01T00:00:00",
            "validityPeriod": [
                {
                  "startTime": "2020-07-15T16:00:00",
                  "endTime": "2020-07-15T24:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

5. Seleccione **Send** (Enviar) y espere a que se procese la solicitud. Una vez finalizada la solicitud, vaya a la pestaña **Headers** (Encabezados) de la respuesta. Copie el valor de la clave **Location** (Ubicación), que es `status URL`.

    ```http
    https://atlas.microsoft.com/mapData/operations/<operationId>?api-version=1.0
    ```

6. Para comprobar el estado de la llamada de API, cree una solicitud HTTP **GET** en el elemento `status URL`. Tendrá que anexar la clave de suscripción principal a la dirección URL para realizar la autenticación. La solicitud **GET** debe ser como la siguiente dirección URL:

   ```HTTP
   https://atlas.microsoft.com/mapData/<operationId>/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Cuando la solicitud HTTP **GET** se completa correctamente, devuelve un elemento `resourceLocation`. El `resourceLocation` contiene el `udid` único para el contenido cargado. Guarde este `udid` para consultar Get Geofence API en la última sección de este tutorial. Opcionalmente, puede usar la dirección URL del `resourceLocation` para recuperar los metadatos de este recurso en el siguiente paso.

      ```json
      {
          "status": "Succeeded",
          "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0"
      }
      ```

8. Para recuperar metadatos de contenido, cree una solicitud HTTP **GET** en la dirección URL `resourceLocation` que se recuperó en el paso 7. Asegúrese de anexar la clave de suscripción principal a la dirección URL para la autenticación. La solicitud **GET** debe ser como la siguiente dirección URL:

    ```http
   https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

9. Cuando la solicitud HTTP **GET** se complete correctamente, el cuerpo de la respuesta contendrá el valor de `udid` especificado en el elemento `resourceLocation` en el paso 7. También contendrá la ubicación para acceder y descargar el contenido en el futuro y otros metadatos sobre el contenido. Un ejemplo de la respuesta general es:

    ```json
    {
        "udid": "{udid}",
        "location": "https://atlas.microsoft.com/mapData/{udid}?api-version=1.0",
        "created": "7/15/2020 6:11:43 PM +00:00",
        "updated": "7/15/2020 6:11:45 PM +00:00",
        "sizeInBytes": 1962,
        "uploadStatus": "Completed"
    }
    ```

## <a name="create-workflows-in-azure-logic-apps"></a>Creación de flujos de trabajo en Azure Logic Apps

A continuación, va a crear dos puntos de conexión de la [aplicación lógica](https://docs.microsoft.com/azure/event-grid/handler-webhooks#logic-apps) que desencadenan una notificación por correo electrónico. Aquí se muestra cómo crear el primero:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. En la esquina superior izquierda de Azure Portal, seleccione **Crear un recurso**.

3. En el campo **Buscar en el Marketplace**, escriba **Aplicación lógica**.

4. En los resultados, seleccione **Aplicación lógica** > **Crear**.

5. En la página **Aplicación lógica**, escriba los valores siguientes:
    * La **suscripción** que quiere usar para esta aplicación lógica.
    * El nombre del **grupo de recursos** para esta aplicación lógica. Para el grupo de recursos, puede elegir entre **Crear nuevo** o **Usar existente**.
    * El **nombre de la aplicación lógica**. En este caso, usaremos `Equipment-Enter` como nombre.

    Para los fines de este tutorial, mantenga la configuración predeterminada del resto de los valores.

    :::image type="content" source="./media/tutorial-geofence/logic-app-create.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

6. Seleccione **Revisar + crear**. Revise la configuración y seleccione **Crear** para enviar la implementación. Cuando la implementación se complete correctamente, seleccione **Ir al recurso**. Se le dirigirá al **Diseñador de aplicación lógica**.

7. Seleccione un tipo de desencadenador. Desplácese hacia abajo hasta la sección **Empezar con un desencadenador común**. Seleccione **Cuando se recibe una solicitud HTTP**.

     :::image type="content" source="./media/tutorial-geofence/logic-app-trigger.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

8. En la esquina superior derecha del Diseñador de aplicación lógica, seleccione **Guardar**. La **dirección URL de HTTP POST** se genera automáticamente. Guarde la dirección URL. La necesitará en la sección siguiente para crear un punto de conexión de eventos.

    :::image type="content" source="./media/tutorial-geofence/logic-app-httprequest.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

9. Seleccione **+ Nuevo paso**. Ahora elegiremos una acción. Escriba `outlook.com email` en el cuadro de búsqueda. En la lista **Acciones**, desplácese hacia abajo y seleccione **Enviar un correo electrónico (V2)** .
  
    :::image type="content" source="./media/tutorial-geofence/logic-app-designer.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

10. Inicie sesión en su cuenta de Outlook. Asegúrese de seleccionar **Sí** para permitir que la aplicación lógica acceda a la cuenta. Rellene los campos para enviar un correo electrónico.

    :::image type="content" source="./media/tutorial-geofence/logic-app-email.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

    >[!TIP]
    > Puede recuperar datos de la respuesta GeoJSON, como `geometryId` o `deviceId`, en las notificaciones de correo electrónico. Puede configurar Logic Apps para leer los datos enviados por Event Grid. Para más información sobre cómo configurar Logic Apps para usar y pasar datos de eventos en notificaciones por correo electrónico, consulte [Tutorial: Envío de notificaciones por correo electrónico sobre eventos de Azure IoT Hub mediante Event Grid y Logic Apps](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps).

11. En la esquina superior izquierda del Diseñador de aplicación lógica, seleccione **Guardar**.

Repita los pasos del 3 al 11 para crear una segunda aplicación lógica que notifique al responsable cuando el equipo salga del sitio de construcción. Asigne a la aplicación lógica el nombre `Equipment-Exit`.

## <a name="create-azure-maps-events-subscriptions"></a>Creación de suscripciones a eventos de Azure Maps

Azure Maps admite [tres tipos de eventos](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps). Aquí, tendremos que crear dos suscripciones a eventos diferentes: una para los eventos de entrada de la geovalla y otra para los eventos de salida de la geovalla.

Siga estos pasos para crear una suscripción a eventos para los eventos de entrada de la geovalla. Para suscribirse a los eventos de salida de la geovalla, puede repetir los pasos de la misma manera.

1. Vaya a su cuenta de Azure Maps. En el panel, seleccione **Suscripciones**. Seleccione el nombre de la suscripción y seleccione **Eventos** en el menú de configuración.

    :::image type="content" source="./media/tutorial-geofence/events-tab.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

2. Para crear una suscripción de eventos, seleccione **+ Suscripción de eventos** desde la página correspondiente.

    :::image type="content" source="./media/tutorial-geofence/create-event-subscription.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

3. En la página **Crear suscripción de eventos**, escriba los siguientes valores:
    * El **nombre** de la suscripción de eventos.
    * El **esquema de eventos**  debe ser *Esquema de Event Grid*.
    * **Nombre de tema del sistema** de esta suscripción a eventos, que en este caso es `Contoso-Construction`.
    * Para **Filtro para tipos de evento**, elija `Geofence Entered` como tipo de evento.
    * Para **Tipo de punto de conexión**, elija `Web Hook`.
    * Para **Punto de conexión**, copie la dirección URL de HTTP POST del punto de conexión de entrada de la aplicación lógica que creó en la sección anterior. Si olvidó guardarlo, simplemente puede volver al Diseñador de aplicación lógica y copiarlo desde el paso del desencadenador HTTP.

    :::image type="content" source="./media/tutorial-geofence/events-subscription.png" alt-text="Captura de pantalla de la creación de una aplicación lógica.":::

4. Seleccione **Crear**.

Repita los pasos 1 a 4 para el punto de conexión de salida de la aplicación lógica que creó en la sección anterior. En el paso 3, asegúrese de elegir `Geofence Exited` como tipo de evento.

## <a name="use-spatial-geofence-get-api"></a>Uso de Spatial Geofence Get API

Ahora, usaremos [Spatial Geofence Get API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) para enviar notificaciones por correo electrónico al responsable de operaciones cuando un elemento del equipo entre o salga de las geovallas.

Cada elemento del equipo tiene un `deviceId`. En este tutorial, vamos a realizar el seguimiento de un único equipo, cuyo identificador único es `device_1`.

En el siguiente diagrama se muestran las cinco ubicaciones del equipo a lo largo del tiempo, empezando por la ubicación de *inicio*, que se encuentra fuera de las geovallas. Para los fines de este tutorial, la ubicación de *inicio* no está definida, ya que no se consultará el dispositivo en esa ubicación.

Cuando se realice una consulta a [Spatial Geofence Get API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) con una ubicación de equipo que indica la entrada o salida de la geovalla inicial, Event Grid llama al punto de conexión de la aplicación lógica adecuado para enviar una notificación por correo electrónico al responsable de operaciones.

En cada una de las secciones siguientes se realizan solicitudes a la API con las cinco coordenadas de ubicación diferentes del equipo.

![Diagrama del mapa de geovallas en Azure Maps](./media/tutorial-geofence/geofence.png)

### <a name="equipment-location-1-47638237-122132483"></a>Ubicación del equipo 1 (47,638237,-122,132483)

1. Cerca de la parte superior de la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Especifique *Location 1*. Seleccione la colección que creó en la [sección Carga de datos GeoJSON de geovalla](#upload-geofencing-geojson-data) y, después, seleccione **Save** (Guardar).

2. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la dirección URL siguiente. Asegúrese de reemplazar `{Azure-Maps-Primary-Subscription-key}` por la clave de suscripción principal y `{udid}` por el `udid` guardado en la [sección Carga de datos GeoJSON de geovallas](#upload-geofencing-geojson-data).

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

3. Haga clic en **Send** (Enviar). El siguiente elemento GeoJSON aparecerá en la ventana de respuesta.

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638291,
          "nearestLon": -122.132483
        },
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.638053,
          "nearestLon": -122.13295
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ```

En la respuesta GeoJSON anterior, la distancia negativa desde la geovalla del sitio principal significa que el equipo está dentro de la geovalla. La distancia positiva desde la geovalla del sitio secundario significa que el equipo está fuera de la geovalla del sitio secundario. Dado que esta es la primera vez que este dispositivo se encuentra dentro de la geovalla del sitio principal, el parámetro `isEventPublished` se establece en `true`. El responsable de operaciones recibe una notificación por correo electrónico que indica que el equipo ha entrado en la geovalla.

### <a name="location-2-4763800-122132531"></a>Ubicación 2 (47,63800,-122,132531)

1. Cerca de la parte superior de la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Especifique *Location 2*. Seleccione la colección que creó en la [sección Carga de datos GeoJSON de geovalla](#upload-geofencing-geojson-data) y, después, seleccione **Save** (Guardar).

2. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la dirección URL siguiente. Asegúrese de reemplazar `{Azure-Maps-Primary-Subscription-key}` por la clave de suscripción principal y `{udid}` por el `udid` guardado en la [sección Carga de datos GeoJSON de geovallas](#upload-geofencing-geojson-data).

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

3. Haga clic en **Send** (Enviar). El siguiente elemento GeoJSON aparecerá en la ventana de respuesta:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637997,
          "nearestLon": -122.132399
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.63789,
          "nearestLon": -122.132809
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

En la respuesta GeoJSON anterior, el equipo permaneció en la geovalla del sitio principal y no entró en la geovalla del sitio secundario. Como resultado, el parámetro `isEventPublished` se establece en `false` y el responsable de operaciones no recibe ninguna notificación por correo electrónico.

### <a name="location-3-4763810783315048-12213336020708084"></a>Ubicación 3 (47,63810783315048,-122,13336020708084)

1. Cerca de la parte superior de la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Especifique *Location 3*. Seleccione la colección que creó en la [sección Carga de datos GeoJSON de geovalla](#upload-geofencing-geojson-data) y, después, seleccione **Save** (Guardar).

2. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la dirección URL siguiente. Asegúrese de reemplazar `{Azure-Maps-Primary-Subscription-key}` por la clave de suscripción principal y `{udid}` por el `udid` guardado en la [sección Carga de datos GeoJSON de geovallas](#upload-geofencing-geojson-data).

    ```HTTP
      https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
      ```

3. Haga clic en **Send** (Enviar). El siguiente elemento GeoJSON aparecerá en la ventana de respuesta:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638294,
          "nearestLon": -122.133359
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": -999.0,
          "nearestLat": 47.638161,
          "nearestLon": -122.133549
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

En la respuesta GeoJSON anterior, el equipo permaneció en la geovalla del sitio principal, pero entró en la geovalla del sitio secundario. Como resultado, el parámetro `isEventPublished` se establece en `true`. El responsable de operaciones recibe una notificación por correo electrónico que indica que el equipo ha entrado en la geovalla.

>[!NOTE]
>Si el equipo ha entrado en el sitio secundario después del horario laboral, no se publicará ningún evento y el responsable de operaciones no recibirá ninguna notificación.  

### <a name="location-4-47637988-1221338344"></a>Ubicación 4 (47,637988,-122,1338344)

1. Cerca de la parte superior de la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Especifique *Location 4*. Seleccione la colección que creó en la [sección Carga de datos GeoJSON de geovalla](#upload-geofencing-geojson-data) y, después, seleccione **Save** (Guardar).

2. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la dirección URL siguiente. Asegúrese de reemplazar `{Azure-Maps-Primary-Subscription-key}` por la clave de suscripción principal y `{udid}` por el `udid` guardado en la [sección Carga de datos GeoJSON de geovallas](#upload-geofencing-geojson-data).

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&userTime=2023-01-16&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

3. Haga clic en **Send** (Enviar). El siguiente elemento GeoJSON aparecerá en la ventana de respuesta:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637985,
          "nearestLon": -122.133907
        }
      ],
      "expiredGeofenceGeometryId": [
        "2"
      ],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

En la respuesta GeoJSON anterior, el equipo permaneció en la geovalla del sitio principal, pero ha salido de la geovalla del sitio secundario. Sin embargo,observe que el valor `userTime` es posterior a `expiredTime`, tal y como se define en los datos de geovalla. Como resultado, el parámetro `isEventPublished` se establece en `false` y el responsable de operaciones no recibe una notificación por correo electrónico.

### <a name="location-547637988-1221338344"></a>Ubicación 5 (47,637988,-122,1338344)

1. Cerca de la parte superior de la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Especifique *Location 5*. Seleccione la colección que creó en la [sección Carga de datos GeoJSON de geovalla](#upload-geofencing-geojson-data) y, después, seleccione **Save** (Guardar).

2. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la dirección URL siguiente. Asegúrese de reemplazar `{Azure-Maps-Primary-Subscription-key}` por la clave de suscripción principal y `{udid}` por el `udid` guardado en la [sección Carga de datos GeoJSON de geovallas](#upload-geofencing-geojson-data).

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

3. Haga clic en **Send** (Enviar). El siguiente elemento GeoJSON aparecerá en la ventana de respuesta:

    ```json
    {
      "geometries": [
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "1",
        "distance": -999.0,
        "nearestLat": 47.637985,
        "nearestLon": -122.133907
      },
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "2",
        "distance": 999.0,
        "nearestLat": 47.637945,
        "nearestLon": -122.133683
      }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

En la respuesta GeoJSON anterior, el equipo ha salido de la geovalla del sitio principal. Como resultado, el parámetro `isEventPublished` se establece en `true` y el responsable de operaciones recibe una notificación por correo electrónico que indica que el equipo ha salido de una geovalla.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administración de tipos de contenido en Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type)

> [!div class="nextstepaction"]
> [Envío de notificaciones por correo electrónico sobre eventos de Azure IoT Hub mediante Event Grid y Logic Apps](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps)

> [!div class="nextstepaction"]
> [Controladores de eventos de Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers)
