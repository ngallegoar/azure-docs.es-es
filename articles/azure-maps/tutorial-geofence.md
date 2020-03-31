---
title: 'Tutorial: Creación de una geovalla y seguimiento de dispositivos en un mapa | Microsoft Azure Maps'
description: Aprenderá a configurar una geovalla y a realizar un seguimiento de los dispositivos en relación con la geovalla mediante el servicio espacial de Microsoft Azure Maps.
author: philmea
ms.author: philmea
ms.date: 1/15/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 126829f12d71e40511c26e781cb191988c1d031e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333865"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Tutorial: Configuración de una geovalla con Azure Maps

Este tutorial le guiará por los pasos básicos para configurar la geovalla con Azure Maps. Considere este escenario: un jefe de obra tiene que supervisar un equipo potencialmente peligroso. El jefe de obra debe asegurarse de que el equipo permanezca en el área de construcción general seleccionada. Este área de construcción general es un parámetro fijo. La normativa exige que el equipo permanezca dentro de los límites de este parámetro y cualquier infracción se comunicará al responsable de operaciones.  

Vamos a usar Data Upload API para almacenar una geovalla y Geofence API para comprobar la ubicación del equipo en relación con la geovalla. Tanto Data Upload API como Geofence API son de Azure Maps. También usaremos Azure Event Grid para transmitir los resultados de la geovalla y establecer una notificación basada en los resultados de la geovalla. Para más información sobre Event Grid, consulte [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).

En este tutorial se explica cómo:

> [!div class="checklist"]
> * Cargar el área de la geovalla en el servicio de datos de Azure Maps con Data Upload API.
> *   Configurar una instancia de Event Grid para controlar los eventos de la geovalla.
> *   Configurar el controlador de eventos de la geovalla.
> *   Configurar alertas en respuesta a eventos de la geovalla con Logic Apps.
> *   Usar las API del servicio de geovalla de Azure Maps para realizar un seguimiento de si un recurso de construcción está dentro del sitio de construcción o no.


## <a name="prerequisites"></a>Prerrequisitos

### <a name="create-an-azure-maps-account"></a>Crear una cuenta de Azure Maps 

Siga las instrucciones de [Crear una cuenta](quick-demo-map-app.md#create-an-account-with-azure-maps) para crear una suscripción de cuenta de Azure Maps en el plan de tarifa S1. Los pasos descritos en [Obtención de la clave principal](quick-demo-map-app.md#get-the-primary-key-for-your-account) muestran cómo recuperar la clave principal de la cuenta. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).

## <a name="upload-geofences"></a>Carga de geovallas

Se asume que la geovalla principal es subsitio1, que tiene un tiempo de expiración establecido. Se pueden crear más geovallas anidadas según sus requisitos. Estos conjuntos de vallas se pueden usar para realizar el seguimiento de diferentes áreas de construcción dentro del área de construcción general. Por ejemplo, en el subsitio 1 se podría realizar el trabajo durante las semanas 1 a 4 de la programación y en el subsitio 2 el trabajo se llevaría a cabo durante las semanas 5 a 7. Se pueden cargar todas estas vallas como un único conjunto de datos al principio del proyecto. Estas vallas se pueden utilizar para realizar un seguimiento de las reglas basadas en el tiempo y el espacio. 

Para cargar la geovalla en el sitio de construcción mediante Data Upload API, se va a usar la aplicación Postman. Instale la [aplicación Postman](https://www.getpostman.com/) y cree una cuenta gratuita. 

Una vez instalada la aplicación Postman, siga estos pasos para cargar la geovalla del sitio de construcción mediante Data Upload API de Azure Maps.

1. Abra la aplicación Postman, haga clic en New | Create new (Nuevo | Crear nuevo) y seleccione Request (Solicitud). Escriba un nombre de solicitud para la carga de datos de geovalla, seleccione una colección o carpeta donde guardarlo y haga clic en Save (Guardar).

    ![Carga de geovallas con Postman](./media/tutorial-geofence/postman-new.png)

2. Seleccione el método POST HTTP en la pestaña del generador e introduzca la siguiente URL para realizar una solicitud POST.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    El parámetro GEOJSON en la ruta de acceso de la dirección URL representa el formato de datos de los datos que se cargan.

3. Haga clic en **Params** (Parámetros) y escriba los siguientes pares de clave-valor que se usarán para la dirección URL de la solicitud POST. Reemplace {subscription-key} por la clave de suscripción de Azure Maps, también llamada clave principal.
   
    ![Parámetros de carga de datos (geovalla) en Postman](./media/tutorial-geofence/postman-key-vals.png)

4. Haga clic en **Body** (Cuerpo), seleccione el formato de entrada sin procesar y elija JSON como formato de entrada en la lista desplegable. Proporcione el siguiente código JSON como datos que se van a cargar:

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
              "expiredTime": "2019-01-15T00:00:00",
              "validityPeriod": [
                {
                  "startTime": "2019-01-08T01:00:00",
                  "endTime": "2019-01-08T17:00:00",
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

5. Haga clic en Send (Enviar) y revise el encabezado de la respuesta. Tras una solicitud correcta, el encabezado **Location** (Ubicación) contendrá el identificador URI de estado. El identificador URI de estado tendrá el siguiente formato. El valor de uploadStatusId no está entre { }. Es una práctica común usar { } para mostrar los valores que el usuario debe especificar o los valores que son diferentes para usuarios diferentes.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Copie el identificador URI de estado y anexe la clave de suscripción. El formato del identificador URI de estado debe ser como el siguiente. Tenga en cuenta que, en el formato siguiente, debería cambiar {subscription-key}, sin incluir las llaves { }, por su clave de suscripción.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Para obtener `udId`, abra una pestaña nueva en la aplicación Postman y seleccione el método GET HTTP en la pestaña del generador. Realice una solicitud GET en el identificador URI de estado del paso anterior. Si la carga de datos se realizó correctamente, recibirá un UDID en el cuerpo de la respuesta. Copie el UDID para su uso posterior.

   ```JSON
   {
    "status": "Succeeded",
    "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udId}?api-version=1.0"
   }
   ```

## <a name="set-up-an-event-handler"></a>Configuración de un controlador de eventos

En esta sección, crearemos un controlador de eventos que recibe notificaciones. Este controlador de eventos debe notificar al responsable de operaciones los eventos de entrada y salida de cualquier equipo.

Se crearán dos servicios de [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps) para controlar los eventos de entrada y salida. Cuando se desencadenen los eventos de Logic Apps, se desencadenarán más eventos en secuencia. La idea es enviar alertas, en este caso mensajes de correo electrónico, al responsable de operaciones. La siguiente ilustración muestra la creación de una aplicación lógica para el evento de entrada de la geovalla. De forma similar, puede crear otro para el evento de salida. Para más información, puede ver todos los [controladores de eventos admitidos](https://docs.microsoft.com/azure/event-grid/event-handlers).

1. Cree una aplicación lógica en Azure Portal, selecciónela en Azure Marketplace y, después, seleccione el botón **Crear**.

   ![Creación de aplicaciones lógicas de Azure para controlar eventos de geovalla](./media/tutorial-geofence/logic-app.png)

2. En el menú de configuración del servicio de Logic Apps, vaya a **Diseñador de aplicación lógica**.

3. Agregue el desencadenador de solicitud HTTP y, a continuación, seleccione "Nuevo paso". En el conector de Outlook, seleccione "Enviar un correo electrónico" como una acción.
  
   ![Esquema de Logic Apps](./media/tutorial-geofence/logic-app-schema.png)

4. Rellene los campos para enviar un correo electrónico. Deje la dirección URL HTTP, que se generará automáticamente después de hacer clic en "Guardar".

   ![Generación de un punto de conexión de Logic Apps](./media/tutorial-geofence/logic-app-endpoint.png)

5. Guarde la aplicación lógica para generar el punto de conexión de la dirección URL HTTP y copie dicha dirección.

## <a name="create-an-azure-maps-events-subscription"></a>Creación de una suscripción a eventos de Azure Maps

Azure Maps admite tres tipos de eventos. Puede echar un vistazo a los tipos de eventos de Azure Maps admitidos [aquí](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps). Necesitamos dos suscripciones de eventos diferentes, una para el evento de entrada y otra para los eventos de salida.

Siga estos pasos para crear una suscripción de eventos para los eventos de entrada de la geovalla. Puede suscribirse a eventos de salida de la geovalla de forma similar.

1. Vaya a su cuenta de Azure Maps. En el panel, seleccione Suscripciones. Haga clic en el nombre de la suscripción y seleccione **Eventos** en el menú de configuración.

   ![Events (Eventos) en la cuenta de Azure Maps](./media/tutorial-geofence/events-tab.png)

2. Para crear una suscripción de eventos, seleccione la suscripción de eventos desde la página correspondiente.

   ![Creación de una suscripción a eventos de Azure Maps](./media/tutorial-geofence/create-event-subscription.png)

3. Asigne un nombre a la suscripción de eventos y suscríbase al tipo de evento de entrada. Ahora, seleccione Webhook como "Tipo de punto de conexión". Haga clic en "Seleccionar un punto de conexión" y copie el punto de conexión de la dirección URL HTTP de la aplicación lógica en "{Endpoint}".

   ![Detalles de la suscripción a Events (Eventos) de Azure Maps](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Uso de Geofence API

Puede usar Geofence API para comprobar si un **dispositivo** (en este caso, un equipo) está dentro o fuera de una geovalla. Vamos a realizar una consulta a Geofence GET API en diferentes ubicaciones donde un equipo determinado se ha movido con el tiempo. En la figura siguiente se muestran cinco ubicaciones con cinco equipos de construcción. 

> [!Note]
> El escenario y el comportamiento se basan en el mismo **id. de dispositivo**, de modo que se reflejan las cinco ubicaciones diferentes como en la siguiente ilustración.

"deviceId" es un identificador único que se proporciona para el dispositivo en la solicitud GET, al consultar su ubicación. Cuando realiza una solicitud asincrónica a la **geovalla de búsqueda - GET API**, "deviceId" ayuda a publicar eventos de geovalla para ese dispositivo, con respecto a la geovalla especificada. En este tutorial, hemos realizado solicitudes asincrónicas a la API con un "deviceId" único. Las solicitudes del tutorial se realizan en orden cronológico, como en el diagrama. La propiedad "isEventPublished" de la respuesta se publica siempre que un dispositivo entra o sale de la geovalla. No es necesario registrar un dispositivo para continuar con este tutorial.

Vuelva a examinar el diagrama. Cada una de estas cinco ubicaciones se utiliza para evaluar el cambio de estado de entrada y salida de la geovalla frente a la misma. Si se produce un cambio de estado, el servicio de geovalla desencadena un evento, que envía Event Grid a la aplicación lógica. Como resultado, el responsable de operaciones recibirá la correspondiente notificación de entrada o salida por correo electrónico.

![Mapa de geovallas en Azure Maps](./media/tutorial-geofence/geofence.png)

En la aplicación Postman, abra una nueva pestaña en la misma colección que ha creado anteriormente. Seleccione el método GET HTTP en la pestaña del generador:

A continuación se presentan cinco solicitudes HTTP GET de Geofence API, con diferentes coordenadas de ubicación del equipo. Las coordenadas se observan en orden cronológico. Cada solicitud va seguida por el cuerpo de la respuesta.
 
1. Ubicación 1:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Consulta de la geovalla 1](./media/tutorial-geofence/geofence-query1.png)

   En la respuesta anterior, la distancia negativa desde la geovalla principal significa que el equipo está dentro de la geovalla. La distancia positiva desde la geovalla del subsitio significa que el equipo está fuera de la geovalla del subsitio. 

2. Ubicación 2: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Consulta de la geovalla 2](./media/tutorial-geofence/geofence-query2.png)

   Si se observa cuidadosamente la respuesta anterior de JSON, el equipo está fuera del subsitio, pero dentro de la valla principal. No se desencadena ningún evento y no se envía ningún correo electrónico.

3. Ubicación 3: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Consulta de la geovalla 3](./media/tutorial-geofence/geofence-query3.png)

   Se ha producido un cambio de estado y ahora el equipo se encuentra dentro de las geovallas principal y secundaria. Este cambio hace que se publique un evento y que se envíe un correo electrónico de notificación al responsable de operaciones.

4. Ubicación 4: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Consulta de la geovalla 4](./media/tutorial-geofence/geofence-query4.png)

   Al observar atentamente la respuesta correspondiente, se puede observar que aquí no se publica ningún evento a pesar de que el equipo haya salido de la geovalla del subsitio. Si se observa la hora especificada por el usuario en la solicitud GET, se puede ver que la geovalla del subsitio ha expirado en relación a esta hora. El equipo todavía sigue en la geovalla principal. También puede ver el identificador de geometría de la geovalla del subsitio en `expiredGeofenceGeometryId` en el cuerpo de respuesta.


5. Ubicación 5:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Consulta de la geovalla 5](./media/tutorial-geofence/geofence-query5.png)

   Puede ver que el equipo ha salido de la geovalla del sitio de construcción principal. Se publicará un evento y se enviará un correo electrónico de alerta al responsable de operaciones.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido cómo configurar una geovalla al cargarla en Azure Maps y un servicio de datos mediante Data Upload API. También ha aprendido a usar Event Grid de Azure Maps para suscribirse y controlar los eventos de la geovalla. 

* Consulte [Control de tipos de contenido en Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type), para obtener información sobre cómo usar Logic Apps para analizar JSON y crear una lógica más compleja.
* Para más información acerca de los controladores de eventos de Event Grid, consulte [Controladores de eventos de Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers).
