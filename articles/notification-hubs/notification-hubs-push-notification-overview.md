---
title: ¿Qué es Azure Notification Hubs?
description: Aprenda a agregar funcionalidades de notificación push con Azure Notification Hubs.
author: sethmanheim
manager: femila
editor: tjsomasundaram
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 03/23/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 04/30/2019
ms.openlocfilehash: f5b59d1669d89c73b93199d1f833da149003d399
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80159343"
---
# <a name="what-is-azure-notification-hubs"></a>¿Qué es Azure Notification Hubs?

Azure Notification Hubs proporciona un motor de inserción de escalabilidad horizontal y fácil uso que le permite enviar notificaciones a cualquier plataforma (iOS, Android, Windows, etc.) desde cualquier back-end (en la nube o local). Notification Hubs funciona muy bien tanto para escenarios empresariales como de consumidores. Estos son algunos escenarios de ejemplo:

- Enviar notificaciones de noticias de última hora a millones de usuarios con baja latencia.
- Enviar cupones basados en la ubicación a segmentos de usuarios interesados.
- Enviar notificaciones relacionadas con eventos a usuarios o grupos para aplicaciones de medios, deportivas, de finanzas o de juegos.
- Inserte contenido promocional en aplicaciones para ponerse en contacto y comercializar con clientes.
- Informar a los usuarios sobre eventos empresariales como nuevos mensajes o elementos de trabajo.
- Enviar códigos para Multi-Factor Authentication.

## <a name="what-are-push-notifications"></a>¿Qué son las notificaciones push?

Las notificaciones push son una forma de comunicación de aplicación a usuario, en la que los usuarios de aplicaciones móviles reciben notificaciones que contienen determinada información que el usuario desea, esta generalmente se muestra en un cuadro de diálogo o una ventana emergente en el dispositivo móvil. Normalmente, los usuarios eligen ver o descartar el mensaje; si eligen lo primero, se abrirá la aplicación móvil que les ha comunicado la notificación. Algunas notificaciones son silenciosas, es decir, que se entregan en segundo plano para que la aplicación las procese en segundo plano y decida qué hacer.

Las notificaciones push son vitales para las aplicaciones de consumidor, ya que aumentan el uso y la interacción de las aplicaciones, así como para las aplicaciones empresariales, ya que comunican información empresarial actualizada. Es la mejor forma de comunicación de aplicación a usuario porque ahorra energía para los dispositivos móviles, es flexible para los remitentes de notificaciones y está disponible cuando las aplicaciones correspondientes no están activas.

> [!NOTE]
> Azure Notification Hubs no es compatible oficialmente con las notificaciones push de Voz sobre IP (VoIP); sin embargo, [este artículo describe cómo puede usar las notificaciones de VoIP de APNS](voip-apns.md) mediante Azure Notification Hubs.

Para obtener más información acerca de las notificaciones push de algunas plataformas populares, consulte los temas siguientes:

- [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
- [iOS](https://developer.apple.com/notifications/)
- [Windows](https://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-do-push-notifications-work"></a>Funcionamiento de las notificaciones push

Las notificaciones push se entregan a través de unas infraestructuras específicas para la plataforma llamadas *Sistemas de notificación de plataforma* (PNS). Ofrecen funcionalidades de inserción básicas para la entrega de un mensaje a un dispositivo con un manipulador que se proporciona y no tienen ninguna interfaz común. Para enviar una notificación a todos los clientes en versiones de Android, iOS y Windows de una aplicación, el desarrollador tiene que trabajar por separado con Apple Push Notification Service (APNS), Firebase Cloud Messaging (FCM) y el Servicio de notificaciones de Windows (WNS).

A continuación se muestra cómo funciona la inserción, a nivel general:

1. Una aplicación desea recibir una notificación, por lo que se pone en contacto con el PNS para la plataforma de destino en la que la aplicación se está ejecutando y solicita un manipulador de inserción único y temporal. El tipo de identificador depende del sistema (por ejemplo, WNS usa URI, mientras que APNS usa tokens).
2. La aplicación cliente almacena este identificador en el back-end o el proveedor de la aplicación.
3. Para enviar una notificación push, el back-end de la aplicación se pone en contacto con el PNS a través del identificador para dirigirse a una aplicación cliente específica.
4. El PNS reenvía la notificación al dispositivo que especifica el identificador.

![Flujo de trabajo de las notificaciones push](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>Los desafíos de las notificaciones push

Los PNS son potentes. No obstante, exigen mucho trabajo al desarrollador de aplicaciones para poder implementar incluso escenarios comunes de notificaciones push, como la difusión de notificaciones push a usuarios segmentados.

El envío de notificaciones push requiere una infraestructura compleja que no está relacionada con la lógica de negocios principal de la aplicación. Algunos de los desafíos que presenta la infraestructura son:

- **Dependencia de la plataforma**
  - El back-end requiere una lógica, dependiente de la plataforma, compleja y difícil de mantener para enviar notificaciones a dispositivos de distintas plataformas, ya que los PNS no están unificados.
- **Escala**
  - Según las directrices de PNS, los tokens de dispositivo se tienen que actualizar cada vez que se inicia la aplicación. El back-end trata con una gran cantidad de tráfico y acceso de bases de datos solo para mantener actualizados los tokens. Cuando el número de dispositivos aumenta a centenares, millares o millones, la creación y el mantenimiento de esta infraestructura supone un costo enorme.
  - La mayoría de los PNS no son compatibles con la difusión a varios dispositivos. Una simple difusión a un millón de dispositivos genera un millón de llamadas a los PNS. El escalado de esta cantidad de tráfico con una latencia mínima no es algo trivial.
- **Enrutamiento**
  - Aunque los PNS proporcionan una manera de enviar mensajes a dispositivos, la mayoría de las notificaciones de aplicaciones se dirigen a usuarios o grupos de interés. El back-end debe mantener un registro para asociar dispositivos a grupos de interés, usuarios, propiedades, etc. Esta sobrecarga se agrega al tiempo de comercialización total y a los costos de mantenimiento de una aplicación.

## <a name="why-use-azure-notification-hubs"></a>Motivos para usar Azure Notification Hubs

Notification Hubs elimina todas las complejidades asociadas al envío de notificaciones push por su cuenta desde el back-end de su aplicación. Su infraestructura de notificaciones push multiplataforma y escalada horizontalmente reduce la codificación relativa a la inserción y simplifica el back-end. Con Notification Hubs, los dispositivos solo son responsables de registrar identificadores de PNS con un centro, mientras que el back-end envía mensajes a usuarios o grupos de interés, tal como se muestra en la ilustración siguiente:

![Diagrama de Notification Hubs](./media/notification-hubs-overview/notification-hub-diagram.png)

Notification Hubs es un motor de inserción listo para usar que presenta las siguientes ventajas:

- **Multiplataforma**
  - Compatibilidad con las principales plataformas de inserción.
  - Una interfaz común para insertar en todas las plataformas en formatos específicos de la plataforma o independientes de esta sin ningún tipo de trabajo específico de la plataforma.
  - Administración de controladores de dispositivos en un único lugar.
- **Back-ends cruzados**
  - En la nube o local.
  - .NET, Node.js, Java, Python, etc.
- **Conjunto completo de patrones de entrega**
  - Difusión para una o más plataformas: puede difundir al instante a millones de dispositivos entre plataformas con una sola llamada API.
  - Inserción en dispositivo: puede destinar las notificaciones a dispositivos individuales.
  - Inserción en usuario: las etiquetas y plantillas le ayudan a llegar a todos los dispositivos multiplataforma de un usuario.
  - Inserción en segmento con etiquetas dinámicas: la característica de etiquetas le ayuda a segmentar dispositivos e insertar en ellos en función de sus necesidades, tanto si va a realizar un envío a un segmento o a una expresión de segmentos (por ejemplo: activo AND reside en Seattle NOT usuario nuevo). En lugar de limitarse a la publicación-suscripción, puede actualizar las etiquetas de dispositivo en cualquier lugar y en cualquier momento.
  - Inserción localizada: la característica de plantillas le ayuda a obtener la localización sin alterar el código de back-end.
  - Inserción silenciosa: puede habilitar el modelo de inserción a extracción enviando notificaciones silenciosas a dispositivos y desencadenándolos para que realicen ciertas extracciones o acciones.
  - Inserción programada: puede programar que las notificaciones se envíen en cualquier momento.
  - Inserción directa: puede omitir el registro de dispositivos con el servicio Notification Hubs y procesar por lotes directamente las inserciones a una lista de identificadores de dispositivo.
  - Inserción personalizada: las variables de inserción de dispositivo le ayudan a enviar notificaciones push personalizadas específicas de dispositivo con pares de clave-valor personalizados.
- **Telemetría enriquecida**
  - La telemetría de inserción general, dispositivo, error y operación está disponible tanto en Azure Portal como mediante programación.
  - La telemetría por mensaje realiza el seguimiento de cada inserción desde la llamada de solicitud inicial al envío correcto de las inserciones por Notification Hubs.
  - Los comentarios del Sistema de notificación de plataforma comunican todos los comentarios de los PNS para ayudar en la depuración.
- **Escalabilidad**
  - Enviar mensajes rápidos a millones de dispositivos sin tener que volver a diseñar la arquitectura o realizar el particionamiento del dispositivo.
- **Seguridad**
  - Firma de acceso compartido (SAS) o autenticación federada.

## <a name="next-steps"></a>Pasos siguientes

Conozca la información para la creación y el uso de un centro de notificaciones en [Tutorial: Envío de notificaciones push a aplicaciones móviles](notification-hubs-android-push-notification-google-fcm-get-started.md).

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png

[How customers are using Notification Hubs]: https://azure.microsoft.com/services/notification-hubs
[Notification Hubs tutorials and guides]: https://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: https://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Universal]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: https://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: https://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: https://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure portal]: https://portal.azure.com
[tags]: (https://msdn.microsoft.com/library/azure/dn530749.aspx)
