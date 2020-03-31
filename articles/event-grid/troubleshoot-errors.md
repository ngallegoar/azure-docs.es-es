---
title: 'Azure Event Grid: guía para la solución de problemas'
description: En este artículo se proporciona una lista de códigos de error, mensajes de error, descripciones y acciones recomendadas.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: 1ab9aeac0bde21e229fdb57b7ad02d5d48471551
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645079"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Solución de problemas de Azure Event Grid
En esta guía de solución de problemas se le proporciona una lista de códigos de error de Azure Event Grid, mensajes de error, sus descripciones y acciones recomendadas que debe realizar cuando reciba estos errores. 

## <a name="error-code-400"></a>Código de error: 400
| Código de error | Mensaje de error | Descripción | Recomendación |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.BadRequest<br/>400 | El nombre del tema debe tener entre 3 y 50 caracteres. | La longitud del nombre del dominio debe estar entre 3 y 50 caracteres. Solo se permiten caracteres alfanuméricos, dígitos y el carácter "-" en el nombre del tema. Además, el nombre no debe comenzar con las siguientes palabras reservadas: <ul><li>Microsoft</li><li>EventGrid</li><li>Sistema</li></ul> | Elija otro nombre de tema que cumpla con los requisitos pertinentes. |
| HttpStatusCode.BadRequest<br/>400 | El nombre del dominio debe tener entre 3 y 50 caracteres. | La longitud del nombre del dominio debe estar entre 3 y 50 caracteres. Solo se permiten caracteres alfanuméricos, dígitos y el carácter "-" en el nombre del tema. Además, el nombre no debe comenzar con las siguientes palabras reservadas:<ul><li>Microsoft</li><li>EventGrid</li><li>Sistema</li> | Elija otro nombre de dominio que cumpla con los requisitos pertinentes. |
| HttpStatusCode.BadRequest<br/>400 | Hora de expiración no válida. | La hora de expiración de la suscripción de eventos determina cuándo se retirará la suscripción de eventos. Este valor debe ser un valor DateTime válido en el futuro.| Asegúrese de la hora de expiración de la suscripción de eventos está en un formato DateTime válido y que está configurada en un momento futuro. |

## <a name="error-code-409"></a>Código de error: 409
| Código de error | Mensaje de error | Descripción | Acción recomendada |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.Conflict <br/>409 | Ya existe un tema con el nombre especificado. Elija otro nombre para el tema.   | El nombre del tema personalizado debe ser único en una única región de Azure con el fin de garantizar una operación de publicación correcta. Se puede usar el mismo nombre en distintas regiones de Azure. | Elija otro nombre para el tema. |
| HttpStatusCode.Conflict <br/> 409 | Ya existe un dominio con el nombre especificado. Elija otro nombre de dominio. | El nombre del dominio personalizado debe ser único en una única región de Azure con el fin de garantizar una operación de publicación correcta. Se puede usar el mismo nombre en distintas regiones de Azure. | Elija otro nombre para el dominio. |
| HttpStatusCode.Conflict<br/>409 | Se ha alcanzado el límite de cuota. Para obtener más información sobre estos límites, vea [Límites de Event Grid](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits).  | Cada suscripción de Azure tiene un límite en cuanto al número de recursos de Azure Event Grid que puede usar. Se ha superado parte de esta cuota, o toda ella, y no se pueden crear más recursos. |    Compruebe el uso actual de los recursos y elimine los que no sean necesarios. Si todavía necesita aumentar la cuota, envíe un correo electrónico a [aeg@microsoft.com](mailto:aeg@microsoft.com) con el número exacto de recursos necesarios. |


## <a name="next-steps"></a>Pasos siguientes
Si necesita más ayuda, publique su problema en el [foro de Stack Overflow](https://stackoverflow.com/questions/tagged/azure-eventgrid) o abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/). 
