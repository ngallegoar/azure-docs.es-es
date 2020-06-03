---
title: Conectar datos de Office 365 a Azure Sentinel| Microsoft Docs
description: Aprenda a conectar datos de Office 365 a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/12/2020
ms.author: yelevin
ms.openlocfilehash: c3e63063b3ea4e7fba3997ddd645aa59fe857488
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758578"
---
# <a name="connect-data-from-office-365-logs"></a>Conectar datos de registros de Office 365



Se pueden transmitir registros de auditoría desde [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) a Azure Sentinel con tan solo un clic. Puede transmitir registros de auditoría desde Office 365 a su área de trabajo de Azure Sentinel en el mismo inquilino. El conector de registro de actividades de Office 365 proporciona información sobre las actividades de usuario en curso. Obtendrá información sobre distintas acciones de los usuarios, los administradores, las directivas o el sistema, así como de eventos de Office 365. Al conectar registros de Office 365 a Azure Sentinel, estos datos se pueden usar para ver paneles, crear alertas personalizadas y mejorar el proceso de investigación.

> [!IMPORTANT]
> Si tiene una licencia de E3, antes de poder acceder a los datos a través de la API de administración de Office 365, debe habilitar el registro de auditoría unificado para la organización de Office 365. Para ello, active el registro de auditoría de Office 365. Para obtener instrucciones, consulte [Activar o desactivar la búsqueda de registros de auditoría de Office 365](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Para más información, consulte [Referencia de la API de Actividad de administración de Office 365](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Prerrequisitos

- Debe ser administrador global o administrador de seguridad en el inquilino.
- El inquilino debe tener habilitada la auditoría unificada. Los inquilinos con licencias de Office 365 E3 o E5 tienen habilitada la auditoría unificada de forma predeterminada. <br>Si el inquilino no tiene ninguna de estas licencias, debe habilitar la auditoría unificada en el inquilino mediante uno de estos métodos:
    - [Usar el cmdlet Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) y habilitar el parámetro “UnifiedAuditLogIngestionEnabled”.
    - [Usar la interfaz de usuario del Centro de seguridad y cumplimiento](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).
   
   > [!NOTE]
   > Actualmente, el conector de datos de O365 solo captura automáticamente Exchange y SharePoint Activity, como se mencionó en la página del conector en la sección Tipos de datos. Le recomendamos que consulte [este artículo en caso de que necesite datos de auditoría de Teams y proteger Teams con Sentinel](https://techcommunity.microsoft.com/t5/azure-sentinel/protecting-your-teams-with-azure-sentinel/ba-p/1265761). 

## <a name="connect-to-office-365"></a>Conectarse a Office 365

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Office 365**.

2. Si aún no lo ha habilitado, puede ir a la hoja **Conectores de datos** y seleccionar el conector de **Office 365**. Aquí puede hacer clic en la página **Open Connector** (Abrir conector) y en la sección de configuración con la etiqueta **Configuración** seleccionar todos los registros de actividad de Office 365 que quiera conectar a Azure Sentinel. 
   > [!NOTE]
   > Si ya ha conectado varios inquilinos en una versión compatible con anterioridad del conector de Office 365 en Azure Sentinel, podrá ver y modificar los registros que recopile de cada inquilino. No podrá agregar inquilinos adicionales, pero puede quitar los inquilinos agregados anteriormente.
3. Para usar el esquema correspondiente en Log Analytics para encontrar registros de Office 365, busque **OfficeActivity**.


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Office 365 a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

