---
title: Configuración de Advanced Threat Protection
description: Advanced Threat Protection detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad para la base de datos en una sola base de datos o grupo elástico.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-dt-2019
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 08/05/2019
ms.openlocfilehash: 8eb8e4fccc17fe31def671cf6e8edb19d867b244
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822504"
---
# <a name="azure-sql-database-advanced-threat-protection-for-single-or-pooled-databases"></a>Advanced Threat Protection de Azure SQL Database para bases de datos únicas o agrupadas

[Advanced Threat Protection](sql-database-threat-detection-overview.md) para bases de datos únicas o agrupadas detecta actividades anómalas que indican intentos inusuales y potencialmente peligrosos de acceder a las bases de datos o de vulnerar su seguridad. Advanced Threat Protection puede identificar una **posible inyección de código SQL**, **acceso desde un centro de datos o una ubicación inusuales**, **acceso desde una aplicación potencialmente peligrosa o entidad de seguridad desconocida** y **credenciales SQL por fuerza bruta**; consulte más detalles en [Alertas de Advanced Threat Protection](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

Puede recibir notificaciones sobre las amenazas detectadas mediante [notificaciones por correo electrónico](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) o en [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal).

[Advanced Threat Protection](sql-database-threat-detection-overview.md) forma parte de la oferta de [seguridad de datos avanzada](sql-database-advanced-data-security.md) (ADS), que es un paquete unificado para las funcionalidades avanzadas de seguridad de SQL. Puede acceder a Advanced Threat Protection y administrarlo a través del portal central de ADS en SQL.

## <a name="set-up-advanced-threat-protection-in-the-azure-portal"></a>Configuración de Advanced Threat Protection en el Azure Portal

1. Inicie Azure Portal en [https://portal.azure.com](https://portal.azure.com).
2. Vaya a la página de configuración del servidor de Azure SQL Database que quiere proteger. En la configuración de seguridad, seleccione **Advanced Data Security**.
3. En la página de configuración **Advanced Data Security**:

   - Habilite Advanced Data Security en el servidor.
   - En **Configuración de Advanced Threat Protection**, en el cuadro de texto **Enviar alertas a**, proporcione la lista de correos electrónicos para recibir alertas de seguridad cuando se detecten actividades anómalas en la base de datos.
  
   ![Configuración de Advanced Threat Protection](./media/sql-database-threat-detection/set_up_threat_detection.png)

   > [!NOTE]
   > Los precios que aparecen en las capturas de pantalla no siempre reflejan el precio actual y no son más que ejemplos.

## <a name="set-up-advanced-threat-protection-using-powershell"></a>Configuración de Advanced Threat Protection mediante PowerShell

Para ver un script de ejemplo, consulte [Configuración de auditoría y Advanced Threat Protection mediante PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Advanced Threat Protection](sql-database-threat-detection-overview.md).
- Obtenga más información sobre [Advanced Threat Protection en una instancia administrada](sql-database-managed-instance-threat-detection.md).  
- Más información sobre [Advanced Data Security](sql-database-advanced-data-security.md).
- Más información sobre las [auditorías](sql-database-auditing.md)
- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Para más información sobre los precios, consulte la [página Precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)  
