---
title: Compatibilidad con el traslado de recursos de Azure SQL entre regiones con Azure Resource Mover.
description: Revise la compatibilidad con el traslado de recursos de Azure SQL entre regiones con Azure Resource Mover.
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 09/07/2020
ms.author: raynew
ms.openlocfilehash: 4925f6ffd2383c21f8ff9b0e3196d44fc15bb657
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89652653"
---
# <a name="support-for-moving-azure-sql-resources-between-azure-regions"></a>Compatibilidad con el traslado de recursos de Azure SQL entre regiones de Azure

En este artículo se resumen la compatibilidad y los requisitos previos para mover recursos de Azure SQL entre regiones de Azure con Azure Resource Mover.

## <a name="requirements"></a>Requisitos

Los requisitos se resumen en la tabla siguiente.

**Característica** | **No admite/no se admite** | **Detalles**
--- | --- | ---
**Hiperescala de Azure SQL Database** | No compatible | No puede mover bases de datos del nivel de servicio Hiperescala de Azure SQL con Resource Mover.
**Redundancia de zona** | Compatible |  Opciones de traslado admitidas:<br/><br/> - Entre regiones que admiten la redundancia de zona.<br/><br/> - Entre regiones que no admiten la redundancia de zona.<br/><br/> - Entre una región que admite la redundancia de zona y una región que no la admite.<br/><br/> - Entre una región que no admite la redundancia de zona y una región que sí la admite. 
**Sincronización de datos** | Base de datos central/de sincronización: No compatible<br/><br/> Miembro de sincronización: Compatible. | Si se mueve un miembro de sincronización, debe configurar la sincronización de datos en la nueva base de datos de destino.
**Replicación geográfica existente** | Compatible | Las réplicas geográficas existentes se reasignan a la nueva réplica principal en la región de destino.<br/><br/> La propagación debe inicializarse después del traslado. [Más información](/azure/sql-database/sql-database-active-geo-replication-portal)
**Cifrado de datos transparente (TDE) con Bring Your Own Key (BYOK)** | Compatible | [Más información](../key-vault/general/move-region.md) sobre cómo trasladar almacenes de claves entre regiones.
**TDE con clave administrada por servicios** | Compatible. |  [Más información](../key-vault/general/move-region.md) sobre cómo trasladar almacenes de claves entre regiones.
**Reglas de enmascaramiento dinámico de datos** | Compatible. | Las reglas se copian automáticamente en la región de destino como parte del traslado. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started-portal).
**Advanced Data Security para Azure SQL Database** | No compatible. | Solución alternativa: configúrela en el nivel de SQL Server de la región de destino. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).
**Reglas de firewall** | No compatible. | Solución alternativa: configure las reglas de firewall para el servidor SQL Server en la región de destino. Las reglas de firewall del nivel de base de datos se copiarán desde el servidor de origen al servidor de destino. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-server-level-firewall-rule).
**Directivas de auditoría** | No compatible. | Las directivas se restablecerán en el valor predeterminado después del traslado. [Aprenda](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) a restablecerlas.
**Retención de copias de seguridad** | Compatible. | Las directivas de retención de copias de seguridad para la base de datos de origen se transfieren a la base de datos de destino. [Aprenda](/azure/sql-database/sql-database-long-term-backup-retention-configure) cómo modificar la configuración después del traslado.
**Ajuste automático** | No compatible. | Solución alternativa: establezca la configuración de ajuste automático después del traslado. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning-enable).
**Alertas de base de datos** | No compatible. | Solución alternativa: Establezca alertas después del traslado. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal).
**Base de datos de extensión de Azure SQL Server** | No compatible | Las bases de datos de extensión de SQL Server no se pueden mover con Resource Mover.
**Azure Synapse Analytics** | No compatible | Synapse Analytics (anteriormente Azure SQL Data Warehouse) no se puede mover con Resource Mover.
## <a name="next-steps"></a>Pasos siguientes

Intente [mover recursos de Azure SQL](tutorial-move-region-sql.md) a otra región con Resource Mover.