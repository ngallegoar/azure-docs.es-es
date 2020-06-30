---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 71e4b0b3b7bf84938123acf70ac18a93c3015a4d
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095512"
---
### <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging-for-android"></a>Crear un proyecto de Firebase y habilitar Firebase Cloud Messaging para Android

1. Inicie sesión en la [consola Firebase](https://firebase.google.com/console/). Cree un nuevo proyecto de Firebase escribiendo **PushDemo** como **nombre del proyecto**.

    > [!NOTE]
    > Se generará un nombre único. De forma predeterminada, se compone de una variante en minúsculas del nombre que proporcionó más un número generado separado mediante un guión. Puede cambiarlo si lo desea siempre que sea globalmente único.

1. Después de crear el proyecto, seleccione **Add Firebase to your Android app** (Añade Firebase a tu aplicación de Android).

    ![Agregar Firebase a una aplicación de Android](./media/notification-hubs-common-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)

1. En la página **Add Firebase to your Android app** (Agregar Firebase a la aplicación Android), haga lo siguiente.
    1. En **Android package name** (Nombre del paquete Android), escriba un nombre para el paquete. Por ejemplo: `com.<organization_identifier>.<package_name>`.

        ![Especifique el nombre del paquete.](./media/notification-hubs-common-enable-firebase-cloud-messaging/specify-package-name-fcm-settings.png)
    1. Seleccione **Registrar aplicación**.  
    1. Seleccione **Download google-services.json** (Descargar google-services.json). A continuación, guarde el archivo en una carpeta local para un uso posterior y seleccione **Siguiente**.  

        ![Descargue el archivo google-services.json.](./media/notification-hubs-common-enable-firebase-cloud-messaging/download-google-service-button.png)
    1. Seleccione **Next** (Siguiente).
    1. Seleccione **Continue to console**(Continuar en la consola).

        > [!NOTE]
        > Si el botón **Continue to console** (Continuar en la consola) no está habilitado debido a la *comprobación de la instalación*, elija **Omitir este paso**.

1. En la consola Firebase, seleccione el icono de la rueda dentada del proyecto. Luego, seleccione **Project Settings** (Configuración del proyecto).

    ![Seleccionar Project Settings (Configuración del proyecto)](./media/notification-hubs-common-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)

    > [!NOTE]
    > Si no ha descargado el archivo **google-services.json**, puede hacerlo en esta página.

1. Cambie a la pestaña **Cloud Messaging** de la parte superior. Copie y guarde la **clave del servidor** para su uso posterior. Este valor se utilizará para configurar el centro de notificaciones.

    ![Copia de la clave del servidor](./media/notification-hubs-common-enable-firebase-cloud-messaging/server-key.png)
