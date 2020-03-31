---
title: COT en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL Cotangent (COT) en Azure Cosmos DB devuelve la cotangente trigonométrica del ángulo especificado, en radianes, en la expresión numérica especificada.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25c907644f58ee40ea08e5636d68dc0e84564a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299494"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
 Devuelve la cotangente trigonométrica del ángulo especificado, en radianes, en la expresión numérica especificada.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se calcula el `COT` del ángulo especificado.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"cot": -0.040311998371148884}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
