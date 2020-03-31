---
title: LEFT en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema de SQL LEFT en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0eac35a91e4d5158335d6797d49a09f8f6f391e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303756"
---
# <a name="left-azure-cosmos-db"></a>LEFT (Azure Cosmos DB)
 Devuelve la parte izquierda de una cadena con el número especificado de caracteres.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
LEFT(<str_expr>, <num_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es la expresión de cadena de la que se van a extraer caracteres.  
  
*num_expr*  
   Es una expresión numérica que especifica el número de caracteres.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se devuelve la parte izquierda de "abc" para distintos valores de longitud.  
  
```sql
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"l1": "a", "l2": "ab"}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
