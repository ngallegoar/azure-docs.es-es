---
title: LOWER en el lenguaje de consulta de Azure Cosmos DB
description: Más información sobre la función del sistema SQL LOWER en Azure Cosmos DB para devolver una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: dd1d9ddefd67cadb92632fd6db7a1fbd5a34f35a
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93338465"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve una expresión de cadena después de convertir los datos de caracteres en mayúsculas a minúsculas.  

La función del sistema LOWER no emplea el índice. Si planea realizar con frecuencia comparaciones que no distingan mayúsculas de minúsculas, la función del sistema LOWER puede consumir una cantidad significativa de RU. Si este es el caso, en lugar de usar la función del sistema LOWER para normalizar los datos cada vez que realice comparaciones, puede normalizar el uso de mayúsculas y minúsculas durante la inserción. Al hacerlo, una consulta como SELECT * FROM c WHERE LOWER(c.name) = 'bob' simply se convertirá en SELECT * FROM c WHERE c.name = 'bob'.

## <a name="syntax"></a>Sintaxis
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `LOWER` en una consulta.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
