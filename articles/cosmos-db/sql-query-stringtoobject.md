---
title: StringToObject en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL StringToObject en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c3e61d1efe20910d84ef4ff583d74982b3ea9f3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78296388"
---
# <a name="stringtoobject-azure-cosmos-db"></a>StringToObject (Azure Cosmos DB)
 Devuelve la expresión traducida a un objeto. Si no se puede traducir la expresión, devuelve undefined.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
StringToObject(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena que se va a analizar como una expresión de objeto JSON. Tenga en cuenta que los valores de cadena anidados deben escribirse entre comillas dobles para ser válidos. Para más información sobre el formato JSON, consulte [json.org](https://json.org/)  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de objeto o undefined.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo se comporta `StringToObject` en tipos diferentes. 
  
 Los siguientes son ejemplos con entradas válidas.

```sql
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

El conjunto de resultados es el siguiente:

```json
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Los siguientes son ejemplos con una entrada no válida.
Aunque son válidas dentro de una consulta, no se analizarán en objetos válidos. Las cadenas dentro de la cadena de objeto deben escaparse "{\\"a\\":\\"str\\"}" o la comilla que la rodea debe ser simple '{"a": "str"}'.

Las comillas simples que rodean a los nombres de propiedad no son válidos en JSON.

```sql
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

El conjunto de resultados es el siguiente:

```json
[{}]
```  

Los nombres de propiedad sin comillas circundantes no son válidos en JSON.

```sql
SELECT 
    StringToObject("{a:[1,2,3]}")
```

El conjunto de resultados es el siguiente:

```json
[{}]
``` 

Los siguientes son ejemplos con una entrada no válida.

 La expresión pasada se analizará como un objeto JSON; estas entradas no se evalúan para el tipo de objeto y, por tanto, se devuelven undefined.

```sql
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 El conjunto de resultados es el siguiente:

```json
[{}]
```

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
