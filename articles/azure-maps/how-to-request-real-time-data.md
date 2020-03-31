---
title: Solicitud de datos de tránsito en tiempo real | Microsoft Azure Maps
description: Solicitud de datos en tiempo real mediante Microsoft Azure Maps Mobility Service.
author: philmea
ms.author: philmea
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: d3145181a863bf8188dd0b0bb52cd2efc662ce2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335479"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Solicitud de datos en tiempo real mediante Azure Maps Mobility Service

En este artículo se muestra cómo usar Azure Maps [Mobility Service](https://aka.ms/AzureMapsMobilityService) para solicitar datos de tránsito en tiempo real.

En este artículo, aprenderá a:


 * Solicitar las siguientes llegadas en tiempo real para todas las líneas que llegan a una parada determinada.
 * Solicitar información en tiempo real de una estación de aparcamiento de bicicletas determinado.


## <a name="prerequisites"></a>Prerequisites

En primer lugar, debe tener una cuenta de Azure Maps y una clave de suscripción para realizar cualquier llamada a las API de transporte público de Azure Maps. Para más información, siga las instrucciones de [Crear una cuenta](quick-demo-map-app.md#create-an-account-with-azure-maps) para crear una cuenta de Azure Maps. Siga los pasos descritos en la sección de [obtención de la clave principal](quick-demo-map-app.md#get-the-primary-key-for-your-account) para obtener la clave principal de la cuenta. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).


Este artículo utiliza la [aplicación Postman](https://www.getpostman.com/apps) para generar llamadas REST. Puede usar cualquier entorno de desarrollo de API que prefiera.


## <a name="request-real-time-arrivals-for-a-stop"></a>Solicitud de llegadas en tiempo real en una parada

Para solicitar datos de las llegadas en tiempo real para una parada de transporte público determinada, deberá realizar una solicitud a la [API de llegadas en tiempo real](https://aka.ms/AzureMapsMobilityRealTimeArrivals) de [Mobility Service](https://aka.ms/AzureMapsMobilityService) de Azure Maps. Necesitará los valores de **metroID** y **stopID** para completar la solicitud. Para más información sobre cómo solicitar estos parámetros, consulte nuestra guía para [solicitar rutas de transporte público](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

Vamos a usar "522" como metroID, que es el identificador de metro del área "Seattle – Tacoma – Bellevue, WA". Use "522---2060603" como stopID, esta parada de autobús está en "Ne 24th St & 162nd Ave Ne, Bellevue WA". Para solicitar los datos de las siguientes cinco llegadas en tiempo real de esta parada, complete los pasos siguientes:

1. Abra la aplicación Postman y cree una colección para almacenar las solicitudes. Cerca de la parte superior de la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Collection** (Colección).  Asigne un nombre a la colección y seleccione el botón **Create** (Crear).

2. Para crear la solicitud, seleccione **New** (Nuevo) otra vez. En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Seleccione la colección que creó en el paso anterior, como la ubicación en la que se va a guardar la solicitud. Después, seleccione **Guardar**.

    ![Creación de una solicitud en Postman](./media/how-to-request-transit-data/postman-new.png)

3. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET. Reemplace `{subscription-key}` por la clave principal de Azure Maps.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. Si la solicitud es correcta, recibirá la respuesta siguiente.  Observe que el parámetro "scheduleType" define si la hora de llegada estimada se basa en datos en tiempo real o en datos estáticos.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```


## <a name="real-time-data-for-bike-docking-station"></a>Datos en tiempo real para la estación de aparcamiento de bicicletas

La [API para obtener información de las estaciones de depósito](https://aka.ms/AzureMapsMobilityTransitDock) permite a los usuarios solicitar información estática y en tiempo real. Por ejemplo, los usuarios pueden solicitar información de disponibilidad y espacios libres para una bicicleta o una scooter en la estación de depósito. La [API para obtener información de las estaciones de depósito](https://aka.ms/AzureMapsMobilityTransitDock) también forma parte de [Mobility Service](https://aka.ms/AzureMapsMobilityService) de Azure Maps.

Para realizar una solicitud a la [API para obtener información de las estaciones de depósito](https://aka.ms/AzureMapsMobilityTransitDock), necesitará el valor de **dockId** de esa estación. Para obtener el identificador de la estación; realice una solicitud de búsqueda a la [API para obtener información de transporte público cercano](https://aka.ms/AzureMapsMobilityNearbyTransit) y establecer el parámetro **objectType** asignado en "bikeDock". Siga los pasos siguientes para obtener datos en tiempo real de una estación de aparcamiento de bicicletas.


### <a name="get-dock-id"></a>Obtener el identificador de aparcamiento

Para obtener el valor de **dockID**, siga estos pasos y realice una solicitud a la API para obtener el tránsito cercano:

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get dock ID** (Obtener id. de aparcamiento).

2.  En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud y haga clic en **Send** (Enviar).
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Si la solicitud es correcta, recibirá la respuesta siguiente. Observe que ahora tenemos el valor **id** en la respuesta, que se puede usar posteriormente como parámetro de consulta en la solicitud a la API para obtener información de aparcamiento de tránsito.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 31,
                    "lastUpdated": "2019-09-07T00:55:19Z",
                    "operatorInfo": {
                        "id": "121---80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Obtener el estado de aparcamiento de bicicletas en tiempo real

Siga los pasos siguientes para realizar una solicitud a la API para obtener información de aparcamiento de tránsito y obtener datos en tiempo real para el aparcamiento seleccionado.

1. En Postman, haga clic en **New Request** (Nueva solicitud)  | **GET request** (Solicitud GET) y asígnele el nombre **Get real-time dock data** (Obtener datos de aparcamiento en tiempo real).

2.  En la pestaña Builder (Generador), seleccione el método HTTP **GET**, escriba la siguiente dirección URL de solicitud y haga clic en **Send** (Enviar).
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Si la solicitud se realizó correctamente, recibirá una respuesta con la estructura siguiente:

    ```JSON
    {
        "availableVehicles": 0,
        "vacantLocations": 31,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-09-07T00:55:19Z",
        "operatorInfo": {
            "id": "121---80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Pasos siguientes

Aprenda a solicitar datos de tránsito mediante Mobility Service:

> [!div class="nextstepaction"]
> [Cómo solicitar datos de tránsito](how-to-request-transit-data.md)

Explore la documentación de la API de Azure Maps Mobility Service:

> [!div class="nextstepaction"]
> [Documentación de Mobility Service API](https://aka.ms/AzureMapsMobilityService)
