---
title: Uso de etiquetas para instrumentar consultas
description: Sugerencias para usar etiquetas para instrumentar las consultas para grupos de SQL dedicados en Azure Synapse Analytics
services: synapse-analytics
author: MSTehrani
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: emtehran
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 7958caf71658dcdcbf31bac84697931e9049452f
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96462739"
---
# <a name="using-labels-to-instrument-queries-for-dedicated-sql-pools-in-azure-synapse-analytics"></a>Uso de etiquetas para instrumentar las consultas para grupos de SQL dedicados en Azure Synapse Analytics

En este artículo se incluyen sugerencias para desarrollar soluciones que usan etiquetas para instrumentar consultas en grupos de SQL dedicados.

## <a name="what-are-labels"></a>¿Qué son las etiquetas?

Un grupo de SQL dedicado admite un concepto conocido como "etiquetas de consulta". Antes de entrar en materia, vamos a ver un ejemplo:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Esta última línea etiqueta la cadena "Mi etiqueta de consulta" a la consulta. Esta etiqueta es útil porque la etiqueta se puede consultar a través de las DMV.

La consulta de etiquetas proporciona un mecanismo para buscar consultas problemáticas y ayudar a identificar el progreso a través de una ejecución de ETL.

Una buena convención de nomenclatura realmente ayuda en este caso. Por ejemplo, que la etiqueta empiece por algo como PROYECTO, PROCEDIMIENTO, INSTRUCCIÓN o COMENTARIO identifica de forma única la consulta entre todo el código de control del código fuente.

La consulta siguiente usa una vista de administración dinámica para buscar por etiqueta:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Es esencial que encierre entre corchetes o comillas dobles la etiqueta de la palabra al consultar. La etiqueta es una palabra reservada y provoca un error cuando no se delimita.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).
