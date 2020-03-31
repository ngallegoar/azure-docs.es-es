---
author: ggailey777
ms.service: cost-management-billing
ms.topic: include
ms.date: 05/09/2019
ms.author: glenga
ms.openlocfilehash: 4643bb7f95e4fd1249d3ab6699c1f835c77f18fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77198350"
---
| Resource | [Plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan) | [Plan Premium](../articles/azure-functions/functions-scale.md#premium-plan) | [Plan de App Service](../articles/azure-functions/functions-scale.md#app-service-plan)<sup>1</sup> |
| --- | --- | --- | --- |
| Escalado horizontal | Basado en eventos | Basado en eventos | [Manual o escalabilidad automática](../articles/app-service/manage-scale-up.md) | 
| Instancias máximas | 200 | 100 | 10-20 |
|[Duración de tiempo de espera](../articles/azure-functions/functions-scale.md#timeout) predeterminada (min) |5 | 30 |30<sup>2</sup> |
|[Duración máxima de tiempo de espera](../articles/azure-functions/functions-scale.md#timeout) (min) |10 | sin enlazar<sup>8</sup> | sin enlazar<sup>3</sup> |
| Número máximo de conexiones salientes (por instancia) | 600 activas (1200 en total) | sin enlazar | sin enlazar |
| Tamaño máximo de la solicitud (MB)<sup>4</sup> | 100 | 100 | 100 |
| Longitud máxima de la cadena de consulta<sup>4</sup> | 4096 | 4096 | 4096 |
| Longitud máxima de URL de solicitud<sup>4</sup> | 8192 | 8192 | 8192 |
| [ACU](../articles/virtual-machines/windows/acu.md) por instancia | 100 | 210-840 | 100-840 |
| Memoria máxima (GB por instancia) | 1.5 | 3,5-14 | 1,75-14 |
| Aplicaciones de funciones por plan |100 |100 |sin enlazar<sup>5</sup> |
| [Planes de App Service](../articles/app-service/overview-hosting-plans.md) | 100 por [región](https://azure.microsoft.com/global-infrastructure/regions/) |100 por grupo de recursos |100 por grupo de recursos |
| Almacenamiento<sup>6</sup> |1 GB |250 GB |50-1000 GB |
| Dominios personalizados por aplicación</a> |500<sup>7</sup> |500 |500 |
| Compatibilidad con dominio [Compatibilidad con SSL](../articles/app-service/configure-ssl-bindings.md) |conexión SNI SSL sin enlazar incluida | conexiones SNI SSL ilimitadas y 1 conexión SSL de IP incluidas |conexiones SNI SSL ilimitadas y 1 conexión SSL de IP incluidas | 

<sup>1</sup> Para conocer los límites específicos de las distintas opciones de plan de App Service, consulte [Límites del plan de App Service](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits).  
<sup>2</sup> De manera predeterminada, el tiempo de espera para el tiempo de ejecución de Functions 1.x en un plan de App Service no está enlazado.  
<sup>3</sup> Requiere que el plan de App Service se establezca en [Always On](../articles/azure-functions/functions-scale.md#always-on). Abonar según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/) estándar.  
<sup>4</sup> Estos límites [se establecen en el host](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config).  
<sup>5</sup> El número real de aplicaciones de funciones que puede hospedar depende de la actividad de las aplicaciones, el tamaño de las instancias de máquina y la utilización de recursos correspondiente.  
<sup>6</sup> El límite de almacenamiento es el tamaño total del almacenamiento temporal entre todas las aplicaciones en el mismo plan de App Service. El plan de consumo usa Azure Files para el almacenamiento temporal.  
<sup>7</sup> Cuando la aplicación de funciones se hospeda en un [plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan), solo se admite solo la opción CNAME. Para aplicaciones de funciones se hospedan en un [plan Premium](../articles/azure-functions/functions-scale.md#premium-plan) o en un [plan de App Service](../articles/azure-functions/functions-scale.md#app-service-plan), puede asignar un dominio personalizado mediante un CNAME o un registro A.  
<sup>8</sup> Garantizado hasta 60 minutos.