---
title: PI en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL PI en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 27832008e8922e339a648985192a58b111555bc9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "71349652"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
 Devuelve el valor constante de PI.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
PI ()  
```  
   
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  El siguiente ejemplo devuelve el valor de `PI`.  
  
```sql
SELECT PI() AS pi 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
