---
title: Funciones de fecha y hora en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre las funciones del sistema de fecha y hora de SQL en Azure Cosmos DB para realizar operaciones de fecha y hora y marca de tiempo.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e3666f58b12855c19dd9b8ecf5519ab772c49743
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246944"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Funciones de fecha y hora (Azure Cosmos DB)

Las funciones de fecha y hora permiten realizar operaciones de fecha y hora y marca de tiempo en Azure Cosmos DB.

## <a name="functions-to-obtain-the-date-and-time"></a>Funciones para obtener la fecha y hora

Las funciones escalares siguientes le permiten obtener la fecha y la hora UTC actuales de dos formas: una cadena que se ajusta al formato ISO 8601, o una marca de tiempo numérica cuyo valor es la época de Unix en milisegundos:

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)

## <a name="functions-to-work-with-datetime-values"></a>Funciones para trabajar con valores DateTime

Las siguientes funciones permiten manipular fácilmente valores DateTime:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [DateTimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)

## <a name="next-steps"></a>Pasos siguientes

- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
- [Funciones definidas por el usuario](sql-query-udfs.md)
- [Aggregates](sql-query-aggregates.md) (Agregados)
