---
title: ACOS en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL ACOS (arccosice) en Azure Cosmos DB devuelve el ángulo, en radianes, cuyo coseno es la expresión numérica especificada.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 733d6b009f03d61c37170cc506a3b2ec842d7c47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300968"
---
# <a name="acos-azure-cosmos-db"></a>ACOS (Azure Cosmos DB)
 Devuelve el ángulo, en radianes, cuyo coseno es la expresión numérica especificada; también se denomina arcocoseno.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ACOS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve el `ACOS` de -1.  
  
```sql
SELECT ACOS(-1) AS acos 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
