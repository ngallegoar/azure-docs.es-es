---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/29/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 115818ef2a5df04fc07802d882762223f2ddc625
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235707"
---
|Nombre |Descripción |Efectos |Versión |GitHub |
|---|---|---|---|---|
|[Implementar la configuración de diagnóstico de Stream Analytics en el Centro de eventos](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fedf3780c-3d70-40fe-b17e-ab72013dafca) |Implementa la configuración de diagnóstico para que Stream Analytics se transmita a un Centro de eventos regional cuando se cree o actualice cualquier instancia de Stream Analytics a la que falte esta configuración de diagnóstico. |DeployIfNotExists, Disabled |2.0.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/StreamAnalytics_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Implementar la configuración de diagnóstico de Stream Analytics en el área de trabajo de Log Analytics](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F237e0f7e-b0e8-4ec4-ad46-8c12cb66d673) |Implementa la configuración de diagnóstico para que Stream Analytics se transmita a un área de trabajo de Log Analytics regional cuando se cree o actualice cualquier instancia de Stream Analytics a la que falte esta configuración de diagnóstico. |DeployIfNotExists, Disabled |1.0.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/StreamAnalytics_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[Se deben habilitar los registros de diagnóstico en Azure Stream Analytics](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff9be5368-9bf5-4b84-9e0a-7850da98bb46) |Permite auditar la habilitación de registros de diagnóstico. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red. |AuditIfNotExists, Disabled |2.0.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Stream%20Analytics/StreamAnalytics_AuditDiagnosticLog_Audit.json) |
