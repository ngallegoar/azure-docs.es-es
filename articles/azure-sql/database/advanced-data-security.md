---
title: Advanced Data Security
description: Obtenga información sobre la funcionalidad para detectar y clasificar datos confidenciales, administrar los puntos vulnerables de la base de datos y detectar actividades anómalas que podrían indicar una amenaza para la base de datos en Azure SQL Database, Instancia administrada de Azure SQL o Azure Synapse.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
author: memildin
manager: rkarlin
ms.reviewer: vanto
ms.date: 04/23/2020
ms.openlocfilehash: d600d174aa37c5c4d5d1011b9cb61e4487256c13
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84195143"
---
# <a name="advanced-data-security"></a>Advanced Data Security
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


Advanced Data Security (ADS) es un paquete unificado con funcionalidades de seguridad avanzadas de SQL. ADS están disponible para Azure SQL Database, Instancia administrada de Azure SQL y Azure Synapse Analytics. Incluye una funcionalidad para detectar y clasificar datos confidenciales, buscar y mitigar los potenciales puntos vulnerables de una base de datos y detectar actividades anómalas que puedan indicar una amenaza para dicha base de datos. Proporciona una ubicación única para habilitar y administrar estas funcionalidades.

## <a name="overview"></a>Información general

ADS proporciona un conjunto de funcionalidades avanzadas de seguridad de SQL, entre las que se incluyen Clasificación y detección de datos, Evaluación de vulnerabilidad SQL y Advanced Threat Protection.
- [Clasificación y detección de datos](data-discovery-and-classification-overview.md) proporciona funcionalidades integradas en Azure SQL Database, Instancia administrada de Azure SQL y Azure Synapse para detectar, clasificar, etiquetar y notificar la información confidencial de las bases de datos. Se puede utilizar para proporcionar visibilidad del estado de clasificación de una base de datos y para realizar un seguimiento del acceso a información confidencial dentro de la base de datos y más allá de sus límites.
- [Evaluación de vulnerabilidad](sql-vulnerability-assessment.md) es un servicio fácil de configurar que puede detectar, realizar el seguimiento y ayudarle a corregir vulnerabilidades de la base de datos. Permite ver el estado de la seguridad e incluye pasos interactivos para resolver problemas de seguridad y mejorar las defensas de la base de datos.
- [Advanced Threat Protection](threat-detection-overview.md) permite detectar actividades anómalas que indica intentos poco habituales y posiblemente dañinos de acceder a sus bases de datos o aprovecharse de ellas. Supervisa de forma continuada la base de datos para detectar actividades sospechosas y proporciona de forma inmediata alertas de seguridad sobre posibles vulnerabilidades, ataques por inyección de código SQL de Azure y patrones anómalos de acceso a la base de datos. Las alertas de Advanced Threat Protection proporcionan detalles de la actividad sospechosa y recomiendan acciones para investigar y mitigar la amenaza.

Habilite Advanced Data Security una vez para habilitar todas estas características incluidas. Con un solo clic, puede habilitar ADS para todas las bases de datos en el [servidor](logical-servers.md) en Azure o la instancia administrada de SQL. La habilitación o administración de la configuración de ADS requiere la pertenencia al rol [administrador de seguridad SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager), o bien a uno de los roles de administrador de base de datos o de servidor.

Los precios de ATP se alinean al nivel estándar de Azure Security Center, donde cada servidor o instancia administrada protegidos se contabiliza como un único nodo. Los recursos que acaban de protegerse cumplen los requisitos necesarios para disfrutar de la evaluación gratuita del nivel estándar de Security Center. Para más información, consulte la [página de precios de Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Introducción a ADS

Estos pasos le ayudarán a empezar a usar ADS.

## <a name="1-enable-ads"></a>1. Habilitación de ADS

Para habilitar ADS, vaya a **Advanced Data Security** bajo el encabezado **Seguridad** para el servidor o la instancia administrada.

> [!NOTE]
> Una cuenta de almacenamiento se crea y se configura automáticamente para almacenar los resultados del examen de **evaluación de vulnerabilidades**. Si ya ha habilitado ADS para otro servidor en el mismo grupo de recursos y la misma región, se usa la cuenta de almacenamiento existente.

![Habilitación de ADS](./media/advanced-data-security/enable_ads.png)

> [!NOTE]
> El costo de ADS se alinea al precio por nodo del nivel estándar de Azure Security Center, donde un nodo es la totalidad del servidor o la instancia administrada. Por tanto, se paga una sola vez por proteger con ADS todas las bases de datos del servidor o la instancia administrada. Puede probar ADS desde el principio con una evaluación gratuita.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Comienzo de la clasificación de datos, el seguimiento de puntos vulnerables y la investigación de alertas de amenazas

Haga clic en la tarjeta **Clasificación y detección de datos** para ver las columnas confidenciales que se recomienda clasificar y para clasificar los datos con etiquetas de confidencialidad persistentes. Haga clic en la tarjeta **Evaluación de vulnerabilidad** tarjeta no solo para ver y administrar informes y exámenes de vulnerabilidad, sino también realizar un seguimiento del estado de la seguridad. Si se han recibido alertas de seguridad, haga clic en la tarjeta **Advanced Threat Protection** para ver los detalles de las alertas y un informe consolidado de todas las alertas de su suscripción de Azure en la página de alertas de seguridad de Azure Security Center.

## <a name="3-manage-ads-settings"></a>3. Administración de la configuración de ADS

Para ver y administrar la configuración de ADS, vaya a **Advanced Data Security** bajo el encabezado **Seguridad** para el servidor o la instancia administrada. En esta página, puede habilitar o deshabilitar ADS, y modificar la evaluación de vulnerabilidades y la configuración de Advanced Threat Protection para la totalidad del servidor o la instancia administrada.

![Configuración del servidor](./media/advanced-data-security/server_settings.png)

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. Administración de la configuración de ADS para una base de datos SQL

Para reemplazar la configuración de ADS para una base de datos concreta, seleccione la casilla **Enable Advanced Data Security at the database level** (Habilitar Advanced Data Security en el nivel de base de datos). Use esta opción solo si tiene un requisito concreto para recibir alertas de Advanced Threat Protection o resultados de la evaluación de vulnerabilidades independientes para la base de datos individual, en lugar o además de las alertas y los resultados que recibe de todas las bases de datos en el servidor o la instancia administrada.

Una vez activada la casilla de verificación, puede configurar las opciones pertinentes para esta base de datos.

![Configuración de bases de datos y Advanced Threat Protection](./media/advanced-data-security/database_threat_detection_settings.png)

También se puede acceder a la configuración de Advanced Data Security para el servidor o la instancia administrada desde el panel de bases de datos de ADS. Haga clic en **Configuración** en el panel principal de ADS y, después, haga clic en **Ver configuración de Advanced Data Security para el servidor**.

![Configuración de base de datos](./media/advanced-data-security/database_settings.png)

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [Clasificación y detección de datos](data-discovery-and-classification-overview.md)
- Más información sobre la [evaluación de vulnerabilidades](sql-vulnerability-assessment.md)
- Más información sobre [Advanced Threat Protection](threat-detection-configure.md)
- Más información acerca de [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
