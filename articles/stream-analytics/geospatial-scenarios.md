---
title: Agregación geoespacial y de geovalla con Azure Stream Analytics
description: En este artículo se describe cómo usar Azure Stream Analytics para la agregación geoespacial y de geovalla.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 04/02/2019
ms.openlocfilehash: 90d8c70ac0a34470e8121d2234fd6b898cfed1ab
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130602"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>Escenarios de agregación geoespacial y de geovalla con Azure Stream Analytics

Con las funciones geoespaciales integradas, se puede usar Azure Stream Analytics con el fin de compilar aplicaciones para escenarios como la administración de flotas, un paseo por el uso compartido, automóviles conectados y seguimiento de activos.

## <a name="geofencing"></a>Geovalla

Azure Stream Analytics admite los cálculos de geovallas en tiempo real de baja latencia en la nube y en el entorno de ejecución de Azure IoT Edge.

### <a name="geofencing-scenario"></a>Escenario de geovalla

Una empresa de fabricación debe realizar un seguimiento de activos en sus edificios. Los dispositivos están equipados con un GPS y quieren recibir notificaciones si un dispositivo abandona una zona concreta.

Los datos de referencia que se usan en este ejemplo tienen la información de la geovalla para los edificios y dispositivos que se permiten en cada uno de los edificios. Recuerde que los datos de referencia pueden ser o estáticos o cambiar con lentitud. En este escenario se usan datos de referencia estáticos. Un flujo de datos emite continuamente el identificador de dispositivo y su posición actual.

### <a name="define-geofences-in-reference-data"></a>Definición de las geovallas en datos de referencia

Una geovalla se puede definir mediante un objeto de GeoJSON. Para los trabajos con versión de compatibilidad 1.2 y siguientes, las geovallas también se pueden definir mediante el formato Well Known Text (WKT) como `NVARCHAR(MAX)`. WKT es un estándar de Open Geospatial Consortium (OGC) que se utiliza para representar datos espaciales en formato de texto.

Las funciones geoespaciales integradas pueden usar las geovallas definidas para averiguar si un elemento está dentro o fuera de un polígono de geovalla específico.

La tabla siguiente es un ejemplo de datos de referencia de la geovalla que podrían almacenarse en Azure Blob Storage o en una tabla de SQL Azure. Todos los sitios se representan mediante un polígono geoespacial y todos los dispositivos se asocian con un identificador de sitio permitido.

|SiteID|SiteName|Geofence|AllowedDeviceID|
|------|--------|--------|---------------|
|1|"Redmond edificio 41"|"POLYGON((-122.1337357922017 47.63782998329432,-122.13373042778369 47.637634793257305,-122.13346757130023 47.637642022530954,-122.13348902897235 47.637508280806806,-122.13361777500506 47.637508280806806,-122.13361241058703 47.63732393354484,-122.13265754417773 47.63730947490855,-122.13266290859576 47.637519124743164,-122.13302232460376 47.637515510097955,-122.13301696018573 47.63764925180358,-122.13272728161212 47.63764925180358,-122.13274873928424 47.63784082716388,-122.13373579220172 47.63782998329432))"|"B"|
|2|"Redmond edificio 40"|"POLYGON((-122.1336154507967 47.6366745947009,-122.13361008637867 47.636483015064535,-122.13349206918201 47.636479400347675,-122.13349743360004 47.63636372927573,-122.13372810357532 47.63636372927573,-122.13373346799335 47.63617576323771,-122.13263912671528 47.63616491902258,-122.13264985555134 47.63635649982525,-122.13304682248554 47.636367344000604,-122.13305218690357 47.63650831807564,-122.13276250832996 47.636497473929516,-122.13277323716602 47.63668543881025,-122.1336154507967 47.6366745947009))"|"A"|
|3|"Redmond edificio 22"|"POLYGON((-122.13611660248233 47.63758544698554,-122.13635263687564 47.6374083293018,-122.13622389084293 47.63733603619712,-122.13622389084293 47.63717699101473,-122.13581619507266 47.63692757827657,-122.13559625393344 47.637046862778135,-122.13569281345798 47.637144458985965,-122.13570890671207 47.637314348246214,-122.13611660248233 47.63758544698554))"|"C"|

### <a name="generate-alerts-with-geofence"></a>Generar alertas con geovalla

Los dispositivos pueden emitir el identificador y la ubicación cada minuto a través de un flujo de datos llamado `DeviceStreamInput`. La tabla siguiente representa un flujo de datos de entrada.

|DeviceID|GeoPosition|
|--------|-----------|
|"A"|"POINT(-122.13292341559497 47.636318374032726)"|
|"B"|"POINT(-122.13338475554553 47.63743531308874)"|
|"C"|"POINT(-122.13354001095752 47.63627622505007)"|

Se puede escribir una consulta que conecta el flujo de datos del dispositivo con los datos de referencia de la geovalla y genera una alerta cada vez que un dispositivo se encuentre fuera de un edificio permitido.

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

La imagen siguiente representa las geovallas. Puede ver dónde están los dispositivos en función de la entrada de la transmisión de datos.

![Compilación de geovallas](./media/geospatial-scenarios/building-geofences.png)

El dispositivo "C" se encuentra dentro del identificador de edificio 2, que no está permitido según los datos de referencia. Este dispositivo debería estar localizado en el identificador de edificio 3. Al ejecutar este trabajo se generará una alerta para esta infracción específica.

### <a name="site-with-multiple-allowed-devices"></a>Sitio con varios dispositivos permitidos

Si un sitio permite varios dispositivos, se puede definir una matriz de identidades del dispositivo en `AllowedDeviceID` y se puede usar una función definida por el usuario en la cláusula `WHERE` para comprobar si la identidad del dispositivo del flujo de datos coincide con cualquier identidad del dispositivo en esa lista. Para obtener más información, consulte el tutorial [UDF de JavaScript](stream-analytics-javascript-user-defined-functions.md) para los trabajos en la nube y el tutorial [UDF de C#](stream-analytics-edge-csharp-udf.md) para trabajos de Edge.

## <a name="geospatial-aggregation"></a>Agregación geoespacial

Azure Stream Analytics admite la agregación geoespacial en tiempo real de baja latencia en la nube y en el entorno de ejecución de Azure IoT Edge.

### <a name="geospatial-aggregation-scenario"></a>Escenario de agregación geoespacial

Una empresa de taxis quiere compilar una aplicación en tiempo real para guiar a los conductores que buscan las zonas de las ciudades que experimentan una mayor demanda.

La empresa almacena las regiones lógicas de la ciudad como datos de referencia. Cada región se define con RegionID, RegionName y Geofence.

### <a name="define-the-geofences"></a>Definición de las geovallas

La tabla siguiente es un ejemplo de datos de referencia de la geovalla que podrían almacenarse en Azure Blob Storage o en una tabla de SQL Azure. Todas las regiones se representan mediante un polígono geoespacial, que se usa para crear una correlación con las solicitudes procedentes de los datos de transmisión.

Estos polígonos solo sirven de referencia y no representan las separaciones lógicas o físicas de ciudades reales.

|RegionID|RegionName|Geofence|
|--------|----------|--------|
|1|"SoHo"|"POLYGON((-74.00279525078275 40.72833625216264,-74.00547745979765 40.721929158663244,-74.00125029839018 40.71893680218994,-73.9957785919998 40.72521409075776,-73.9972377137039 40.72557184584898,-74.00279525078275 40.72833625216264))"|
|2|"Chinatown"|"POLYGON((-73.99712367114876 40.71281582267133,-73.9901070123658 40.71336881907936,-73.99023575839851 40.71452359088633,-73.98976368961189 40.71554823078944,-73.99551434573982 40.717337246783735,-73.99480624255989 40.718491949759304,-73.99652285632942 40.719109951574,-73.99776740131233 40.7168005470334,-73.99903340396736 40.71727219249899,-74.00193018970344 40.71938642421256,-74.00409741458748 40.71688186545551,-74.00051398334358 40.71517415773184,-74.0004281526551 40.714377212470005,-73.99849696216438 40.713450141693166,-73.99748845157478 40.71405192594819,-73.99712367114876 40.71281582267133))"|
|3|"Tribeca"|"POLYGON((-74.01091641815208 40.72583120006787,-74.01338405044578 40.71436586362705,-74.01370591552757 40.713617702123415,-74.00862044723533 40.711308107057235,-74.00194711120628 40.7194238654018,-74.01091641815208 40.72583120006787))"|

### <a name="aggregate-data-over-a-window-of-time"></a>Agregación de datos a través de una ventana de tiempo

La tabla siguiente contiene los datos de transmisión de "viajes".

|UserID|FromLocation|ToLocation|TripRequestedTime|
|------|------------|----------|-----------------|
|"A"|"POINT(-74.00726861389182 40.71610611981975)"|"POINT(-73.98615095917779 40.703107386025835)"|"2019-03-12T07:00:00Z"|
|"B"|"POINT(-74.00249841021645 40.723827238895666)"|"POINT(-74.01160699942085 40.71378884930115)"|"2019-03-12T07:01:00Z"|
|"C"|"POINT(-73.99680120565864 40.716439898624024)"|"POINT(-73.98289663412544 40.72582343969828)"|"2019-03-12T07:02:00Z"|
|"D"|"POINT(-74.00741090068288 40.71615626755086)"|"POINT(-73.97999843120539 40.73477895807408)"|"2019-03-12T07:03:00Z"|

La consulta siguiente conecta el flujo de dispositivo con los datos de referencia de la geovalla y calcula el número de solicitudes por región en una ventana de tiempo de 15 minutos cada minuto.

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

Esta consulta genera un recuento de solicitudes cada minuto durante los últimos 15 minutos por cada región que haya en la ciudad. Esta información se puede mostrar fácilmente mediante el panel de Power BI o se puede difundir a todos los conductores como mensajes de texto SMS mediante la integración con servicios como Azure Functions.

La imagen siguiente ilustra la salida de la consulta en el panel de Power BI. 

![Salida de resultado en el panel de Power BI](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>Pasos siguientes

* [Introducción a las funciones geoespaciales de Stream Analytics](stream-analytics-geospatial-functions.md)
* [Funciones geoespaciales (Azure Stream Analytics)](/stream-analytics-query/geospatial-functions)