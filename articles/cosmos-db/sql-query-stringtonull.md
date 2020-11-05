---
title: StringToNull en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL StringToNull en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ee91ca3a32bacbf6590877f49bf74499fc00a8f9
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339595"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve la expresión traducida a null. Si no se puede traducir la expresión, devuelve undefined.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Expresión de cadena que se va a analizar como expresión null.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión null o undefined.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo se comporta `StringToNull` en tipos diferentes. 

Los siguientes son ejemplos con entradas válidas.

 Se admiten espacios en blanco únicamente antes o después de "null".

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"n1": null, "n2": null, "n3": true}]
```  

Los siguientes son ejemplos con una entrada no válida.

Null distingue mayúsculas de minúsculas y debe escribirse con caracteres todos en minúsculas; es decir, "null".

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{}]
```  

La expresión pasada se analizará como una expresión null; estas entradas no se evalúan para el tipo null y, por tanto, se devuelven undefined.

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
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
