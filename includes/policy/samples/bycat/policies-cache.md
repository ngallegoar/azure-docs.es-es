---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/26/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: b24c33c1d9c0471d06cc92a2af9f49a1b5eb1b7d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82191229"
---
|Nombre |Descripción |Efectos |Versión |GitHub |
|---|---|---|---|---|
|[Solo se deben habilitar las conexiones seguras a Redis Cache](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |Permite auditar la habilitación únicamente de conexiones mediante SSL a Redis Cache. El uso de conexiones seguras garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión |Audit, Deny, Disabled |1.0.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
