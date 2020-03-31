---
title: RAND en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL RAND en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e44878b6d65725f08aeca4eb07088315ae2bb78a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302226"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
 Devuelve un valor numérico generado de forma aleatoria a partir de [0,1).
 
## <a name="syntax"></a>Sintaxis
  
```sql
RAND ()  
```  

## <a name="return-types"></a>Tipos de valores devueltos

  Devuelve una expresión numérica.

## <a name="remarks"></a>Observaciones

  `RAND` es una función no determinista. Las llamadas repetitivas de `RAND` no devuelven los mismos resultados.

## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente, se devuelve un valor numérico generado de forma aleatoria.
  
```sql
SELECT RAND() AS rand 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
