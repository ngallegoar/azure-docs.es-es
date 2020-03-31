---
title: Migración de DTU a núcleo virtual
description: Migración desde el modelo de DTU al modelo de núcleos virtuales. La migración a un núcleo virtual es similar al proceso de actualización o degradación entre los niveles estándar y premium.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 03/09/2020
ms.openlocfilehash: 693065046f92e0e9eade14c43e9942772440937d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945397"
---
# <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>Migrar desde el modelo basado en DTU al modelo basado en núcleos virtuales

## <a name="migrate-a-database"></a>Migración de bases de datos

La migración de una base de datos del modelo de compra basado en DTU al modelo de compra basado en núcleo virtual es similar a la actualización o degradación entre los niveles de servicio Estándar y Premium en el modelo de compra basado en DTU.

## <a name="migrate-databases-that-use-geo-replication"></a>Migración de bases de datos que usan la replicación geográfica

La migración desde el modelo basado en DTU al modelo basado en núcleos virtuales es similar a la actualización o degradación de las relaciones de replicación geográfica entre las bases de datos en los niveles de servicio Estándar y Premium. Durante la migración, no tendrá que detener la replicación geográfica, pero tiene que seguir estas reglas de secuenciación:

- Al actualizar, debe actualizar primero la base de datos secundaria y luego la principal.
- Al degradar, invierta el orden; es decir, debe degradar primero la base de datos principal y luego la secundaria.

Cuando se usa la replicación geográfica entre dos grupos elásticos, se recomienda designar un grupo como principal y otro como secundario. En ese caso, cuando migre grupos elásticos debe usar la misma guía de secuenciación. Sin embargo, si tiene grupos elásticos que contienen bases de datos principales y secundarias, trate al grupo con la utilización más alta como principal y siga las reglas de secuenciación como corresponda.  

En la tabla siguiente se proporciona una guía para escenarios de migración específicos:

|Nivel de servicio actual|Nivel de servicio de destino|Tipo de migración|Acciones del usuario|
|---|---|---|---|
|Estándar|Uso general|Lateral|Puede migrar en cualquier orden, pero debe asegurarse de tener un tamaño adecuado de núcleo virtual*|
|Premium|Crítico para la empresa|Lateral|Puede migrar en cualquier orden, pero debe asegurarse de tener un tamaño adecuado de núcleo virtual*|
|Estándar|Crítico para la empresa|Actualizar|Debe migrar primero la secundaria|
|Crítico para la empresa|Estándar|Degradar|Debe migrar primero la principal|
|Premium|Uso general|Degradar|Debe migrar primero la principal|
|Uso general|Premium|Actualizar|Debe migrar primero la secundaria|
|Crítico para la empresa|Uso general|Degradar|Debe migrar primero la principal|
|Uso general|Crítico para la empresa|Actualizar|Debe migrar primero la secundaria|
||||

\* Como regla de oro, cada 100 DTU en el nivel estándar requieren como mínimo 1 núcleo virtual, y cada 125 DTU en el nivel premium requieren como mínimo 1 núcleo virtual. Para más información, consulte la sección [Modelo de compra basado en núcleos virtuales](https://docs.microsoft.com/azure/sql-database/sql-database-purchase-models#vcore-based-purchasing-model).

## <a name="migrate-failover-groups"></a>Migración de grupos de conmutación por error

La migración de grupos de conmutación por error con varias bases de datos requiere la migración individual de las bases de datos principales y secundarias. Durante ese proceso, se aplican las mismas consideraciones y reglas de secuenciación. Después de que las bases de datos se convierten al modelo de compra basado en núcleo virtual, el grupo de conmutación por error permanecerá en vigor con la misma configuración de directiva.

### <a name="create-a-geo-replication-secondary-database"></a>Creación de una base de datos secundaria de replicación geográfica

Puede crear una base de datos secundaria de replicación geográfica (geo-secundaria) solo con el mismo nivel de servicio que utilizó para la base de datos principal. Para las bases de datos con una tasa alta de generación de registro, se recomienda crear la replicación geográfica secundaria con el mismo tamaño de proceso que la principal.

Si va a crear una base de datos secundaria de replicación geográfica en el grupo elástico para una única base de datos principal, asegúrese de que el valor `maxVCore` del grupo coincida con el tamaño de proceso de la base de datos principal. Si va a crear una base de datos secundaria de replicación geográfica para una principal en otro grupo elástico, se recomienda que los grupos tengan la misma configuración de `maxVCore`.

## <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Uso de una copia de base de datos para convertir una base de datos basada en DTU en una base de datos basada en núcleos virtuales

Puede copiar cualquier base de datos con un tamaño de proceso basado en DTU en una base de datos que tenga un tamaño de proceso basado en núcleos virtuales sin restricciones o secuencias especiales, siempre y cuando el tamaño de proceso de destino admita el tamaño máximo de base de datos de la base de datos de origen. La copia de base de datos crea una instantánea de los datos a partir de la hora de inicio de la operación de copia y no sincroniza los datos entre el origen y el destino.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las opciones de tamaños de proceso y de almacenamiento específicas que hay disponibles para las bases de datos únicas, consulte [Límites de recursos basados en núcleos virtuales de SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md).
- Para más información sobre las opciones de tamaño de proceso y de almacenamiento específicas que hay disponibles para los grupos elásticos, consulte [Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).
