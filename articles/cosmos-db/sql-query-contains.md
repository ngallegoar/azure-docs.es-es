---
title: Contains en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL CONTAINS en Azure Cosmos DB devuelve un valor booleano que indica si la primera expresión de cadena contiene la segunda
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: a08fe47122d7e9ddd1c9038bb5f15ebbb0be30fa
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848981"
---
# <a name="contains-azure-cosmos-db"></a>CONTAINS (Azure Cosmos DB)

 Devuelve un valor booleano que indica si la primera expresión de cadena contiene la segunda.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
CONTAINS(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr1*  
   La expresión de cadena en la que se va a buscar.  
  
*str_expr2*  
   La expresión de cadena que se va a buscar.  

*bool_expr* Valor opcional para omitir mayúsculas y minúsculas. Cuando se establece en true, CONTAINS realizará una búsqueda sin distinguir mayúsculas y minúsculas. Si no se especifica, este valor es false.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión condicional.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se comprueba si "abc" contiene "ab" y si "abc" contiene "A".  
  
```sql
SELECT CONTAINS("abc", "ab", false) AS c1, CONTAINS("abc", "A", false) AS c2, CONTAINS("abc", "A", true) AS c3
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[
    {
        "c1": true,
        "c2": false,
        "c3": true
    }
]
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy).

El consumo de RU de Contains aumentará a medida que se incremente la cardinalidad de la propiedad en la función del sistema. En otras palabras, si está comprobando si un valor de propiedad contiene una cadena determinada, el cargo de la consulta RU dependerá del número de valores posibles para esa propiedad.

Por ejemplo, considere dos propiedades: ciudad y país. La cardinalidad de ciudad es 5000 y la de país es 200. Aquí se muestran dos consultas de ejemplo:

```sql
    SELECT * FROM c WHERE CONTAINS(c.town, "Red", false)
```

```sql
    SELECT * FROM c WHERE CONTAINS(c.country, "States", false)
```

La primera consulta probablemente usará más RU que la segunda porque la cardinalidad de ciudad es mayor que la de país.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
