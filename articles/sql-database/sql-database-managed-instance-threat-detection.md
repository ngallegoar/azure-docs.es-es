---
title: 'Configuración de Advanced Threat Protection: Instancia administrada'
description: Advanced Threat Protection detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad para la base de datos en una instancia administrada.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 69292a934af8b8777f11ab58ed3fe306abf8b408
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822562"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-database-managed-instance"></a>Configuración de Advanced Threat Protection en Instancia administrada de Azure SQL Database

[Advanced Threat Protection](sql-database-threat-detection-overview.md) para una [instancia administrada](sql-database-managed-instance-index.yml) detecta actividades anómalas que indican intentos inusuales y potencialmente peligrosos de acceder a las bases de datos o de vulnerar su seguridad. Advanced Threat Protection puede identificar una **posible inyección de código SQL**, **acceso desde un centro de datos o una ubicación inusuales**, **acceso desde una aplicación potencialmente peligrosa o entidad de seguridad desconocida** y **credenciales SQL por fuerza bruta**; consulte más detalles en [Alertas de Advanced Threat Protection](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Puede recibir notificaciones sobre las amenazas detectadas mediante [notificaciones por correo electrónico](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) o en [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal).

[Advanced Threat Protection](sql-database-threat-detection-overview.md) forma parte de la oferta de [seguridad de datos avanzada](sql-database-advanced-data-security.md) (ADS), que es un paquete unificado para las funcionalidades avanzadas de seguridad de SQL. Puede acceder a Advanced Threat Protection y administrarlo a través del portal central de ADS en SQL.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Configuración de Advanced Threat Protection en el Azure Portal

1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com).
2. Vaya a la página de configuración de la instancia administrada que desea proteger. En la página **Configuración**, seleccione **Advanced Data Security**.
3. En la página de configuración de Advanced Data Security
   - **Active** Advanced Data Security.
   - Configure la **lista de direcciones de correo electrónico** que recibirán alertas de seguridad cuando se detecten actividades anómalas en las bases de datos.
   - Seleccione la **cuenta de Azure Storage** donde se guardan los registros de anomalías de la auditoría de amenazas.
   - Seleccione los **tipos de Advanced Threat Protection** que quiere configurar. Más información sobre las [alertas de Advanced Threat Protection](sql-database-threat-detection-overview.md).
4. Haga clic en **Guardar** para guardar o actualizar la directiva de Advanced Data Security.

   ![Protección contra amenazas avanzada](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

   > [!NOTE]
   > Los precios que aparecen en las capturas de pantalla no siempre reflejan el precio actual y no son más que ejemplos.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Advanced Threat Protection](sql-database-threat-detection-overview.md).
- Para más información acerca de las instancias administradas, consulte [¿Qué es una instancia administrada?](sql-database-managed-instance.md)
- Más información sobre [Advanced Threat Protection para una base de datos única](sql-database-threat-detection.md).
- Más información sobre la [auditoría de instancias administradas](https://go.microsoft.com/fwlink/?linkid=869430).
- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
