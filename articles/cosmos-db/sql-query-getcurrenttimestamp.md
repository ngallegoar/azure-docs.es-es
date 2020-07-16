---
title: GetCurrentTimestamp en lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL GetCurrentTimestamp en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9c35f83ce7a9a478f706e9ed560d884d9bf5e508
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261285"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)

 Devuelve el número de milisegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970.
  
## <a name="syntax"></a>Sintaxis
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve un valor numérico, el número actual de milisegundos que han transcurrido desde la época de Unix, es decir, el número de milisegundos que han transcurrido desde las 00:00:00 del jueves, 1 de enero de 1970.

## <a name="remarks"></a>Observaciones

  GetCurrentTimestamp() es una función no determinista.
  
  El resultado devuelto está en UTC (hora universal coordinada).

## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo obtener la marca de tiempo actual mediante la función integrada GetCurrentTimestamp().
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Este es un conjunto de resultados de ejemplo.
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de fecha y hora (Azure Cosmos DB)](sql-query-date-time-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
