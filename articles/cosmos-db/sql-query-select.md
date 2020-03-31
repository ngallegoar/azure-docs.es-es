---
title: Cláusula SELECT en Azure Cosmos DB
description: Obtenga información sobre la cláusula SELECT de SQL para Azure Cosmos DB. Use SQL como lenguaje de consulta de JSON para Azure Cosmos DB.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: 013ebdcdbac41825c10a1362f73ab4c94052400d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77469942"
---
# <a name="select-clause-in-azure-cosmos-db"></a>Cláusula SELECT en Azure Cosmos DB

Todas las consultas constan de una cláusula SELECT y cláusulas [FROM](sql-query-from.md) y [WHERE](sql-query-where.md) opcionales, según los estándares SQL ANSI. Normalmente, se enumera el origen en la cláusula FROM y la cláusula WHERE aplica un filtro en el origen para recuperar un subconjunto de elementos JSON. La cláusula SELECT luego proyecta los valores JSON solicitados en la lista seleccionada.

## <a name="syntax"></a>Sintaxis

```sql
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | [DISTINCT] <object_property_list>   
      | [DISTINCT] VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
## <a name="arguments"></a>Argumentos
  
- `<select_specification>`  

  Propiedades o valor que se van a seleccionar para los resultados.  
  
- `'*'`  

  Especifica que se debe recuperar el valor sin cambios. En particular, si el valor procesado es un objeto, se recuperarán todas las propiedades.  
  
- `<object_property_list>`  
  
  Especifica la lista de propiedades que se recuperan. Los valores devueltos serán objetos con las propiedades especificadas.  
  
- `VALUE`  

  Especifica que se debe recuperar el valor JSON en lugar del objeto JSON completo. Esto, a diferencia de `<property_list>`, no encapsula el valor proyectado en un objeto.  
 
- `DISTINCT`
  
  Especifica que se deben quitar los duplicados de las propiedades proyectadas.  

- `<scalar_expression>`  

  Expresión que representa el valor que hay que calcular. Consulte la sección [Expresiones escalares](sql-query-scalar-expressions.md) para más información.  

## <a name="remarks"></a>Observaciones

La sintaxis `SELECT *` solo es válida si la cláusula FROM ha declarado exactamente un alias. `SELECT *` proporciona una proyección de identidad, que puede resultar útil si no se necesitan proyecciones. SELECT * solo es válida si se especifica cláusula FROM y se introdujo solo un origen de entrada.  
  
`SELECT <select_list>` y `SELECT *` son código sintáctico y pueden expresarse también mediante instrucciones SELECT sencillas, como se muestra a continuación.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   equivale a:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   equivale a:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Ejemplos

El siguiente ejemplo de consulta SELECT devuelve `address` de `Families` cuyo `id` coincide con `AndersenFamily`:

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="quoted-property-accessor"></a>Descriptor de acceso de propiedad entre comillas
Puede acceder a las propiedades mediante el operador de la propiedad entre comillas []. Por ejemplo, `SELECT c.grade` and `SELECT c["grade"]` son equivalentes. Esta sintaxis es útil para crear una secuencia de escape para una propiedad que contiene espacios en blanco, caracteres especiales o que tiene el mismo nombre que una palabra clave SQL o una palabra reservada.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

### <a name="nested-properties"></a>Propiedades anidadas

En el ejemplo siguiente, se proyectan dos propiedades anidadas, `f.address.state` y `f.address.city`.

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "state": "WA",
      "city": "Seattle"
    }]
```
### <a name="json-expressions"></a>Expresiones JSON

La proyección también admite expresiones JSON, como se muestra en el siguiente ejemplo:

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle",
        "name": "AndersenFamily"
      }
    }]
```

En el ejemplo anterior, la cláusula SELECT tiene que crear un objeto JSON y, puesto que el ejemplo no proporciona ninguna clave, la cláusula usa el nombre de variable del argumento implícito `$1`. La consulta siguiente devuelve dos variables de argumentos implícitos: `$1` y `$2`.

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Los resultados son:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```
## <a name="reserved-keywords-and-special-characters"></a>Palabras clave reservadas y caracteres especiales

Si los datos contienen propiedades con los mismos nombres como palabras clave reservadas, como, por ejemplo, "pedido" o "Grupo", las consultas realizadas en estos documentos producirán error de sintaxis. Debe incluir explícitamente la propiedad en el carácter `[]` para ejecutar la consulta correctamente.

Por ejemplo, a continuación se muestra un documento con una propiedad denominada `order` y una propiedad `price($)` que contiene caracteres especiales:

```json
{
  "id": "AndersenFamily",
  "order": [
     {
         "orderId": "12345",
         "productId": "A17849",
         "price($)": 59.33
     }
  ],
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Si ejecuta una consulta que incluya la propiedad `order` o `price($)`, recibirá un error de sintaxis.

```sql
SELECT * FROM c where c.order.orderid = "12345"
```
```sql
SELECT * FROM c where c.order.price($) > 50
```
El resultado es el siguiente:

`
Syntax error, incorrect syntax near 'order'
`

Debe volver a escribir las mismas consultas como se indica a continuación:

```sql
SELECT * FROM c WHERE c["order"].orderId = "12345"
```

```sql
SELECT * FROM c WHERE c["order"]["price($)"] > 50
```

## <a name="next-steps"></a>Pasos siguientes

- [Introducción](sql-query-getting-started.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Cláusula WHERE](sql-query-where.md)
