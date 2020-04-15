---
title: Uso de SQL dinámico
description: Sugerencias para las soluciones de desarrollo que usan SQL dinámico en el grupo de SQL de Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: a9280bb8153204f86096cf8249ff053bee3f71cc
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633523"
---
# <a name="dynamic-sql-in-synapse-sql-pool"></a>SQL dinámico en el grupo de SQL de Synapse

En este artículo se incluyen sugerencias para las soluciones de desarrollo que usan SQL dinámico en el grupo de SQL.

## <a name="dynamic-sql-example"></a>Ejemplo de SQL dinámico

Al desarrollar código de aplicación para el grupo de SQL, puede que necesite usar SQL dinámico con el fin de proporcionar soluciones flexibles, genéricas y modulares. En este momento, el grupo de SQL no admite tipos de datos de blob.

El hecho de no admitir tipos de datos blob podría limitar el tamaño de las cadenas, ya que los tipos de datos blob incluyen los tipos varchar(max) y nvarchar(max).

Si ha usado estos tipos en el código de aplicación para crear cadenas grandes, es necesario dividir el código en fragmentos y usar en su lugar la instrucción EXEC.

Un ejemplo sencillo:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Si la cadena es corta, puede usar [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) como de costumbre.

> [!NOTE]
> Las instrucciones ejecutadas como SQL dinámico seguirán sujetas a todas las reglas de validación de T-SQL.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).
