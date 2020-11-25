---
title: ST_ISVALIDDETAILED en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL ST_ISVALIDDETAILED en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 11/23/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6ccd3178f1126ce8fe8f10b126dc6eadaf72bf53
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "96004422"
---
# <a name="st_isvaliddetailed-azure-cosmos-db"></a>ST_ISVALIDDETAILED (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve un valor JSON que contiene un valor booleano si la expresión de Point, Polygon o LineString de GeoJSON especificada es válida; si no es válida, devuelve también el motivo como un valor de cadena.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   Es cualquier expresión Point o Polygon de GeoJSON.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve un valor JSON que contiene un valor booleano si la expresión de punto o polígono GeoJSON especificada es válida; si no es válida, devuelve también el motivo en forma de valor de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el siguiente ejemplo se muestra cómo comprobar la validez (con detalles) mediante `ST_ISVALIDDETAILED`.  
  
```sql
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones espaciales (Azure Cosmos DB)](sql-query-spatial-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
