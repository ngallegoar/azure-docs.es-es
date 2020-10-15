---
title: Envío de notificaciones de VoIP de APNS con Azure Notification Hubs
description: Aprenda a enviar notificaciones de VoIP de APNS a través de Azure Notification Hubs (no compatible oficialmente).
author: sethmanheim
ms.author: sethm
ms.date: 3/23/2020
ms.topic: how-to
ms.service: notification-hubs
ms.openlocfilehash: c99af881b8f93b75633741c2352dc5df17dd2963
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80137652"
---
# <a name="use-apns-voip-through-notification-hubs-not-officially-supported"></a>Uso de VoIP de APNS a través de Notification Hubs (no compatible oficialmente)

Es posible usar las notificaciones de VoIP de APNS a través de Azure Notification Hubs; sin embargo, no hay soporte técnico oficial para este escenario.

## <a name="considerations"></a>Consideraciones

Si aún así elige enviar notificaciones de VoIP de APNS a través de Notification Hubs, tenga en cuenta estas limitaciones:

- El envío de una notificación de VoIP requiere que el encabezado `apns-topic` se establezca en el id. del lote de aplicaciones más el sufijo `.voip`. Por ejemplo, para una aplicación de ejemplo con el id. de lote `com.microsoft.nhubsample`, el encabezado `apns-topic` se debe establecer en `com.microsoft.nhubsample.voip.`.

   Este método no funciona bien con Azure Notification Hubs, porque el id. del lote de la aplicación se debe configurar como parte de las credenciales de APNS del centro y el valor no se puede cambiar. Además, Notification Hubs no permite que el valor del encabezado `apns-topic` se invalide en tiempo de ejecución.

   Para enviar notificaciones de VoIP, debe configurar un centro de notificaciones independiente con el id. del lote de aplicaciones `.voip`.

- El envío de una notificación de VoIP requiere que el encabezado `apns-push-type` se establezca en el valor `voip`.

   Para ayuda a los clientes con la transición a iOS 13, Notification Hubs intenta inferir el valor correcto del encabezado `apns-push-type`. La lógica de inferencia es deliberadamente sencilla, en un esfuerzo por evitar interrumpir las notificaciones estándar. Lamentablemente, este método provoca problemas con las notificaciones de VoIP, ya que Apple trata las notificaciones de VoIP como un caso especial que no sigue las mismas reglas que las notificaciones estándar.

   Para enviar notificaciones de VoIP, debe especificar un valor explícito para el encabezado `apns-push-type`.

- Notification Hubs limita las cargas de APNS a 4 KB, tal como se documenta en Apple. En el caso de las notificaciones de VoIP, Apple permite cargas de hasta 5 KB. Notification Hubs no distingue entre las notificaciones estándar y las notificaciones de VoIP; por lo tanto, todas las notificaciones están limitadas a 4 KB.

   Para enviar notificaciones de VoIP, no debe superar el límite de tamaño de carga de 4 KB.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes vínculos:

- [Documentación para los encabezados `apns-topic` y `apns-push-type` y los valores, incluidos casos especiales para las notificaciones de VoIP](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns).

- [Documentación sobre el límite de tamaño de la carga](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification).

- [Actualizaciones de Notification Hubs para iOS 13](push-notification-updates-ios-13.md#apns-push-type).
