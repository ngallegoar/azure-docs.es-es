---
title: ENDSWITH en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL ENDSWITH en Azure Cosmos DB para devolver un valor booleano que indica si la primera expresión de cadena finaliza con la segunda.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 37c5a8b3c44c5ac46b837e4d851d22f85aeaf39c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299455"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)
 Devuelve un valor booleano que indica si la primera expresión de cadena finaliza con la segunda.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ENDSWITH(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr1*  
   Es una expresión de cadena.  
  
*str_expr2*  
   Es una expresión de cadena que se va a comparar con el final de *str_expr1*.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión condicional.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve las terminaciones "b" y "bc"de "abc".  
  
```sql
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"e1": false, "e2": true}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
