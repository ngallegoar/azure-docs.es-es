---
title: 'Azure Defender para SQL: ventajas y características'
description: Obtenga información sobre las ventajas y características de Azure Defender para SQL.
author: memildin
ms.author: memildin
ms.date: 11/30/2020
ms.topic: overview
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: c2fc1bf065bce3ca844c5284168d8ff96fa065bf
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512246"
---
# <a name="introduction-to-azure-defender-for-sql"></a>Introducción a Azure Defender para SQL

Azure Defender para SQL incluye dos planes de Azure Defender que amplían el [paquete de seguridad de datos](../azure-sql/database/azure-defender-for-sql.md) de Azure Security Center para proteger las bases de datos y sus datos dondequiera que se encuentren. 

> [!VIDEO https://www.youtube.com/embed/V7RdB6RSVpc]

## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|**Azure Defender para servidores de base de datos de Azure SQL**: disponible con carácter general (GA)<br>**Azure Defender para servidores SQL Server en máquinas**: disponible con carácter general (GA) |
|Precios:|Los dos planes que forman **Azure Defender para SQL** se facturan como se muestra en [la página de precios](security-center-pricing.md).|
|Versiones de SQL protegidas:|[SQL en máquinas virtuales de Azure](../azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)<br>[Servidores SQL Server habilitados para Azure Arc](https://docs.microsoft.com/sql/sql-server/azure-arc/overview)<br>Servidores SQL Server locales en máquinas Windows sin Azure Arc<br>[Bases de datos únicas](../azure-sql/database/single-database-overview.md) y [grupos elásticos](../azure-sql/database/elastic-pool-overview.md) de Azure SQL<br>[Instancia administrada de Azure SQL](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)<br>[Grupo de SQL dedicado de Azure Synapse Analytics (anteriormente SQL DW)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)|
|Nubes:|![Sí](./media/icons/yes-icon.png) Nubes comerciales<br>![Sí](./media/icons/yes-icon.png) US Gov<br>![No](./media/icons/no-icon.png) China Gov, otros gobiernos|
|||

## <a name="what-does-azure-defender-for-sql-protect"></a>¿Qué protege Azure Defender para SQL?

**Azure Defender para SQL** consta de dos planes de Azure Defender independientes:

- **Azure Defender para servidores de base de datos de Azure SQL** protege:
    - [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md)
    - [Instancia administrada de Azure SQL](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)
    - [Grupo de SQL dedicado en Azure Synapse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

- **Azure Defender para servidores SQL Server en máquinas** amplía las protecciones de los servidores SQL Server nativos de Azure para admitir totalmente los entornos híbridos y proteger los servidores SQL Server (todas las versiones compatibles) hospedados en Azure, otros entornos en la nube e incluso en máquinas locales:
    - [SQL Server en Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/)
    - Servidores SQL Server locales:
        - [SQL Server habilitado para Azure Arc (versión preliminar)](https://docs.microsoft.com/sql/sql-server/azure-arc/overview)
        - [Servidores SQL Server que se ejecutan en máquinas Windows sin Azure Arc](../azure-monitor/platform/agent-windows.md)


## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>¿Cuáles son las ventajas de Azure Defender para SQL?

Estos dos planes incluyen funcionalidad para identificar y mitigar posibles vulnerabilidades de la base de datos, además de detectar actividades anómalas que puedan suponer alguna amenaza para las bases de datos:

- [Evaluación de vulnerabilidades](../azure-sql/database/sql-vulnerability-assessment.md): servicio de exploración que puede detectar, seguir y corregir posibles vulnerabilidades en las bases de datos. Los exámenes de evaluación proporcionan información general sobre el estado de seguridad de las máquinas SQL y detalles de las conclusiones de seguridad.

- [Advanced Threat Protection](../azure-sql/database/threat-detection-overview.md): servicio de detección que supervisa continuamente los servidores SQL Server para detectar amenazas tales como inyección de código SQL, ataques por fuerza bruta y abuso de privilegios. Este servicio proporciona alertas de seguridad orientadas a acciones en Azure Security Center con detalles de la actividad sospechosa, instrucciones sobre cómo mitigar las amenazas y opciones para continuar las investigaciones con Azure Sentinel.


## <a name="what-kind-of-alerts-does-azure-defender-for-sql-provide"></a>¿Qué tipos de alertas ofrece Azure Defender para SQL?

Las alertas de seguridad enriquecidas con inteligencia sobre amenazas se desencadenan en estos casos:

- **Posibles ataques por inyección de código SQL**, incluidas las vulnerabilidades detectadas cuando las aplicaciones generan una instrucción SQL defectuosa en la base de datos.
- **Acceso a bases de datos y patrones de consulta anómalos**: por ejemplo, un número anormalmente alto de intentos de inicio de sesión incorrectos con credenciales diferentes (un intento por fuerza bruta).
- **Actividad sospechosa en las bases de datos**: por ejemplo, un usuario legítimo que accede a un servidor SQL Server desde un equipo vulnerado que se comunica con un servidor de comando y control de minería de datos de cifrado.

Las alertas incluyen detalles acerca del incidente que las desencadenó, así como recomendaciones acerca de cómo investigar y corregir las amenazas.



## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre Azure Defender para SQL.

> [!div class="nextstepaction"]
> [Examen de los servidores SQL Server para detectar vulnerabilidades con Azure Defender](defender-for-sql-usage.md)

Para obtener material relacionado, consulte los siguientes artículos: 

- [Habilitación de Azure Defender para servidores de bases de datos de SQL](../azure-sql/database/azure-defender-for-sql.md)
- [Lista de alertas de seguridad para los servidores SQL Server](alerts-reference.md#alerts-sql-db-and-warehouse)
