---
title: DEGREES en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL DEGREES en Azure Cosmos DB que devuelve el ángulo correspondiente en grados para un ángulo especificado en radianes
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5c7aa5496533a42ad726f9ed8efd8ed7429375fc
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93338907"
---
# <a name="degrees-azure-cosmos-db"></a>DEGREES (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve el ángulo correspondiente en grados de un ángulo especificado en radianes.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
DEGREES (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se devuelven los grados de un ángulo de pi/2 radianes.  
  
```sql
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"degrees": 90}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
