---
title: REPLICATE en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL REPLICATE en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 19fcde522c5cb0355e53a5616145f27fada7dad9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302192"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
 Repite un valor de cadena un número especificado de veces.
  
## <a name="syntax"></a>Sintaxis
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.
  
*num_expr*  
   Es una expresión numérica. Si el valor *num_expr* es negativo o no finito, el resultado es indefinido.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.
  
## <a name="remarks"></a>Observaciones
  La longitud máxima del resultado es de 10 000 caracteres, es decir, (length(*str_expr*)  *  *num_expr*) <= 10 000.

## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `REPLICATE` en una consulta.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 El conjunto de resultados es el siguiente:
  
```json
[{"replicate": "aaa"}]
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
