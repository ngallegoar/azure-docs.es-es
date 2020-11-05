---
title: Funciones de comprobación de tipos en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre las funciones de comprobación de tipos del sistema SQL en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 96fea7cb53c89c02583a6dba8434cd33641d6a09
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341508"
---
# <a name="type-checking-functions-azure-cosmos-db"></a>Funciones de comprobación de tipos (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Las funciones de comprobación de tipos permiten comprobar el tipo de una expresión dentro de una consulta SQL. Puede usar las funciones de comprobación de tipos para determinar los tipos de propiedades dentro de los elementos sobre la marcha, cuando son variables o desconocidos. 

## <a name="functions"></a>Functions

Esta es una tabla de las funciones de comprobación de tipos integradas admitidas:

Las siguientes funciones admiten la comprobación de tipos de valores de entrada y devuelven un valor booleano.  

* [IS_ARRAY](sql-query-is-array.md)
* [IS_BOOL](sql-query-is-bool.md)
* [IS_DEFINED](sql-query-is-defined.md)
* [IS_NULL](sql-query-is-null.md)
* [IS_NUMBER](sql-query-is-number.md)
* [IS_OBJECT](sql-query-is-object.md)
* [IS_PRIMITIVE](sql-query-is-primitive.md)
* [IS_STRING](sql-query-is-string.md)

## <a name="next-steps"></a>Pasos siguientes

- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
- [Funciones definidas por el usuario](sql-query-udfs.md)
- [Aggregates](sql-query-aggregates.md) (Agregados)
