---
title: Consulta del grafo de gemelos
titleSuffix: Azure Digital Twins
description: Vea cómo consultar el grafo gemelo de Azure Digital Twins para información.
author: baanders
ms.author: baanders
ms.date: 11/19/2020
ms.topic: conceptual
ms.service: digital-twins
ms.custom: contperfq2
ms.openlocfilehash: 9aa1156da48ba39672d59858d0640619581329ee
ms.sourcegitcommit: f311f112c9ca711d88a096bed43040fcdad24433
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94981126"
---
# <a name="query-the-azure-digital-twins-twin-graph"></a>Consulta del grafo gemelo de Azure Digital Twins

En este artículo se ofrecen ejemplos de consultas e instrucciones más detalladas sobre el uso del **lenguaje de consultas de Azure Digital Twins** para consultar un [grafo de gemelos](concepts-twins-graph.md) para obtener información. (Para obtener una introducción al lenguaje de consulta y una lista completa de sus características, vea [*Conceptos: lenguaje de consulta*](concepts-query-language.md)).

Este artículo comienza con algunas consultas de ejemplo que muestran la estructura del lenguaje de consulta y las operaciones de consulta habituales para los gemelos digitales. Luego, se describe cómo ejecutar las consultas una vez escritas, mediante [Query API](/rest/api/digital-twins/dataplane/query) de Azure Digital Twins o un [SDK](how-to-use-apis-sdks.md#overview-data-plane-apis).

> [!TIP]
> Si ejecuta las consultas de ejemplo siguientes con la llamada a una API o un SDK, deberá condensar el texto de la consulta en una sola línea.

## <a name="show-all-digital-twins"></a>Mostrar todos los gemelos digitales

Esta es la consulta básica que devolverá una lista de todos los gemelos digitales de la instancia:

```sql
SELECT *
FROM DIGITALTWINS
```

## <a name="query-by-property"></a>Consulta por propiedad

Obtenga instancias de Digital Twins por **propiedades** (incluidos el identificador y los metadatos):

```sql
SELECT  *
FROM DigitalTwins T  
WHERE T.firmwareVersion = '1.1'
AND T.$dtId in ['123', '456']
AND T.Temperature = 70
```

> [!TIP]
> El identificador de un gemelo digital se consulta con el campo de metadatos `$dtId`.

También puede obtener instancias de Digital Twins en función de **si una propiedad determinada está definida**. Esta es una consulta que obtiene instancias de Digital Twins que tienen una propiedad *Location* definida:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_DEFINED(Location)
```

Esto le puede ayudar a obtener instancias de Digital Twins por sus propiedades de *etiqueta*, como se describe en [Incorporación de etiquetas a gemelos digitales](how-to-use-tags.md). Esta es una consulta que obtiene todas las instancias de Digital Twins etiquetadas con *red*:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_DEFINED(tags.red)
```

También puede obtener instancias de Digital Twins en función del **tipo de una propiedad**. Esta es una consulta que obtiene instancias de Digital Twins cuya propiedad *Temperature* es un número:

```sql
SELECT * FROM DIGITALTWINS T WHERE IS_NUMBER(T.Temperature)
```

## <a name="query-by-model"></a>Consulta por modelo

El operador `IS_OF_MODEL` se puede usar para filtrar en función del [**modelo**](concepts-models.md) de la instancia de Digital Twins.

Tiene en cuenta la [herencia](concepts-models.md#model-inheritance) y el [control de versiones](how-to-manage-model.md#update-models) del modelo, y se evalúa como **true** para un gemelo dado si este cumple cualquiera de estas condiciones:

* El gemelo implementa directamente el modelo proporcionado a `IS_OF_MODEL()` y el número de versión del modelo en el gemelo es *mayor o igual que* el número de versión del modelo proporcionado.
* El gemelo implementa un modelo que *amplía* el modelo proporcionado a `IS_OF_MODEL()` y el número de versión del modelo extendido del gemelo es *mayor o igual que* el número de versión del modelo proporcionado.

Por ejemplo, si consulta los gemelos del modelo `dtmi:example:widget;4`, la consulta devolverá todos los gemelos basados en la **versión 4 o superior** del modelo de **widget**, y también los gemelos basados en la versión **4 o superior** de los **modelos que heredan del widget**.

`IS_OF_MODEL` puede aceptar varios parámetros diferentes, y el resto de esta sección se dedica a sus distintas opciones de sobrecarga.

El uso más simple de `IS_OF_MODEL` solo toma un parámetro `twinTypeName`: `IS_OF_MODEL(twinTypeName)`.
A continuación, se muestra un ejemplo de consulta que pasa un valor en este parámetro:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:example:thing;1')
```

Para especificar una colección gemela para buscar cuando hay más de uno (como cuando se usa `JOIN`), agregue el parámetro `twinCollection`: `IS_OF_MODEL(twinCollection, twinTypeName)`.
A continuación, se muestra un ejemplo de consulta que agrega un valor en este parámetro:

```sql
SELECT * FROM DIGITALTWINS DT WHERE IS_OF_MODEL(DT, 'dtmi:example:thing;1')
```

Para realizar una coincidencia exacta, agregue el parámetro `exact`: `IS_OF_MODEL(twinTypeName, exact)`.
A continuación, se muestra un ejemplo de consulta que agrega un valor en este parámetro:

```sql
SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:example:thing;1', exact)
```

También puede pasar los tres argumentos juntos: `IS_OF_MODEL(twinCollection, twinTypeName, exact)`.
A continuación, se muestra un ejemplo de consulta que especifica un valor para los tres parámetros:

```sql
SELECT ROOM FROM DIGITALTWINS DT WHERE IS_OF_MODEL(DT, 'dtmi:example:thing;1', exact)
```

## <a name="query-by-relationship"></a>Consulta por relación

Al realizar consultas basadas en **relaciones** de Digital Twins, el lenguaje de consultas de Azure Digital Twins tiene una sintaxis especial.

Las relaciones se extraen en el ámbito de la consulta en la cláusula `FROM`. Una diferencia importante de los lenguajes de tipo SQL "clásico" es que cada expresión de esta cláusula `FROM` no es una tabla; en su lugar, la cláusula `FROM` expresa un recorrido de relación entre entidades y se escribe con una versión de Azure Digital Twins de `JOIN`.

Recuerde que, con las funcionalidades del [modelo](concepts-models.md) de Azure Digital Twins, las relaciones no existen de forma independiente de los gemelos. Esto significa que la operación `JOIN` del lenguaje de consultas de Azure Digital Twins es un poco diferente de la operación `JOIN` general de SQL, ya que las relaciones aquí no se pueden consultar de forma independiente y deben estar vinculadas a un gemelo.
Para incorporar esta diferencia, se usa la palabra clave `RELATED` en la cláusula `JOIN` para hacer referencia a un conjunto de relaciones de un gemelo.

En la siguiente sección se proporcionan varios ejemplos de lo que se ve a continuación.

> [!TIP]
> Conceptualmente, esta característica imita la funcionalidad centrada en documentos de CosmosDB, donde se puede llevar a cabo `JOIN` en los objetos secundarios dentro de un documento. CosmosDB usa la palabra clave `IN` para indicar que `JOIN` está pensado para recorrer en iteración los elementos de la matriz en el documento de contexto actual.

### <a name="relationship-based-query-examples"></a>Ejemplos de consultas basadas en relaciones

Para obtener un conjunto de resultados que incluya relaciones, use una única instrucción `FROM` seguida de N instrucciones `JOIN`, donde las instrucciones `JOIN` expresan relaciones en el resultado de una instrucción `FROM` o `JOIN` anterior.

A continuación se muestra un ejemplo de consulta basada en relaciones. Este fragmento de código selecciona todos los gemelos digitales con una propiedad *ID* "ABC" y todos los gemelos digitales relacionados con estos gemelos digitales a través de una relación *contains*.

```sql
SELECT T, CT
FROM DIGITALTWINS T
JOIN CT RELATED T.contains
WHERE T.$dtId = 'ABC'
```

> [!NOTE]
> El desarrollador no necesita poner en correlación esta operación `JOIN` con un valor de clave en la cláusula `WHERE` (ni especificar un valor de clave insertado con la definición de `JOIN`). El sistema calcula esta correlación automáticamente, ya que las propias propiedades de la relación identifican la entidad de destino.

### <a name="query-the-properties-of-a-relationship"></a>Consulta de las propiedades de una relación

Del mismo modo que los gemelos digitales tienen propiedades que se describen a través de DTDL, las relaciones también pueden tener propiedades. Puede consultar instancias de Digital Twins **en función de las propiedades de sus relaciones**.
El lenguaje de consultas de Azure Digital Twins permite filtrar y proyectar relaciones mediante la asignación de un alias a la relación dentro de la cláusula `JOIN`.

Como ejemplo, considere una relación *servicedBy* que tiene una propiedad *reportedCondition*. En la consulta siguiente, a esta relación se le asigna el alias "R" para hacer referencia a su propiedad.

```sql
SELECT T, SBT, R
FROM DIGITALTWINS T
JOIN SBT RELATED T.servicedBy R
WHERE T.$dtId = 'ABC'
AND R.reportedCondition = 'clean'
```

En el ejemplo anterior, observe cómo *reportedCondition* es una propiedad de la propia relación *servicedBy* (NO de un gemelo digital que tiene una relación *servicedBy*).

### <a name="query-with-multiple-joins"></a>Consulta con varias combinaciones JOIN

En una sola consulta se admiten hasta cinco elementos `JOIN`. Esto le permite atravesar varios niveles de relaciones a la vez.

Este es un ejemplo de una consulta de varias combinaciones, que obtiene todas las bombillas contenidas en los paneles de luz de las salas 1 y 2.

```sql
SELECT LightBulb
FROM DIGITALTWINS Room
JOIN LightPanel RELATED Room.contains
JOIN LightBulb RELATED LightPanel.contains
WHERE IS_OF_MODEL(LightPanel, 'dtmi:contoso:com:lightpanel;1')
AND IS_OF_MODEL(LightBulb, 'dtmi:contoso:com:lightbulb ;1')
AND Room.$dtId IN ['room1', 'room2']
```

## <a name="count-items"></a>Recuento de elementos

Puede contar el número de elementos de un conjunto de resultados mediante la cláusula `Select COUNT`:

```sql
SELECT COUNT()
FROM DIGITALTWINS
```

Agregue una cláusula `WHERE` para contar el número de elementos que cumplen determinados criterios. Estos son algunos ejemplos de recuento con un filtro aplicado basado en el tipo de modelo gemelo (para obtener más información sobre esta sintaxis, vea [*Consulta por modelo*](#query-by-model) a continuación):

```sql
SELECT COUNT()
FROM DIGITALTWINS
WHERE IS_OF_MODEL('dtmi:sample:Room;1')

SELECT COUNT()
FROM DIGITALTWINS c
WHERE IS_OF_MODEL('dtmi:sample:Room;1') AND c.Capacity > 20
```

También puede usar `COUNT` junto con la cláusula `JOIN`. Esta es una consulta que cuenta todas las bombillas incluidas en los paneles de luz de los salones 1 y 2:

```sql
SELECT COUNT()  
FROM DIGITALTWINS Room  
JOIN LightPanel RELATED Room.contains  
JOIN LightBulb RELATED LightPanel.contains  
WHERE IS_OF_MODEL(LightPanel, 'dtmi:contoso:com:lightpanel;1')  
AND IS_OF_MODEL(LightBulb, 'dtmi:contoso:com:lightbulb ;1')  
AND Room.$dtId IN ['room1', 'room2']
```

## <a name="filter-results-select-top-items"></a>Resultados del filtro: seleccionar elementos principales

Puede seleccionar los diversos elementos "principales" en una consulta mediante la cláusula `Select TOP`.

```sql
SELECT TOP (5)
FROM DIGITALTWINS
WHERE ...
```

## <a name="filter-results-specify-return-set-with-projections"></a>Resultados del filtro: especificación del conjunto de devoluciones con proyecciones

El uso de las proyecciones en la instrucción `SELECT` permite elegir las columnas que devolverá una consulta.

>[!NOTE]
>En este momento, no se admiten las propiedades complejas. Para asegurarse de que las propiedades de proyección son válidas, combine las proyecciones con una comprobación de `IS_PRIMITIVE`.

Este es un ejemplo de una consulta que usa una proyección para devolver gemelos y relaciones. La siguiente consulta proyecta los valores *Consumidor*, *Fábrica* y *Perimetral* de un escenario en el que una *Fábrica* con un identificador de *ABC* está relacionado con el *Consumidor* mediante una relación de *Factory.customer*, y esa relación se presenta como *Perimetral*.

```sql
SELECT Consumer, Factory, Edge
FROM DIGITALTWINS Factory
JOIN Consumer RELATED Factory.customer Edge
WHERE Factory.$dtId = 'ABC'
```

También puede usar la proyección para devolver una propiedad de un gemelo. La siguiente consulta proyecta la propiedad *Nombre* de los *Consumidores* que están relacionados con la *Fábrica* con un identificador de *ABC* mediante una relación de *Factory.customer*.

```sql
SELECT Consumer.name
FROM DIGITALTWINS Factory
JOIN Consumer RELATED Factory.customer Edge
WHERE Factory.$dtId = 'ABC'
AND IS_PRIMITIVE(Consumer.name)
```

También puede usar la proyección para devolver una propiedad de una relación. Al igual que en el ejemplo anterior, la siguiente consulta proyecta la propiedad *Nombre* de los *Consumidores* relacionados con la *Fábrica* con un identificador de *ABC* mediante una relación de *Factory.customer*; pero ahora también devuelve dos propiedades de esa relación, *Prop1* y *prop2*. Para ello, se asigna un nombre a la relación *Perimetral* y se recopilan sus propiedades.  

```sql
SELECT Consumer.name, Edge.prop1, Edge.prop2, Factory.area
FROM DIGITALTWINS Factory
JOIN Consumer RELATED Factory.customer Edge
WHERE Factory.$dtId = 'ABC'
AND IS_PRIMITIVE(Factory.area) AND IS_PRIMITIVE(Consumer.name) AND IS_PRIMITIVE(Edge.prop1) AND IS_PRIMITIVE(Edge.prop2)
```

También puede utilizar alias para simplificar las consultas con proyección.

La siguiente consulta realiza las mismas operaciones que el ejemplo anterior, pero asigna a los nombres de propiedades los alias `consumerName`, `first`, `second` y `factoryArea`.

```sql
SELECT Consumer.name AS consumerName, Edge.prop1 AS first, Edge.prop2 AS second, Factory.area AS factoryArea
FROM DIGITALTWINS Factory
JOIN Consumer RELATED Factory.customer Edge
WHERE Factory.$dtId = 'ABC'
AND IS_PRIMITIVE(Factory.area) AND IS_PRIMITIVE(Consumer.name) AND IS_PRIMITIVE(Edge.prop1) AND IS_PRIMITIVE(Edge.prop2)"
```

Esta es una consulta similar que consulta el mismo conjunto que en el caso anterior, pero solo proyecta la propiedad *Consumer.name* como `consumerName` y proyecta la *Fábrica* completa como gemela.

```sql
SELECT Consumer.name AS consumerName, Factory
FROM DIGITALTWINS Factory
JOIN Consumer RELATED Factory.customer Edge
WHERE Factory.$dtId = 'ABC'
AND IS_PRIMITIVE(Factory.area) AND IS_PRIMITIVE(Consumer.name)
```

## <a name="build-efficient-queries-with-the-in-operator"></a>Creación de consultas eficientes con el operador IN

Puede reducir significativamente el número de consultas que necesita si crea una matriz de gemelos y consulta con el operador `IN`. 

Por ejemplo, considere un escenario en el que *Buildings* contenga a *Floors* y *Floors* contenga a *Rooms*. Una forma de buscar las habitaciones que estén activas dentro de un edificio, es seguir estos pasos.

1. Busque los pisos en el edificio con la relación `contains`.

    ```sql
    SELECT Floor
    FROM DIGITALTWINS Building
    JOIN Floor RELATED Building.contains
    WHERE Building.$dtId = @buildingId
    ```

2. Para buscar las habitaciones, en lugar de tener que considerar los pisos uno por uno y ejecutar una consulta `JOIN` para buscar las habitaciones de cada uno, puede realizar consultas con una colección de los pisos del edificio (con el nombre *Floor* en la consulta siguiente).

    En la aplicación cliente:
    
    ```csharp
    var floors = "['floor1','floor2', ..'floorn']"; 
    ```
    
    En la consulta:
    
    ```sql
    
    SELECT Room
    FROM DIGITALTWINS Floor
    JOIN Room RELATED Floor.contains
    WHERE Floor.$dtId IN ['floor1','floor2', ..'floorn']
    AND Room. Temperature > 72
    AND IS_OF_MODEL(Room, 'dtmi:com:contoso:Room;1')
    
    ```

## <a name="other-compound-query-examples"></a>Otros ejemplos de consultas compuestas

Puede **combinar** cualquiera de los tipos de consulta anteriores mediante operadores de combinación con el fin de incluir más detalles en una sola consulta. A continuación se muestran algunos ejemplos adicionales de consultas compuestas que realizan consultas para más de un tipo de descriptor de instancia de Digital Twins a la vez.

| Descripción | Consultar |
| --- | --- |
| De entre los dispositivos que tiene *Room 123*, se devuelven los dispositivos MxChip que tienen el rol de operador. | `SELECT device`<br>`FROM DigitalTwins space`<br>`JOIN device RELATED space.has`<br>`WHERE space.$dtid = 'Room 123'`<br>`AND device.$metadata.model = 'dtmi:contoso:com:DigitalTwins:MxChip:3'`<br>`AND has.role = 'Operator'` |
| Se obtienen las instancias de Digital Twins que tienen una relación denominada *Contains* con otra instancia que tiene un identificador *id1* | `SELECT Room`<br>`FROM DIGITALTWINS Room`<br>`JOIN Thermostat RELATED Room.Contains`<br>`WHERE Thermostat.$dtId = 'id1'` |
| Se obtienen todas las salas de este modelo de sala contenidos en *floor11* | `SELECT Room`<br>`FROM DIGITALTWINS Floor`<br>`JOIN Room RELATED Floor.Contains`<br>`WHERE Floor.$dtId = 'floor11'`<br>`AND IS_OF_MODEL(Room, 'dtmi:contoso:com:DigitalTwins:Room;1')` |

## <a name="run-queries-with-the-api"></a>Ejecución de consultas con la API

Una vez que haya decidido una cadena de consulta, puede ejecutarla realizando una llamada a [**Query API**](/rest/api/digital-twins/dataplane/query).

Puede llamar a la API directamente, o bien usar uno de los [SDK](how-to-use-apis-sdks.md#overview-data-plane-apis) disponibles para Azure Digital Twins.

En el fragmento de código siguiente se muestra la llamada al [SDK de .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) desde una aplicación cliente:

```csharp
    string adtInstanceEndpoint = "https://<your-instance-hostname>";

    var credential = new DefaultAzureCredential();
    DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceEndpoint), credential);

    // Run a query for all twins   
    string query = "SELECT * FROM DIGITALTWINS";
    AsyncPageable<BasicDigitalTwin> result = client.QueryAsync<BasicDigitalTwin>(query);
```

Esta llamada devuelve los resultados de la consulta en forma de objeto [BasicDigitalTwin](/dotnet/api/azure.digitaltwins.core.basicdigitaltwin?view=azure-dotnet&preserve-view=true).

Las llamadas de consulta admiten la paginación. Este es un ejemplo completo donde se usa `BasicDigitalTwin` como tipo de resultado de la consulta con control de errores y paginación:

```csharp
try
{
    await foreach(BasicDigitalTwin twin in result)
        {
            // You can include your own logic to print the result
            // The logic below prints the twin's ID and contents
            Console.WriteLine($"Twin ID: {twin.Id} \nTwin data");
            IDictionary<string, object> contents = twin.Contents;
            foreach (KeyValuePair<string, object> kvp in contents)
            {
                Console.WriteLine($"{kvp.Key}  {kvp.Value}");
            }
        }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"Error {e.Status}: {e.Message}");
    throw;
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las [API y los SDK de Azure Digital Twins](how-to-use-apis-sdks.md), incluida Query API, que se usa para ejecutar las consultas de este artículo.
