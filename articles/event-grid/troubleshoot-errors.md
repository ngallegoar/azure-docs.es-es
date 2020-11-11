---
title: 'Azure Event Grid: guía para la solución de problemas'
description: En este artículo se proporciona una lista de códigos de error, mensajes de error, descripciones y acciones recomendadas.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 1dd464339e7654f8886224ff07cf368b4724ff82
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93041387"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Solución de problemas de Azure Event Grid
En esta guía de solución de problemas se le proporciona una lista de códigos de error de Azure Event Grid, mensajes de error, sus descripciones y acciones recomendadas que debe realizar cuando reciba estos errores. 

## <a name="error-code-400"></a>Código de error: 400
| Código de error | Mensaje de error | Descripción | Recomendación |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.BadRequest<br/>400 | El nombre del tema debe tener entre 3 y 50 caracteres. | La longitud del nombre del dominio debe estar entre 3 y 50 caracteres. Solo se permiten caracteres alfanuméricos, dígitos y el carácter "-" en el nombre del tema. Además, el nombre no debe comenzar con las siguientes palabras reservadas: <ul><li>Microsoft-</li><li>EventGrid-</li><li>System-</li></ul> | Elija otro nombre de tema que cumpla con los requisitos pertinentes. |
| HttpStatusCode.BadRequest<br/>400 | El nombre del dominio debe tener entre 3 y 50 caracteres. | La longitud del nombre del dominio debe estar entre 3 y 50 caracteres. Solo se permiten caracteres alfanuméricos, dígitos y el carácter "-" en el nombre de dominio. Además, el nombre no debe comenzar con las siguientes palabras reservadas:<ul><li>Microsoft-</li><li>EventGrid-</li><li>System-</li> | Elija otro nombre de dominio que cumpla con los requisitos pertinentes. |
| HttpStatusCode.BadRequest<br/>400 | Hora de expiración no válida. | La hora de expiración de la suscripción de eventos determina cuándo se retirará la suscripción de eventos. Este valor debe ser un valor DateTime válido en el futuro.| Asegúrese de la hora de expiración de la suscripción de eventos está en un formato DateTime válido y que está configurada en un momento futuro. |

## <a name="error-code-409"></a>Código de error: 409
| Código de error | Mensaje de error | Descripción | Acción recomendada |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.Conflict <br/>409 | Ya existe un tema con el nombre especificado. Elija otro nombre para el tema.   | El nombre del tema personalizado debe ser único en una única región de Azure con el fin de garantizar una operación de publicación correcta. Se puede usar el mismo nombre en distintas regiones de Azure. | Elija otro nombre para el tema. |
| HttpStatusCode.Conflict <br/> 409 | Ya existe un dominio con el nombre especificado. Elija otro nombre de dominio. | El nombre del dominio personalizado debe ser único en una única región de Azure con el fin de garantizar una operación de publicación correcta. Se puede usar el mismo nombre en distintas regiones de Azure. | Elija otro nombre para el dominio. |
| HttpStatusCode.Conflict<br/>409 | Se ha alcanzado el límite de cuota. Para obtener más información sobre estos límites, vea [Límites de Event Grid](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits).  | Cada suscripción de Azure tiene un límite en cuanto al número de recursos de Azure Event Grid que puede usar. Se ha superado parte de esta cuota, o toda ella, y no se pueden crear más recursos. |    Compruebe el uso actual de los recursos y elimine los que no sean necesarios. Si todavía necesita aumentar la cuota, envíe un correo electrónico a [aeg@microsoft.com](mailto:aeg@microsoft.com) con el número exacto de recursos necesarios. |

## <a name="error-code-403"></a>Código de error: 403

| Código de error | Mensaje de error | Descripción | Acción recomendada |
| ---------- | ------------- | ----------- | ------------------ |
| HttpStatusCode.Forbidden <br/>403 | La publicación en {Topic/Domain} por el cliente {IpAddress} se rechaza debido a las reglas de filtrado de IpAddress. | El tema o el dominio tienen reglas de firewall de IP configuradas y el acceso solo está restringido a las direcciones IP configuradas. | Para agregar la dirección IP a las reglas de firewall de IP, consulte [Configuración de la dirección IP del firewall ](configure-firewall.md) |
| HttpStatusCode.Forbidden <br/> 403 | La publicación en {Topic/Domain} por cliente se rechaza ya que la solicitud proviene de un punto de conexión privado y no se encontró ninguna conexión de punto de conexión coincidente para el recurso. | El tema o el dominio tienen puntos de conexión privados configurados y la solicitud de publicación procedía de uno que no está configurado o aprobado. | Configure un punto de conexión privado para el tema o dominio. [Configuración de puntos de conexión privados](configure-private-endpoints.md) |

## <a name="troubleshoot-event-subscription-validation"></a>Solución de problemas de validación de suscripciones a eventos

Durante la creación de suscripciones a eventos, si ve un mensaje de error como `The attempt to validate the provided endpoint https://your-endpoint-here failed. For more details, visit https://aka.ms/esvalidation`, indica que hay un error en el protocolo de enlace de la validación. Para resolver este error, compruebe los siguientes aspectos:

- Realice una solicitud HTTP POST a la dirección URL de webhook con un cuerpo de la solicitud [SubscriptionValidationEvent de ejemplo](webhook-event-delivery.md#validation-details) con Postman, curl o una herramienta similar.
- Si el webhook está implementando un mecanismo de enlace de validación sincrónica, compruebe que ValidationCode se devuelve como parte de la respuesta.
- Si el webhook está implementando un mecanismo de enlace de validación asincrónica, compruebe que la solicitud HTTP POST devuelve 200 OK.
- Si el webhook devuelve 403 (Prohibido) en la respuesta, compruebe si el webhook está detrás de una puerta de enlace de aplicaciones de Azure o del firewall de aplicaciones web. Si es así, tendrá que deshabilitar estas reglas de firewall y volver a realizar una solicitud HTTP POST:

  920300 [Falta un encabezado de aceptación (Accept) en la solicitud; se puede corregir]

  942430 [Restringe la detección de anomalías de caracteres de SQL (args): número de caracteres especiales que se han excedido (12)]

  920230 (Varias codificaciones de direcciones URL detectadas)

  942130 (Ataque por inyección de código SQL: Tautología de SQL detectada).

  931130 [Posible ataque remoto de inclusión de archivos (RFI) = Referencia o vínculo fuera del dominio]


## <a name="next-steps"></a>Pasos siguientes
Si necesita más ayuda, publique su problema en el [foro de Stack Overflow](https://stackoverflow.com/questions/tagged/azure-eventgrid) o abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/). 
