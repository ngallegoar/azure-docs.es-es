---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/07/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 8081624a533925b5a9c513b540c4296ed16a346e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91859273"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure SignalR Service debe usar vínculos privados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F53503636-bcc9-4748-9663-5348217f160f) |Audite los recursos de Azure SignalR Service que no tengan al menos una conexión de punto de conexión privada aprobada. Los clientes de una red virtual pueden acceder de forma segura a los recursos que tengan conexiones de punto de conexión privadas mediante vínculos privados. Para más información, visite [https://aka.ms/asrs/privatelink](https://aka.ms/asrs/privatelink). |Audit, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SignalR/SignalR_PrivateEndpointEnabled_Audit.json) |
