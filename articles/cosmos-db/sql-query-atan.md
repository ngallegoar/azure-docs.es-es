---
title: ATAN en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema Arcotangente (ATAN) de SQL en Azure Cosmos DB devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 899c94a939be7825dca82522eab235bde9252896
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78302685"
---
# <a name="atan-azure-cosmos-db"></a>ATAN (Azure Cosmos DB)
 Devuelve el ángulo, en radianes, cuya tangente es la expresión numérica especificada. También se denomina arcotangente.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ATAN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve la `ATAN` del valor especificado.  
  
```sql
SELECT ATAN(-45.01) AS atan  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"atan": -1.5485826962062663}]  
```  
  
## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
