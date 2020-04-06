---
title: Azure Notification Hubs y migración de Google Firebase Cloud Messaging (FCM)
description: Describe cómo aborda Azure Notification Hubs la migración de Google GCM y FCM.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/10/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 04/10/2019
ms.openlocfilehash: 2f2ca4b56445b3f399477e396de579d8a8c539e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127020"
---
# <a name="azure-notification-hubs-and-google-firebase-cloud-messaging-migration"></a>Azure Notification Hubs y migración de Google Firebase Cloud Messaging

## <a name="current-state"></a>Estado actual

Cuando Google anunció su migración de Google Cloud Messaging (GCM) a Firebase Cloud Messaging (FCM), los servicios de inserción como los nuestros tuvieron que ajustar la manera de enviar notificaciones a dispositivos Android para adaptarse a este cambio.

Se actualizó el back-end del servicio y luego se publicaron las actualizaciones necesarias para las API y los SDK. Con nuestra implementación, tomamos la decisión de mantener la compatibilidad con los esquemas de notificación existentes de GCM a fin de minimizar el impacto para el cliente. Esto significa que actualmente enviamos las notificaciones a dispositivos Android mediante FCM en el modo heredado de FCM. En última instancia, queremos agregar compatibilidad real para FCM, incluidas las nuevas características y el formato de carga útil. Este es un cambio a largo plazo y la migración actual se centra en mantener la compatibilidad con las aplicaciones y los SDK existentes. Puede usar los SDK de GCM o FCM de su aplicación (junto con nuestro SDK) y nos aseguraremos de que la notificación se envíe correctamente.

Algunos clientes recibieron recientemente un correo electrónico de Google que les advertía acerca de las aplicaciones que usan un punto de conexión GCM para las notificaciones. Era una simple advertencia y no pasó nada: las notificaciones de Android de la aplicación todavía se envían a Google para su procesamiento y Google todavía las procesa. Algunos clientes que especifican el punto de conexión GCM explícitamente en la configuración de servicio todavía utilizaban el punto de conexión en desuso. Ya habíamos identificado esta brecha y estábamos trabajando para solucionar el problema cuando Google envió el correo electrónico.

Reemplazamos ese punto de conexión en desuso y la corrección ya está implementada.

## <a name="going-forward"></a>A partir de ahora

Según las Preguntas frecuentes de Google FCM, no tiene que hacer nada. En las [P+F de FCM](https://developers.google.com/cloud-messaging/faq), Google afirma que los "SDK de cliente y los tokens de GCM seguirán funcionando indefinidamente. Sin embargo, no podrá usar la versión más reciente de Google Play Services en su aplicación de Android a menos que migre a FCM."

Si su aplicación usa la biblioteca de GCM, continúe y siga las instrucciones de Google para actualizar a la biblioteca de FCM en la aplicación. Nuestro SDK es compatible con cualquiera de las opciones, por lo que por nuestra parte no tendrá que actualizar nada en la aplicación (siempre y cuando tenga actualizada la versión del SDK).

## <a name="questions-and-answers"></a>Preguntas y respuestas

A continuación, se muestran algunas respuestas a preguntas habituales que hemos recibido de los clientes:

**P:** ¿Qué tengo que hacer para tener compatibilidad antes de la fecha límite (la fecha límite actual de Google es el 29 de mayo y puede cambiar)?

**R:** Nada. Se mantendrá la compatibilidad con el esquema de notificaciones de GCM existente. La clave de GCM seguirá funcionando con normalidad, así como todos los SDK y las bibliotecas de GCM que se usan en la aplicación.

Si decide actualizar a los SDK y las bibliotecas de FCM para aprovechar las nuevas características, su clave de GCM seguirá funcionando. Puede cambiar al uso de una clave de FCM si lo desea, pero asegúrese de agregar Firebase a su proyecto de GCM existente al crear el nuevo proyecto de Firebase. De esta forma garantizará la compatibilidad con versiones anteriores a los clientes que ejecutan versiones anteriores de la aplicación y que aún utilizan las bibliotecas y los SDK de GCM.

Si está creando un nuevo proyecto de FCM y no lo asocia al proyecto de GCM existente, cuando actualice Notification Hubs con el nuevo secreto de FCM, perderá la capacidad de insertar notificaciones a las instalaciones actuales de la aplicación, dado que la nueva clave de FCM no tiene ningún vínculo al proyecto de GCM antiguo.

**P:** ¿Por qué recibo este correo electrónico sobre los puntos de conexión de GCM antiguos que se utilizan? ¿Qué debo hacer?

**R:** Nada. Hemos realizado la migración a los nuevos puntos de conexión, que terminará pronto, por lo que no es necesario ningún cambio. No ha sucedido nada. El punto de conexión omitido solo ha causado mensajes de advertencia de Google.

**P:** ¿Cómo puedo realizar la transición a las bibliotecas y los SDK de FCM nuevos sin que ello afecte a los usuarios existentes?

A. Realice la actualización en cualquier momento. Google no ha anunciado aún ninguna retirada de las bibliotecas y los SDK de GCM existentes. Para asegurarse de no interrumpir las notificaciones de inserción a los usuarios existentes, cuando cree el nuevo proyecto de Firebase asegúrese de asociarlo al proyecto de GCM existente. De este modo se asegurará de que los nuevos secretos de Firebase funcionan para los usuarios que ejecutan versiones anteriores de la aplicación con SDK y bibliotecas de GCM, así como para los nuevos usuarios de su aplicación con SDK y bibliotecas de FCM.

**P:** ¿Cuándo puedo utilizar las características y los esquemas de FCM nuevos para mis notificaciones?

**R:** Cuando publiquemos una actualización para nuestros SDK y nuevas API, esté atento: tendremos algo para usted en los próximos meses.
