---
title: 'Tutorial: Búsqueda y visualización de rutas para modos de desplazamiento específicos mediante Microsoft Azure Maps'
description: Aprenda a utilizar Azure Maps para encontrar y mostrar rutas para modos de desplazamiento específicos.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/10/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: a5b8dba920db0227c400e62ef7ddaf718d27c78a
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90085066"
---
# <a name="tutorial-find-and-display-routes-for-different-modes-of-travel-using-azure-maps"></a>Tutorial: Búsqueda y visualización de rutas para diferentes modos de desplazamiento mediante Azure Maps

En este tutorial se muestra cómo utilizar el [servicio Azure Maps Route](https://docs.microsoft.com/rest/api/maps/route) y [Control de mapa](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control) para mostrar direcciones de ruta para vehículos privados y comerciales (camiones) con el tipo de carga `USHazmatClass2`. Además, le mostraremos cómo visualizar datos de tráfico en tiempo real en un mapa. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear y mostrar el control de mapa en una página web.
> * Representar datos de tráfico en tiempo real en un mapa.
> * Solicitar y mostrar las rutas de vehículos privados y comerciales en un mapa.

## <a name="prerequisites"></a>Requisitos previos

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).

3. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](how-to-manage-authentication.md).

Puede obtener el código fuente completo para el ejemplo [aquí](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html). Se puede encontrar un ejemplo dinámico [aquí](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel).

## <a name="create-a-new-web-page-using-the-map-control-api"></a>Crear una nueva página web con Map Control API

En los pasos siguientes se muestra cómo crear y mostrar el Control de mapa en una página web.

1. En la máquina local, cree un nuevo archivo y asígnele el nombre **MapTruckRoute.html**.
2. Copie y pegue el siguiente marcado HTML en el archivo.

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```

     El encabezado HTML incluye los archivos de recursos CSS y JavaScript hospedados por la biblioteca de Control de mapa de Azure. El evento `onload` del cuerpo llama a la función `GetMap`. En el paso siguiente, agregaremos el código de inicialización del Control de mapa.

3. Agregue el siguiente código JavaScript a la función `GetMap`. Reemplace la cadena `<Your Azure Maps Key>` por la clave principal que copió de la cuenta de Maps.

    ```JavaScript
    //Instantiate a map object
    var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
            authType: 'subscriptionKey',
            subscriptionKey: '<Your Azure Maps Key>'
        }
    });
    ```

4. Guarde el archivo y ábralo en el explorador. Se muestra un ejemplo.

    :::image type="content" source="./media/tutorial-prioritized-routes/basic-map.png" alt-text="Representación de mapa básica del Control de mapa":::

## <a name="render-real-time-traffic-data-on-a-map"></a>Representar datos de tráfico en tiempo real en un mapa.

1. Anexe el siguiente código JavaScript a la función `GetMap`. Este código implementa el controlador de eventos `ready` del Control de mapa. El resto del código de este tutorial se colocará dentro del controlador de eventos `ready`.

    ```javascript
    map.events.add("ready", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```

    En el controlador de eventos `ready` del mapa, la configuración del flujo de tráfico en el mapa se ha establecido en `relative`, que es la velocidad de la carretera en relación con el flujo libre. Para ver más opciones de tráfico, consulte la [interfaz de TrafficOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions?view=azure-maps-typescript-latest&preserve-view=false).

2. Guarde el archivo **MapTruckRoute.html** y actualice la página en el explorador. Si mira de cerca cualquier ciudad (por ejemplo, Los Ángeles), verá que las calles muestran los datos actuales de flujo de tráfico.

    :::image type="content" source="./media/tutorial-prioritized-routes/traffic-map.png" alt-text="Representación de mapa básica del Control de mapa":::

<a id="queryroutes"></a>

## <a name="define-route-display-rendering"></a>Definición de la representación de la visualización de ruta

En este tutorial, se calcularán y se representarán dos rutas en el mapa. La primera ruta se calculará para un vehículo privado (automóvil). La segunda ruta se calculará para que un vehículo comercial (camión) muestre la diferencia entre los resultados. Cuando se representa, el mapa muestra un icono de símbolo para los puntos inicial y final de la ruta, así como geometrías de líneas de ruta con distintos colores para cada ruta de acceso. Para obtener más información sobre cómo agregar capas de línea, vea [Adición de una capa de línea al mapa](map-add-line-layer.md). Para obtener más información acerca de las capas de símbolo, consulte [Adición de una capa de símbolo a un mapa](map-add-pin.md).

1. En el controlador de eventos `ready` del Control de mapa, anexe el código siguiente.

    ```JavaScript

    //Create a data source and add it to the map.
    datasource = new atlas.source.DataSource();
    map.sources.add(datasource);

    //Add a layer for rendering the route lines and have it render under the map labels.
    map.layers.add(new atlas.layer.LineLayer(datasource, null, {
        strokeColor: ['get', 'strokeColor'],
        strokeWidth: ['get', 'strokeWidth'],
        lineJoin: 'round',
        lineCap: 'round'
    }), 'labels');

    //Add a layer for rendering point data.
    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: ['get', 'icon'],
            allowOverlap: true
        },
        textOptions: {
            textField: ['get', 'title'],
            offset: [0, 1.2]
        },
        filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']] //Only render Point or MultiPoints in this layer.
    }));

    ```

    En el controlador de eventos `ready` del control de mapa, se creará un origen de datos para almacenar la ruta de principio a fin. Para recuperar la anchura y el color de línea a partir de las propiedades de la característica de línea de ruta, se utilizan [expresiones](data-driven-style-expressions-web-sdk.md). Para asegurarse de que la línea de ruta no cubre las etiquetas de la carretera, hemos pasado un segundo parámetro con el valor de `'labels'`.

    A continuación, se crea una capa de símbolo y se adjunta al origen de datos. Esta capa especifica cómo se representarán los puntos inicial y final. En este caso, se han agregado expresiones para recuperar la imagen del icono e información de la etiqueta de texto a partir de las propiedades de cada objeto de punto. Para obtener más información sobre las expresiones, consulte [Expresiones de estilo basadas en datos](data-driven-style-expressions-web-sdk.md).

2. Establezca el punto inicial en una empresa ficticia de Seattle llamada Fabrikam y el punto final en una oficina de Microsoft.  En el controlador de eventos `ready` del Control de mapa, anexe el código siguiente.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-round-blue'
    });

    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });

    ```

    Este código crea dos [objetos de punto de GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) para representar los puntos inicial y final, que se agregan al origen de datos.

    El último bloque de código establece la vista de la cámara con la latitud y longitud de los puntos inicial y final. Los puntos inicial y final se agregan al origen de datos. El rectángulo delimitador de los puntos inicial y final se calcula utilizando la función `atlas.data.BoundingBox.fromData`. Este rectángulo delimitador se usa para establecer la vista de las cámaras del mapa sobre la ruta completa mediante la función `map.setCamera`. Para compensar las dimensiones de píxeles de los iconos de símbolos, se agrega relleno. Para obtener más información sobre la propiedad setCamera del Control de mapa, vea la propiedad [setCamera(CameraOptions | CameraBoundsOptions & AnimationOptions)](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-&preserve-view=false).

3. Guarde el archivo **TruckRoute.html** y actualice el explorador. Ahora el mapa se centra en Seattle. La chincheta azul en forma de lágrima marca el punto inicial. La chincheta azul redonda marca el punto final.

   :::image type="content" source="./media/tutorial-prioritized-routes/pins-map.png" alt-text="Representación de mapa básica del Control de mapa":::

<a id="multipleroutes"></a>

## <a name="request-and-display-private-and-commercial-vehicle-routes-on-a-map"></a>Solicitar y mostrar las rutas de vehículos privados y comerciales en un mapa

En esta sección se muestra cómo usar el servicio Azure Maps Route para obtener direcciones de un punto a otro según el modo de transporte. Se utilizarán dos modos de transporte: camión y automóvil.

>[!TIP]
>El servicio Route proporciona las API para planear rutas que sean *más rápidas*, *más cortas*, *ecológicas* o *emocionantes* en función de la distancia, las condiciones del tráfico y el modo de transporte utilizado. Este servicio también permite a los usuarios planear rutas futuras según las condiciones del tráfico histórico. Los usuarios pueden ver la predicción de las duraciones de ruta en un momento dado. Para más información, consulte [Get Route directions API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

1. En la función `GetMap`, dentro del controlador de eventos `ready` del control, agregue lo siguiente al código de JavaScript.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   `SubscriptionKeyCredential` crea un `SubscriptionKeyCredentialPolicy` para autenticar las solicitudes HTTP en Azure Maps con la clave de suscripción. `atlas.service.MapsURL.newPipeline()` toma la directiva `SubscriptionKeyCredential` y crea una instancia de [canalización](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline). `routeURL` representa una dirección URL para las operaciones [Route](https://docs.microsoft.com/rest/api/maps/route) de Azure Maps.

2. Después de configurar las credenciales y la dirección URL, agregue el siguiente código JavaScript para crear una ruta del punto inicial al final. Esta ruta se creará y mostrará para un camión que transporta una carga clasificada como `USHazmatClass2`.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request for a truck vehicle type
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates,{
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```

    El código anterior consulta el servicio Route de Azure Maps mediante [Route Directions API de Azure Maps](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl#calculateroutedirections-aborter--geojson-position----calculateroutedirectionsoptions-). A continuación, se extrae la línea de ruta de la colección de características de GeoJSON de la respuesta extraída con el método `geojson.getFeatures()`. Por último, se agregará la línea de ruta al origen de datos. Se agrega en el índice 0 para garantizar que la ruta del camión se represente antes que cualquier otra línea en el origen de datos, ya que el cálculo de la ruta de camión suele ser más lento que el cálculo de la ruta de automóvil. Si la línea de ruta del camión se agrega al origen de datos después de la ruta del turismo, se representará sobre ella. Se agregan dos propiedades a la línea de ruta de camión: un color de trazo azul y un ancho del trazo de nueve píxeles.

    >[!TIP]
    > Para ver todas las opciones y valores posibles de Route Directions API de Azure Maps, consulte [Parámetros de URI para Post Route Directions](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#uri-parameters).

3. A continuación, anexe el siguiente código JavaScript con el fin de crear una ruta para un automóvil.

    ```JavaScript
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {

        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source. This will add the car route after the truck route.  
        datasource.add(routeLine);
    });
    ```

    El código anterior consulta el servicio de enrutamiento de Azure Maps mediante [Route Directions API de Azure Maps](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl#calculateroutedirections-aborter--geojson-position----calculateroutedirectionsoptions-). A continuación, se extrae la línea de ruta de la colección de características de GeoJSON de la respuesta extraída con el método `geojson.getFeatures()`. Por último, se agregará la línea de ruta al origen de datos. Se agregan dos propiedades a la línea de ruta de camión: un color de trazo púrpura y un ancho de trazo de cinco píxeles.

4. Guarde el archivo **TruckRoute.html** y actualice el explorador web. El mapa debería mostrar ahora las rutas de camión y de automóvil.

    :::image type="content" source="./media/tutorial-prioritized-routes/prioritized-routes.png" alt-text="Representación de mapa básica del Control de mapa":::

    La ruta de camión se muestra mediante una línea gruesa azul. La ruta de automóvil se muestra mediante una línea fina de color púrpura. La ruta de automóvil transcurre por Lake Washington a través de la I-90 y pasa por túneles bajo zonas residenciales. Como los túneles están cerca de las zonas residenciales, se restringe el transporte de residuos peligrosos. La ruta de camión, que especifica el tipo de carga `USHazmatClass2`, se redirige a una autopista diferente.

    Puede obtener el código fuente completo para el ejemplo [aquí](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html). Se puede encontrar un ejemplo dinámico [aquí](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel).

## <a name="next-steps"></a>Pasos siguientes

En el siguiente tutorial se muestra el proceso de creación de un localizador de almacenamiento sencillo mediante Azure Maps.

> [!div class="nextstepaction"]
> [Creación de un localizador de almacén mediante Azure Maps](./tutorial-create-store-locator.md)
