---
title: Protección contra amenazas avanzada
description: Advanced Threat Protection detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/05/2020
tags: azure-synapse
ms.openlocfilehash: 0b231adafabff8414b1cf742e1441e938a3fa212
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848201"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Advanced Threat Protection en Azure SQL Database

Advanced Threat Protection para [Azure SQL Database](sql-database-technical-overview.md) y [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) detecta actividades anómalas que indican intentos inusuales y potencialmente peligrosos de acceder a las bases de datos o de vulnerar su seguridad.

Advanced Threat Protection forma parte de la oferta de [seguridad de datos avanzada](sql-database-advanced-data-security.md) (ADS), que es un paquete unificado para capacidades avanzadas de seguridad de SQL. Puede acceder a Advanced Threat Protection y administrarlo a través del portal central de ADS en SQL.

> [!NOTE]
> Este tema se aplica al servidor de Azure SQL y tanto a la instancia de SQL Database como de Azure Synapse que se crean en el servidor de Azure SQL. Para simplificar, SQL Database se utiliza cuando se hace referencia tanto a SQL Database como a Azure Synapse.

## <a name="what-is-advanced-threat-protection"></a>¿Qué es Advanced Threat Protection?

 Advanced Threat Protection proporciona una nueva capa de seguridad, que permite a los clientes detectar amenazas potenciales y responder a ellas cuando se producen, gracias a las alertas de seguridad sobre actividades anómalas. Los usuarios reciben una alerta sobre actividades sospechosas en las bases de datos, posibles vulnerabilidades y ataques por inyección de código SQL, así como sobre los patrones de acceso y consultas a las bases de datos anómalos. Advanced Threat Protection integra las alertas con [Azure Security Center](https://azure.microsoft.com/services/security-center/), que incluye detalles de actividades sospechosas y acciones recomendadas sobre cómo investigar y mitigar la amenaza. Advanced Threat Protection facilita la solución de posibles amenazas a la base de datos sin necesidad de ser un experto en seguridad ni tener que administrar sistemas de supervisión de seguridad avanzada.

Para obtener una experiencia de investigación, se recomienda habilitar la [auditoría de SQL Database](sql-database-auditing.md), que escribe los eventos de las bases de datos en un registro de auditoría en su cuenta de Azure Storage.  

## <a name="advanced-threat-protection-alerts"></a>Alertas de Advanced Threat Protection

Advanced Threat Protection para Azure SQL Database detecta actividades anómalas que indican intentos inusuales y potencialmente dañinos de acceso o ataque a las bases de datos. Para una lista de las alertas de Azure SQL Database, consulte las [Alertas de SQL Database y SQL Data Warehouse en Azure Security Center](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-sql-db-and-warehouse).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Exploración de las actividades anómalas en la base de datos cuando se detecta un evento sospechoso

Cuando se detecten actividades anómalas en las bases de datos, recibirá una notificación por correo electrónico. El correo electrónico proporciona información sobre el evento de seguridad sospechoso, en la que se incluyen la naturaleza de las actividades anómalas, el nombre de la base de datos, el nombre del servidor, el nombre de la aplicación y la hora del evento. Además, el correo electrónico proporciona información sobre las posibles causas y las medidas recomendadas para investigar y mitigar la amenaza potencial para la base de datos.

![Informes de actividades anómalas](./media/sql-database-threat-detection/anomalous_activity_report.png)

1. Haga clic en el vínculo **View recent SQL alerts** (Ver las alertas recientes de SQL) del correo electrónico para iniciar Azure Portal y mostrar la página de alertas de Azure Security Center, que proporciona información general sobre amenazas activas detectadas en la base de datos de SQL.

   ![Amenazas de actividad](./media/sql-database-threat-detection/active_threats.png)

2. Haga clic en una alerta específica para obtener detalles y acciones adicionales para investigar esta amenaza y solucionar amenazas futuras.

   Por ejemplo, la inyección de código SQL es uno de los problemas de seguridad habituales entre las aplicaciones web en Internet y se usa para atacar aplicaciones controladas por datos. Los atacantes aprovechan las vulnerabilidades de la aplicación para inyectar instrucciones SQL malintencionadas en los campos de entrada de la aplicación, con el fin de infringir la seguridad o modificar datos en la base de datos. Para las alertas de inyección de código SQL, los detalles de la alerta incluyen la instrucción SQL vulnerable que se ha aprovechado.

   ![Alerta específica](./media/sql-database-threat-detection/specific_alert.png)

## <a name="explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal"></a>Exploración de alertas de Advanced Threat Protection para la base de datos en Azure Portal

Advanced Threat Protection integra su alerta con [Azure Security Center](https://azure.microsoft.com/services/security-center/). Los mosaicos dinámicos de Advanced Threat Protection de SQL dentro de las hojas de ADS en SQL y de la base de datos en Azure Portal realizan un seguimiento del estado de las amenazas activas.

Haga clic en la **alerta de Advanced Threat Protection** para iniciar la página de alertas de Azure Security Center y obtener información general de las amenazas de SQL activas detectadas en la base de datos.

   ![Alerta de Advanced Threat Protection](./media/sql-database-threat-detection/threat_detection_alert.png)

   ![Alerta 2 de Advanced Threat Protection](./media/sql-database-threat-detection/threat_detection_alert_atp.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [Advanced Threat Protection en bases de datos únicas y agrupadas](sql-database-threat-detection.md).
- Obtenga más información sobre [Advanced Threat Protection en una instancia administrada](sql-database-managed-instance-threat-detection.md).
- Obtenga más información sobre [Advanced Data Security](sql-database-advanced-data-security.md).
- Más información sobre las [auditorías de Azure SQL Database](sql-database-auditing.md)
- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Para más información sobre los precios, consulte la [página Precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)  
