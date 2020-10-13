---
title: Uso de etiquetas de consulta en SQL de Synapse
description: En este artículo se incluyen sugerencias esenciales para el uso de etiquetas de consulta en SQL de Synapse.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: e8f78d6031e57da42e1d69587aedca0763c9fec2
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91289079"
---
# <a name="use-query-labels-in-synapse-sql"></a>Uso de etiquetas de consulta en SQL de Synapse
En este artículo se incluyen sugerencias esenciales para el uso de etiquetas de consulta en SQL de Synapse.

> [!NOTE]
> SQL a petición (versión preliminar) no admite el etiquetado de consultas.

## <a name="what-are-query-labels"></a>¿Qué son las etiquetas de consulta?
El grupo de SQL admite un concepto conocido como etiquetas de consulta. Antes de entrar en materia, vamos a ver un ejemplo:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Esta última línea etiqueta la cadena "Mi etiqueta de consulta" a la consulta. Esta etiqueta es útil porque se puede consultar a través de las DMV. La consulta de etiquetas proporciona un mecanismo para buscar consultas problemáticas y ayudar a identificar el progreso a través de una ejecución de ETL.

Una buena convención de nomenclatura es muy útil. Por ejemplo, que la etiqueta empiece por algo como PROYECTO, PROCEDIMIENTO, INSTRUCCIÓN o COMENTARIO identifica de forma única la consulta entre todo el código de control del código fuente.

La consulta siguiente usa una vista de administración dinámica para buscar por etiqueta:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Es esencial que encierre entre corchetes o comillas dobles la etiqueta de la palabra al consultar. La etiqueta es una palabra reservada y provoca un error cuando no se delimita. 
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](develop-overview.md).


