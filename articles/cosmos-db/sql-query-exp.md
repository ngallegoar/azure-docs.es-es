---
title: EXP en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL Exponent (EXP) en Azure Cosmos DB que devuelve el valor exponencial de la expresión numérica especificada
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: adf80d36edbe8f9a5535e8fa04501918a62ea7ca
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93335660"
---
# <a name="exp-azure-cosmos-db"></a>EXP (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve el valor exponencial de la expresión numérica especificada.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
EXP (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="remarks"></a>Observaciones
  
  La constante **e** (2.718281…) es la base de los logaritmos naturales.  
  
  El exponente de un número es la constante **e** elevada a la potencia del número. Por ejemplo EXP(1,0) = e^1,0 = 2,71828182845905 y EXP(10) = e^10 = 22026,4657948067.  
  
  El valor exponencial del logaritmo natural de un número es el mismo número: EXP (LOG [n]) = n. Y el logaritmo natural del valor exponencial de un número es el mismo número: LOG (EXP [n]) = n.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se declara una variable y se devuelve el valor exponencial de la variable especificada (10).  
  
```sql
SELECT EXP(10) AS exp  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{exp: 22026.465794806718}]  
```  
  
 En el ejemplo siguiente se devuelve el valor exponencial del logaritmo natural de 20 y el logaritmo natural del valor exponencial de 20. Dado que estas funciones son inversas entre sí, el valor devuelto con redondeo para las operaciones matemáticas de punto flotante en ambos casos es 20.  
  
```sql
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{exp1: 19.999999999999996, exp2: 20}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
