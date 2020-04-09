---
title: Configuración de Apple Push Notification Service en Azure Notification Hubs | Microsoft Docs
description: Aprenda a definir un centro de notificaciones de Azure con la configuración de Apple Push Notification Service (APNS).
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: eb1122ba3de0002507589d3e607d1e39d905c308
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127518"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Apple Push Notification Service para un centro de notificaciones en Azure Portal

En este artículo podrá ver cómo establecer la configuración de Apple Push Notification Service (APNS) para un centro de notificaciones de Azure mediante Azure Portal. 

## <a name="prerequisites"></a>Prerrequisitos
Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md). 

## <a name="configure-apple-push-notification-service"></a>Configuración de Apple Push Notification Service

El siguiente procedimiento le proporciona los pasos para establecer la configuración de Apple Push Notification Service (APNS) para un centro de notificaciones:

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Apple (APNS)** en el menú izquierdo.

1. Para **Modo de autenticación**, seleccione **Certificado** o **Token**.

   a. Si ha seleccionado **Certificado**:
   * Seleccione el icono de archivo, y luego el archivo *.p12* que desea cargar.
   * Escriba una contraseña.
   * Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del certificado APNS en Azure Portal](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   b. Si selecciona **Token**:

   * Especifique los valores **Id. de clave**, **Id. de agrupación**, **Id. de equipo** y **Token**.
   * Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del token APNS en Azure Portal](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Pasos siguientes
Para ver un tutorial con instrucciones paso a paso para insertar notificaciones en dispositivos iOS, consulte el artículo siguiente: [Envío de notificaciones push a aplicaciones iOS mediante Notification Hubs y APNS](notification-hubs-ios-apple-push-notification-apns-get-started.md)
