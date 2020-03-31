---
title: LOG en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL LOG en Azure Cosmos DB devuelve el logaritmo neperiano de la expresión numérica especificada
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9213ef03f383dec7109652246411fac154b4a7f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302515"
---
# <a name="log-azure-cosmos-db"></a>LOG (Azure Cosmos DB)
 Devuelve el algoritmo natural de la expresión numérica especificada.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
LOG (<numeric_expr> [, <base>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
*base*  
   Argumento numérico opcional que establece la base del logaritmo.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="remarks"></a>Observaciones
  
  De forma predeterminada, LOG() devuelve el logaritmo natural. Puede cambiar la base del logaritmo a otro valor mediante el parámetro base opcional.  
  
  El logaritmo natural es el logaritmo en base **e**, donde **e** es una constante irracional aproximadamente igual a 2,718281828.  
  
  El logaritmo natural del valor exponencial de un número es el mismo número: LOG (EXP [n]) = n. Y el valor exponencial del logaritmo natural de un número es el mismo número: EXP (LOG [n]) = n.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor logarítmico de la variable especificada (10).  
  
```sql
SELECT LOG(10) AS log  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{log: 2.3025850929940459}]  
```  
  
 En el ejemplo siguiente se calcula el valor de `LOG` del exponente de un número.  
  
```sql
SELECT EXP(LOG(10)) AS expLog  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{expLog: 10.000000000000002}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
