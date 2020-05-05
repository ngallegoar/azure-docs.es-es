---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/26/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 55ec6f97784fc6c8dc34b67000e9b8f5f67f783c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82191348"
---
|Nombre |Descripción |Efectos |Versión |GitHub |
|---|---|---|---|---|
|[Se debe establecer la propiedad ClusterProtectionLevel en EncryptAndSign en los clústeres de Service Fabric](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F617c02be-7f02-4efd-8836-3180d47b6c68) |Service Fabric proporciona tres niveles de protección (None, Sign y EncryptAndSign) para la comunicación de nodo a nodo mediante un certificado de clúster principal. Establezca el nivel de protección para asegurarse de que todos los mensajes de nodo a nodo se cifran y se firman digitalmente. |Audit, Deny, Disabled |1.1.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditClusterProtectionLevel_Audit.json) |
|[Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb54ed75b-3e1a-44ac-a333-05ba39b99ff0) |Permite auditar el uso de la autenticación de clientes solo mediante Azure Active Directory en Service Fabric |Audit, Deny, Disabled |1.1.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Fabric/ServiceFabric_AuditADAuth_Audit.json) |
