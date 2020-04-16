---
title: Uso de procedimientos almacenados
description: Sugerencias para desarrollar soluciones mediante la implementación de procedimientos almacenados en el grupo de SQL de Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/02/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 3ffdf7a66c2562b43fc2ed02bb088ab1095118fb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416158"
---
# <a name="using-stored-procedures-in-synapse-sql-pool"></a>Uso de procedimientos almacenados en el grupo de SQL de Synapse

En este artículo se proporcionan sugerencias para desarrollar soluciones del grupo de SQL mediante la implementación de procedimientos almacenados.

## <a name="what-to-expect"></a>Qué esperar

El grupo de SQL admite muchas de las características de T-SQL que se usan en SQL Server. Más importante aún, hay características específicas de escalabilidad horizontal que puede utilizar para maximizar el rendimiento de la solución.

Además, para ayudarle a mantener la escala y el rendimiento del grupo de SQL, hay características y funcionalidades adicionales que tienen diferencias de comportamiento.

## <a name="introducing-stored-procedures"></a>Introducción a los procedimientos almacenados

Los procedimientos almacenados son una manera excelente para encapsular el código SQL, que se almacena cerca de los datos del grupo de SQL. Los procedimientos almacenados también ayudan a los desarrolladores a encapsular el código en unidades administrables para modularizar sus soluciones, de tal manera que facilitan una mayor reutilización del código. Cada procedimiento almacenado también puede aceptar parámetros para que sean todavía más flexibles.

El grupo de SQL proporciona una implementación optimizada y simplificada de procedimientos almacenados. La diferencia más importante en comparación con SQL Server es que el procedimiento almacenado no es código compilado previamente.

En general, en el almacenamiento de datos, el tiempo de compilación es corto en comparación con el tiempo necesario para ejecutar consultas en grandes volúmenes de datos. Es más importante asegurarse de que el código del procedimiento almacenado esté optimizado correctamente para consultas grandes.

> [!TIP]
> El objetivo es ahorrar horas, minutos y segundos, no milisegundos. Por lo tanto, resulta útil pensar en los procedimientos almacenados como contenedores para la lógica SQL.

Si el grupo de SQL ejecuta el procedimiento almacenado, las instrucciones SQL se analizan, traducen y optimizan en tiempo de ejecución. Durante este proceso, cada instrucción se convierte en consultas distribuidas. El código SQL que se ejecuta en los datos es diferente de la consulta enviada.

## <a name="nesting-stored-procedures"></a>Anidamiento de los procedimientos almacenados

Cuando los procedimientos almacenados llaman a otros procedimientos almacenados o ejecutan SQL dinámico, se dice que la invocación interna de código o de procedimientos almacenados se anida.

El grupo de SQL admite un máximo de ocho niveles de anidamiento. En cambio, el nivel de anidamiento en SQL Server es 32.

La llamada al procedimiento almacenado de nivel superior es igual al nivel de anidamiento 1.

```sql
EXEC prc_nesting
```

Si el procedimiento almacenado también realiza otra llamada EXEC, el nivel de anidamiento aumentará a 2.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```

Si el segundo procedimiento ejecuta luego SQL dinámico, el nivel de anidamiento aumentará a 3.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

El grupo de SQL actualmente no admite [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest). Por tanto, debe realizar un seguimiento del nivel de anidamiento. Es poco probable que supere el límite de ocho niveles de anidamiento. Sin embargo, si lo hace, debe reutilizar el código para ajustarse a los niveles de anidamiento dentro de este límite.

## <a name="insertexecute"></a>INSERT..EXECUTE

El grupo de SQL no permite utilizar el conjunto de resultados de un procedimiento almacenado con una instrucción INSERT. Sin embargo, puede utilizar un método alternativo. Para ver un ejemplo, consulte el artículo sobre [tablas temporales](sql-data-warehouse-tables-temporary.md).

## <a name="limitations"></a>Limitaciones

Existen algunos aspectos de los procedimientos almacenados de Transact-SQL que no se implementan en el grupo de SQL, como los siguientes:

* Procedimientos almacenados temporales
* Procedimientos almacenados numerados
* Procedimientos almacenados extendidos
* Procedimientos almacenados CLR
* Opción de cifrado
* Opción de replicación
* Parámetros con valores de tabla
* Parámetros de solo lectura
* Parámetros predeterminados
* Contextos de ejecución
* Instrucción de devolución

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).
