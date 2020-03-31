---
title: Advanced Data Security
description: Obtenga información acerca de la funcionalidad para detectar y clasificar datos confidenciales, administrar los puntos vulnerables de una bases de datos y detectar actividades anómalas que puedan indicar una amenaza para una base de datos de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
ms.author: memildin
author: memildin
manager: rkarlin
ms.reviewer: vanto
ms.date: 03/31/2019
ms.openlocfilehash: 1f0e6694e596dc60264dfe0789a2f80090e0da3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232596"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Advanced Data Security para Azure SQL Database

Advanced Data Security es un paquete unificado de funcionalidades avanzadas de seguridad de SQL. Incluye una funcionalidad para detectar y clasificar datos confidenciales, buscar y mitigar los potenciales puntos vulnerables de una base de datos y detectar actividades anómalas que puedan indicar una amenaza para dicha base de datos. Proporciona una ubicación única para habilitar y administrar estas funcionalidades.

## <a name="overview"></a>Información general

Advanced Data Security (ADS) proporciona un conjunto de funcionalidades avanzadas de seguridad de SQL, entre las que se incluyen Clasificación y detección de datos, Evaluación de vulnerabilidades y Advanced Threat Protection.

- La [clasificación y detección de datos](sql-database-data-discovery-and-classification.md) proporciona funcionalidades integradas en Azure SQL Database para detectar, clasificar, etiquetar y proteger la información confidencial de las bases de datos. Se puede utilizar para proporcionar visibilidad del estado de clasificación de una base de datos y para realizar un seguimiento del acceso a información confidencial dentro de la base de datos y más allá de sus límites.
- [Evaluación de vulnerabilidades](sql-vulnerability-assessment.md) es un servicio fácil de configurar que puede detectar, realizar un seguimiento y ayudarle a corregir posibles puntos vulnerables de una base de datos. Permite ver el estado de la seguridad e incluye los pasos necesarios para resolver problemas de seguridad y mejorar las defensas de cualquier base de datos.
- [Advanced Threat Protection](sql-database-threat-detection-overview.md) permite detectar actividades anómalas que indica intentos poco habituales y posiblemente dañinos de acceder a sus bases de datos o aprovecharse de ellas. Supervisa constantemente una base de datos para detectar actividades sospechosas y proporciona de forma inmediata alertas de seguridad de posibles puntos vulnerables, ataques por inyección de código SQL y patrones anómalos de acceso a las bases de datos. Las alertas de Advanced Threat Protection proporcionan detalles de la actividad sospechosa y recomiendan acciones para investigar y mitigar la amenaza.

Habilite ADS de SQL una vez para habilitar todas estas características incluidas. Con un solo clic, puede habilitar ADS para todas las bases de datos en el servidor de SQL Database o la instancia administrada. La habilitación o administración de la configuración de ADS requiere la pertenencia a uno de los siguientes roles: [administrador de seguridad SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager), administrador de base de datos SQL o administrador de SQL Server. 

Los precios de ATP se ajustan al nivel estándar de Azure Security Center, donde cada servidor de SQL Database o instancia administrada protegidos se contabiliza como un nodo. Los recursos que acaban de protegerse cumplen los requisitos necesarios para disfrutar de la evaluación gratuita del nivel estándar de Security Center. Para más información, consulte la [página de precios de Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Introducción a ADS

Estos pasos le ayudarán a empezar a usar ADS.

## <a name="1-enable-ads"></a>1. Habilitación de ADS

Para habilitar ADS, vaya a **Advanced Data Security** bajo el encabezado **Seguridad** para el servidor de SQL Database o la instancia administrada. Para habilitar ADS para todas las bases de datos en el servidor de bases de datos o la instancia administrada, haga clic en **Habilitar Advanced Data Security en el servidor**.

> [!NOTE]
> Una cuenta de almacenamiento se crea y se configura automáticamente para almacenar los resultados del examen de **evaluación de vulnerabilidades**. Si ya ha habilitado ADS para otro servidor en el mismo grupo de recursos y la misma región, se usa la cuenta de almacenamiento existente.

![Habilitación de ADS](./media/sql-advanced-protection/enable_ads.png) 

> [!NOTE]
> El costo de ADS se ajusta al precio por nodo del nivel estándar de Azure Security Center, donde un nodo es todo el servidor de SQL Database o instancia administrada. Por consiguiente, se paga una sola vez por proteger todas las bases de datos en el servidor de bases de datos o la instancia administrada con ADS. Puede probar ADS desde el principio con una evaluación gratuita.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Comienzo de la clasificación de datos, el seguimiento de puntos vulnerables y la investigación de alertas de amenazas

Haga clic en la tarjeta **Clasificación y detección de datos** para ver las columnas confidenciales que se recomienda clasificar y para clasificar los datos con etiquetas de confidencialidad persistentes. Haga clic en la tarjeta **Evaluación de vulnerabilidad** tarjeta no solo para ver y administrar informes y exámenes de vulnerabilidad, sino también realizar un seguimiento del estado de la seguridad. Si se han recibido alertas de seguridad, haga clic en la tarjeta **Advanced Threat Protection** para ver los detalles de las alertas y un informe consolidado de todas las alertas de su suscripción de Azure en la página de alertas de seguridad de Azure Security Center.

## <a name="3-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>3. Administración de la configuración de ADS en el servidor de SQL Database o la instancia administrada

Para ver y administrar la configuración de ADS, vaya a **Advanced Data Security** bajo el encabezado **Seguridad** para el servidor de SQL Database o la instancia administrada. En esta página, se puede habilitar o deshabilitar ADS, así como modificar la evaluación de vulnerabilidades y la configuración de Advanced Threat Protection para todo el servidor de SQL Database o la instancia administrada.

![Configuración del servidor](./media/sql-advanced-protection/server_settings.png) 

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. Administración de la configuración de ADS para una base de datos SQL

Para reemplazar la configuración de ADS para una base de datos concreta, seleccione la casilla **Enable Advanced Data Security at the database level** (Habilitar Advanced Data Security en el nivel de base de datos). Use esta opción solo si tiene un requisito concreto para recibir las alertas de Advanced Threat Protection o resultados de la evaluación de vulnerabilidades independientes para la base de datos individual, en lugar o además de las alertas y los resultados que recibe de todas las bases de datos del servidor de bases de datos o la instancia administrada.

Una vez activada la casilla de verificación, puede configurar las opciones pertinentes para esta base de datos.
 
![Configuración de bases de datos y Advanced Threat Protection](./media/sql-advanced-protection/database_threat_detection_settings.png) 

También se puede acceder a la configuración de Advanced Data Security para el servidor de bases de datos o la instancia administrada desde el panel de bases de datos de ADS. Haga clic en **Configuración** en el panel principal de ADS y, después, haga clic en **Ver configuración de Advanced Data Security para el servidor**. 

![Configuración de base de datos](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Pasos siguientes 

- Más información sobre la [clasificación y detección de datos](sql-database-data-discovery-and-classification.md) 
- Más información sobre la [evaluación de vulnerabilidades](sql-vulnerability-assessment.md) 
- Más información sobre [Advanced Threat Protection](sql-database-threat-detection.md)
- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
