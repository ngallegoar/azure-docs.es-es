---
title: Introducción a las funciones geoespaciales de Azure Stream Analytics
description: En este artículo se describen las funciones geoespaciales que se usan en los trabajos de Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: f47f34b60c858bb9a0feafd25176e4a811046630
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75426224"
---
# <a name="introduction-to-stream-analytics-geospatial-functions"></a>Introducción a las funciones geoespaciales de Stream Analytics

Las funciones geoespaciales en Azure Stream Analytics permiten el análisis en tiempo real de datos geoespaciales de streaming. Con sólo unas pocas líneas de código, puede desarrollar una solución de calidad de producción para escenarios complejos. 

Algunos ejemplos de escenarios que pueden beneficiarse de las funciones geoespaciales incluyen:

* Viajes con vehículo compartido
* Administración de flotas
* Seguimiento de activos
* Geovalla
* Seguimiento de teléfonos a través de sitios celulares

Lenguaje de consulta de Stream Analytics tiene siete funciones geoespaciales integradas: **CreateLineString**, **CreatePoint**, **CreatePolygon**, **ST_DISTANCE**, **ST_OVERLAPS**, **ST_INTERSECTS** y **ST_WITHIN**.

## <a name="createlinestring"></a>CreateLineString

La función `CreateLineString` acepta puntos y devuelve un LineString de GeoJSON, que se puede representar como una línea en un mapa. Debe tener al menos dos puntos para crear un LineString. Los puntos de LineString se conectan en orden.

La siguiente consulta usa `CreateLineString` para crear un elemento LineString con tres puntos. Se crea el primer punto de transmisión a partir de datos de entrada de streaming, mientras que los otros dos se crean manualmente.

```SQL 
SELECT  
     CreateLineString(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5))  
FROM input  
```  

### <a name="input-example"></a>Ejemplo de entrada  
  
|latitude|longitude|  
|--------------|---------------|  
|3.0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Ejemplo de salida  

 {"type" : "LineString", "coordinates" : [ [-10.2, 3.0], [10.0, 10.0], [10.5, 10.5] ]}

 {"type" : "LineString", "coordinates" : [ [20.2321, -87.33], [10.0, 10.0], [10.5, 10.5] ]}

Para obtener más información, consulte la referencia sobre [CreateLineString](https://docs.microsoft.com/stream-analytics-query/createlinestring).

## <a name="createpoint"></a>CreatePoint

La función `CreatePoint` acepta una latitud y una longitud y devuelve un punto de GeoJSON que se puede trazar en un mapa. Las latitudes y longitudes deben ser del tipo de datos **float**.

La siguiente consulta de ejemplo usa `CreatePoint` para crear un punto mediante latitudes y longitudes a partir de datos de entrada de streaming.

```SQL 
SELECT  
     CreatePoint(input.latitude, input.longitude)  
FROM input 
```  

### <a name="input-example"></a>Ejemplo de entrada  
  
|latitude|longitude|  
|--------------|---------------|  
|3.0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Ejemplo de salida
  
 {"type" : "Point", "coordinates" : [-10.2, 3.0]}  
  
 {"type" : "Point", "coordinates" : [20.2321, -87.33]}  

Para obtener más información, consulte la referencia sobre [CreatePoint](https://docs.microsoft.com/stream-analytics-query/createpoint).

## <a name="createpolygon"></a>CreatePolygon

La función `CreatePolygon` acepta puntos y devuelve un registro de polígono de GeoJSON. El orden de los puntos debe seguir una orientación de anillo hacia la derecha o estar en sentido antihorario. Imagine que camina de un punto a otro en el orden en que se declaran. El centro del polígono estaría a su izquierda todo el tiempo.

La siguiente consulta de ejemplo usa `CreatePolygon` para crear un polígono de tres puntos. Los primeros dos puntos se crean manualmente, y el último se crea a partir de datos de entrada.

```SQL 
SELECT  
     CreatePolygon(CreatePoint(input.latitude, input.longitude), CreatePoint(10.0, 10.0), CreatePoint(10.5, 10.5), CreatePoint(input.latitude, input.longitude))  
FROM input  
```  

### <a name="input-example"></a>Ejemplo de entrada  
  
|latitude|longitude|  
|--------------|---------------|  
|3.0|-10.2|  
|-87.33|20.2321|  
  
### <a name="output-example"></a>Ejemplo de salida  

 {"type" : "Polygon", "coordinates" : [[ [-10.2, 3.0], [10.0, 10.0], [10.5, 10.5], [-10.2, 3.0] ]]}
 
 {"type" : "Polygon", "coordinates" : [[ [20.2321, -87.33], [10.0, 10.0], [10.5, 10.5], [20.2321, -87.33] ]]}

Para obtener más información, consulte la referencia sobre [CreatePolygon](https://docs.microsoft.com/stream-analytics-query/createpolygon).


## <a name="st_distance"></a>ST_DISTANCE
La función `ST_DISTANCE` devuelve la distancia en metros entre dos puntos. 

La siguiente consulta usa `ST_DISTANCE` para generar un evento cuando una gasolinera está a menos de 10 km del automóvil.

```SQL
SELECT Cars.Location, Station.Location 
FROM Cars c  
JOIN Station s ON ST_DISTANCE(c.Location, s.Location) < 10 * 1000
```

Para obtener más información, consulte la referencia sobre [ST_DISTANCE](https://docs.microsoft.com/stream-analytics-query/st-distance).

## <a name="st_overlaps"></a>ST_OVERLAPS
La función `ST_OVERLAPS` compara dos polígonos. Si los polígonos se superponen, la función devuelve un 1. La función devuelve 0 si los polígonos no se superponen. 

La siguiente consulta usa `ST_OVERLAPS` para generar un evento cuando un edificio está dentro de una posible zona de inundación.

```SQL
SELECT Building.Polygon, Building.Polygon 
FROM Building b 
JOIN Flooding f ON ST_OVERLAPS(b.Polygon, b.Polygon) 
```

La siguiente consulta de ejemplo genera un evento cuando una tormenta se dirige hacia un automóvil.

```SQL
SELECT Cars.Location, Storm.Course
FROM Cars c, Storm s
JOIN Storm s ON ST_OVERLAPS(c.Location, s.Course)
```

Para obtener más información, consulte la referencia sobre [ST_OVERLAPS](https://docs.microsoft.com/stream-analytics-query/st-overlaps).

## <a name="st_intersects"></a>ST_INTERSECTS
La función `ST_INTERSECTS` compara dos LineString. Si las LineString forman una intersección, la función devuelve 1. La función devuelve 0 si las LineString no forman una intersección.

La siguiente consulta de ejemplo usa `ST_INTERSECTS` para determinar si un camino pavimentado forma una intersección con un camino de tierra.

```SQL 
SELECT  
     ST_INTERSECTS(input.pavedRoad, input.dirtRoad)  
FROM input  
```  

### <a name="input-example"></a>Ejemplo de entrada  
  
|areaCentroDeDatos|areaTormenta|  
|--------------------|---------------|  
|{"type":"LineString", "coordinates": [ [-10.0, 0.0], [0.0, 0.0], [10.0, 0.0] ]}|{"type":"LineString", "coordinates": [ [0.0, 10.0], [0.0, 0.0], [0.0, -10.0] ]}|  
|{"type":"LineString", "coordinates": [ [-10.0, 0.0], [0.0, 0.0], [10.0, 0.0] ]}|{"type":"LineString", "coordinates": [ [-10.0, 10.0], [0.0, 10.0], [10.0, 10.0] ]}|  
  
### <a name="output-example"></a>Ejemplo de salida  

 1  
  
 0  

Para obtener más información, consulte la referencia sobre [ST_INTERSECTS](https://docs.microsoft.com/stream-analytics-query/st-intersects).

## <a name="st_within"></a>ST_WITHIN
La `ST_WITHIN` función determina si un punto o polígono está dentro de un polígono. Si el polígono contiene el punto o polígono, la función devuelve 1. La función devuelve 0 si el punto o polígono no se encuentra dentro del polígono declarado.

La siguiente consulta de ejemplo usa `ST_WITHIN` para determinar si el punto de destino de entrega está dentro del polígono del almacén especificado.

```SQL 
SELECT  
     ST_WITHIN(input.deliveryDestination, input.warehouse)  
FROM input 
```  

### <a name="input-example"></a>Ejemplo de entrada  
  
|destinoDeEntrega|almacén|  
|-------------------------|---------------|  
|{"type":"Point", "coordinates": [76.6, 10.1]}|{"type":"Polygon", "coordinates": [ [0.0, 0.0], [10.0, 0.0], [10.0, 10.0], [0.0, 10.0], [0.0, 0.0] ]}|  
|{"type":"Point", "coordinates": [15.0, 15.0]}|{"type":"Polygon", "coordinates": [ [10.0, 10.0], [20.0, 10.0], [20.0, 20.0], [10.0, 20.0], [10.0, 10.0] ]}|  
  
### <a name="output-example"></a>Ejemplo de salida  

 0  
  
 1  

Para obtener más información, consulte la referencia sobre [ST_WITHIN](https://docs.microsoft.com/stream-analytics-query/st-within).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
