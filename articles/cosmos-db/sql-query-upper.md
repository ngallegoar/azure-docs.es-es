---
title: UPPER en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL UPPER en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5129b4fffafb6918f655263cac2f5564635acf36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303977"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
 Devuelve una expresión de cadena después de convertir datos de caracteres en minúsculas a mayúsculas.  

La función del sistema UPPER no emplea el índice. Si planea realizar con frecuencia comparaciones que no distingan mayúsculas de minúsculas, la función del sistema UPPER puede consumir una cantidad significativa de RU. Si este es el caso, en lugar de usar la función del sistema UPPER para normalizar los datos cada vez que realice comparaciones, puede normalizar el uso de mayúsculas y minúsculas durante la inserción. Al hacerlo, una consulta como SELECT * FROM c WHERE UPPER(c.name) = 'BOB' simplemente se convertirá en SELECT * FROM c WHERE c.name = 'BOB'.

## <a name="syntax"></a>Sintaxis
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `UPPER` en una consulta  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"upper": "ABC"}]  
```

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
