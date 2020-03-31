---
title: Solución de problemas de falta de datos en los registros de actividad | Microsoft Docs
description: Proporciona una solución para el problema de falta de datos en los registros de actividad de Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c335a4d30846f7c1b4dbd6b6aedc4d100a9b43a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74014282"
---
# <a name="troubleshoot-missing-data-in-the-azure-active-directory-activity-logs"></a>Solución de problemas: Faltan datos en los registros de actividad de Azure Active Directory 

## <a name="i-cant-find-audit-logs-for-recent-actions-in-the-azure-portal"></a>No encuentro los registros de auditoría de las últimas acciones en Azure Portal

### <a name="symptoms"></a>Síntomas

Realice algunas acciones en Azure Portal y esperaba ver los registros de auditoría de dichas acciones en la hoja `Activity logs > Audit Logs`, pero no los encuentro.

 ![Notificación](./media/troubleshoot-missing-audit-data/01.png)
 
### <a name="cause"></a>Causa

Las acciones no aparecen inmediatamente en los registro de actividad. En la tabla siguiente se enumeran el tiempo de latencia de los registros de actividad. 

| Informe | &nbsp; | Latencia (P95) | Latencia (P99) |
|--------|--------|---------------|---------------|
| Auditoría de directorio | &nbsp; | 2 minutos | 5 minutos |
| Actividad de inicio de sesión | &nbsp; | 2 minutos | 5 minutos | 

### <a name="resolution"></a>Solución

Espere entre 15 minutos y dos horas para ver si las acciones aparecen en el registro. Si no ve los registros incluso después de dos horas, [cree una incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) y la examinaremos.

## <a name="i-cant-find-recent-user-sign-ins-in-the-azure-active-directory-sign-ins-activity-log"></a>No encuentro inicios de sesión de usuario reciente en el registro de actividad de inicios de sesión de Azure Active Directory

### <a name="symptoms"></a>Síntomas

Hace poco he iniciado sesión en Azure Portal y esperaba ver los registros de inicio de sesión de dichas acciones en la hoja `Activity logs > Sign-ins`, pero no los encuentro.

 ![Notificación](./media/troubleshoot-missing-audit-data/02.png)
 
### <a name="cause"></a>Causa

Las acciones no aparecen inmediatamente en los registro de actividad. En la tabla siguiente se enumeran el tiempo de latencia de los registros de actividad. 

| Informe | &nbsp; | Latencia (P95) | Latencia (P99) |
|--------|--------|---------------|---------------|
| Auditoría de directorio | &nbsp; | 2 minutos | 5 minutos |
| Actividad de inicio de sesión | &nbsp; | 2 minutos | 5 minutos | 

### <a name="resolution"></a>Solución

Espere entre 15 minutos y dos horas para ver si las acciones aparecen en el registro. Si no ve los registros incluso después de dos horas, [cree una incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) y la examinaremos.

## <a name="i-cant-view-more-than-30-days-of-report-data-in-the-azure-portal"></a>No puedo ver más de 30 días de datos de informes en Azure Portal

### <a name="symptoms"></a>Síntomas

No puedo ver más de 30 días de datos de inicio de sesión y auditoría en Azure Portal. ¿Por qué? 

 ![Notificación](./media/troubleshoot-missing-audit-data/03.png)

### <a name="cause"></a>Causa

En función de su licencia, las acciones de Azure Active Directory almacenan los informes de actividad durante el siguiente tiempo:

| Informe           | &nbsp; |  Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| ---              | ----   |  ---           | ---                 | ---                 |
| Auditoría de directorio  | &nbsp; |   7 días     | 30 días             | 30 días             |
| Actividad de inicio de sesión | &nbsp; | No disponible. Puede acceder a sus propios inicios de sesión durante 7 días en la hoja del perfil de usuario individual. | 30 días | 30 días             |

Para más información, consulte [Directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).  

### <a name="resolution"></a>Solución

Tiene dos opciones para conservar los datos durante más de 30 días. Puede usar las [API de generación de informes de Azure AD](concept-reporting-api.md) para recuperar los datos mediante programación y almacenarlos en una base de datos. Como alternativa, puede integrar los registros de auditoría en un sistema SIEM de terceros como Splunk o SumoLogic.

## <a name="next-steps"></a>Pasos siguientes

* [Directivas de retención de informes de Azure Active Directory](reference-reports-data-retention.md).
* [Latencias de informes de Azure Active Directory](reference-reports-latencies.md).
* [Preguntas más frecuentes sobre informes de Azure Active Directory](reports-faq.md).

