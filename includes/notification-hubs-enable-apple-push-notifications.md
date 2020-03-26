---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: sethmanheim
ms.service: notification-hubs
ms.topic: include
ms.date: 02/10/2020
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: bf2596f5a8e287799285f97f3d1be9f3fe10f644
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77123169"
---
## <a name="generate-the-certificate-signing-request-file"></a>Generación del archivo de solicitud de firma de certificado

El servicio Apple Push Notification Service (APNS) utiliza certificados para autenticar las notificaciones push. Siga estas instrucciones para crear el certificado de inserción necesario para enviar y recibir notificaciones. Para obtener más información sobre estos conceptos, consulte la documentación oficial de [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) (Servicio de notificaciones push de Apple).

Genere el archivo de solicitud de firma de certificado (CSR) que utiliza Apple para generar un certificado push firmado.

1. En el Mac, ejecute la herramienta Acceso a llaves. Se puede activar desde la carpeta **Utilities** (Utilidades) o la carpeta **Other** (Otros) en el panel de inicio.

1. Seleccione **Keychain Access** (Acceso con cadena de claves), expanda **Certificate Assistant** (Asistente para certificados) y, después, seleccione **Request a Certificate from a Certificate Authority** (Solicitar un certificado a una entidad de certificación).

    ![Use Acceso a llaves para solicitar un nuevo certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

   > [!NOTE]
   > De forma predeterminada, Keychain Access (Acceso con cadena de claves) selecciona el primer elemento de la lista. Esto puede ser un problema si está en la categoría **Certificates** (Certificados) y **Apple Worldwide Developer Relations Certification Authority** (Entidad de certificación de Apple Worldwide Developer Relations) no es el primer elemento de la lista. Antes de generar la solicitud de firma de certificado, asegúrese de que tiene un elemento que no sea clave o de que esté seleccionada la clave **Apple Worldwide Developer Relations Certification Authority** (Entidad de certificación de Apple Worldwide Developer Relations).

1. Seleccione una **dirección de correo electrónico de usuario**, escriba un valor en **Common Name** (Nombre común), asegúrese de que especifica **Saved to disk** (Guardar en disco) y, finalmente, seleccione **Continue** (Continuar). Deje en blanco **CA Email Address** (Dirección de correo de la entidad de certificación), ya que no es obligatorio.

    ![Información obligatoria del certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. Escriba un nombre para el archivo CSR en **Save As** (Guardar como), seleccione la ubicación en **Where** (Dónde) y, a continuación, seleccione **Save** (Guardar).

    ![Elección de un nombre de archivo para el certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Esta acción guarda el archivo CSR en la ubicación seleccionada. La ubicación predeterminada es **Escritorio**. Recuerde la ubicación seleccionada para este archivo.

A continuación, se registrará la aplicación en Apple, se habilitarán las notificaciones push y se cargará el archivo CSR exportado para crear un certificado de inserción.

## <a name="register-your-app-for-push-notifications"></a>Registro de la aplicación para notificaciones push

Para enviar notificaciones push a una aplicación iOS, registre la aplicación en Apple y también regístrese para el uso de notificaciones push.  

1. Si todavía no ha registrado la aplicación, vaya al [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456) en el centro para desarrolladores de Apple. Inicie sesión en el portal con su identificador de Apple y seleccione **Identifiers** (Identificadores). Luego, seleccione **+** para registrar una nueva aplicación.

    ![Página de identificadores de aplicaciones del portal de aprovisionamiento de iOS](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. En la pantalla **Register a New Identifier** (Registrar un nuevo identificador), seleccione el botón de radio **App IDs** (Id. de aplicaciones). Después, seleccione **Continuar**.

    ![Portal de aprovisionamiento de iOS: página de registro de un nuevo identificador](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids-new.png)

3. Actualice los tres campos siguientes de la nueva aplicación y, después, seleccione **Continue** (Continuar):

   * **Descripción**: escriba un nombre descriptivo para la aplicación.

   * **Identificador de agrupación**: escriba un identificador de agrupación con el formato **Identificador de organización.Nombre del producto**, como se mencionó en la [Guía de distribución de aplicaciones](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). El *identificador de organización* y el *nombre del producto* deben coincidir con el identificador de la organización y con el nombre del producto que use al crear el proyecto Xcode. En la captura de pantalla que aparece a continuación, el valor de **NotificationHubs** se usa como identificador de una organización, mientras que el valor **GetStarted** se usa como el nombre del producto. Asegúrese de que el valor de **Bundle Identifier** (Identificador de agrupación) coincida con el del proyecto de Xcode para que Xcode use el perfil de publicación correcto.

      ![Portal de aprovisionamiento de iOS: página de registro del identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-bundle.png)

   * **Notificaciones push**: marque la opción **Push Notifications** (Notificaciones push) en la sección **Capabilities** (Capacidades).

      ![Formulario para registrar un nuevo identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-push.png)

      De esta forma, se genera el identificador de la aplicación y se solicita que confirme la información. Seleccione **Continue** (Continuar) y, luego, **Register** (Registrar) para confirmar el nuevo identificador de aplicación.

      ![Confirmación del nuevo identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-register.png)

      Tras seleccionar **Register**, (Registrar), verá el nuevo identificador de aplicación como un elemento de línea en la página **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles).

4. En esta página, en **Identifiers** (Identificadores), busque el elemento de línea con el identificador de aplicación que acaba de crear y seleccione su fila para mostrar la pantalla **Edit your App ID Configuration** (Editar la configuración del id. de aplicación).

5. Desplácese hacia abajo hasta la opción activada **Push Notifications** (Notificaciones push) y, luego, seleccione **Configure** (Configurar) para crear el certificado.

    ![Editar página de identificadores de aplicaciones](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

6. Aparece la ventana **Apple Push Notification service SSL Certificates** (Certificados SSL de Apple Push Notification Service). Seleccione el botón **Create Certificate** (Crear certificado) en la sección **Development SSL Certificate** (Certificado SSL de desarrollo).

    ![Crear certificado para el botón de identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Se muestra la pantalla **Create a new Certificate** (Crear un nuevo certificado).

    > [!NOTE]
    > Este tutorial usa un certificado de desarrollo. Se usa el mismo proceso cuando se registra un certificado de producción. Asegúrese de usa el mismo tipo de certificado cuando envíe notificaciones.

1. Seleccione **Choose File** (Elegir archivo), vaya a la ubicación donde guardó el archivo CSR de la primera tarea y, después, haga doble clic en el nombre del certificado para cargarlo. Después, seleccione **Continuar**.

1. Una vez que el portal cree el certificado, seleccione el botón **Download** (Descargar). Guarde el certificado y recuerde la ubicación donde se ha guardado.

    ![Página de descarga de certificados generados](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    El certificado se descarga y se guarda en el equipo en la carpeta de **descargas**.

    ![Busque el archivo de certificado en la carpeta de descargas](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > De manera predeterminada, el certificado de desarrollo descargado se denomina **aps_development.cer**.

1. Haga doble clic en el certificado de inserción **aps_development.cer** descargado. Esta operación instala el certificado nuevo en la cadena de llaves, tal como se muestra en la siguiente imagen:

    ![Lista de certificados de Acceso a llaves en la que aparece el nuevo certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Aunque el nombre del certificado puede ser diferente, llevará el prefijo **Apple Development iOS Push Services** (Servicios push de desarrollo de Apple iOS).

1. En Acceso a llaves, haga clic en el nuevo certificado de inserción que creó, en la categoría **Certificados** . Haga clic en **Export** (Exportar), asigne un nombre al archivo, seleccione el formato **.p12** y, después, haga clic en **Save** (Guardar).

    ![Exportación del certificado en formato p12](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    De manera opcional, puede decidir proteger el certificado con una contraseña. Haga clic en **OK** (Aceptar) si quiere omitir la creación de una contraseña. Anote el nombre de archivo y la ubicación del certificado p12 exportado. Se usan para habilitar la autenticación con APNs.

    > [!NOTE]
    > El nombre y la ubicación del archivo .p12 pueden ser diferentes a los que se ilustran en este tutorial.

## <a name="create-a-provisioning-profile-for-the-app"></a>Creación de un perfil de aprovisionamiento para la aplicación

1. Vuelva al [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456), seleccione **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles), seleccione **Profiles** (Perfiles) en el menú izquierdo y, luego, seleccione **+** para crear un perfil. Aparece la pantalla **Register a New Provisioning Profile** (Registrar un nuevo perfil de aprovisionamiento).

1. En **Development** (Desarrollo), seleccione **iOS App Development** (Desarrollo de aplicaciones de iOS) como tipo de perfil de aprovisionamiento y, después, haga clic en **Continue** (Continuar).

    ![Lista de perfiles de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Una vez hecho esto, seleccione el identificador de la aplicación que acaba de crear en la lista desplegable **App ID** (Id. de la aplicación) y, a continuación, seleccione **Continuar**.

    ![Seleccionar el identificador de la aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. En la ventana **Select certificates** (Seleccionar certificados), seleccione el certificado de desarrollo que se usa para la firma del código y, después, seleccione **Continue** (Continuar). Este certificado no es el certificado push que creó. Si no hay uno, debe crearlo. Si hay un certificado, vaya al paso siguiente. Para crear un certificado de desarrollo si no existe uno:

    1. Si ve **No Certificates are available** (No hay certificados disponibles), seleccione **Create Certificate**(Crear certificado).
    2. En la sección **Software**, seleccione **Apple Development** (Desarrollo de Apple). Después, seleccione **Continuar**.
    3. En la pantalla **Create a new Certificate** (Crear un nuevo certificado), seleccione **Choose File** (Elegir archivo).
    4. Vaya al certificado **Certificate Signing Request** (Solicitud de firma de certificado), selecciónelo y, luego, elija **Open** (Abrir).
    5. Seleccione **Continuar**.
    6. Descargue el certificado de desarrollo y recuerde la ubicación donde se ha guardado.

1. Vuelva a la página **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles), seleccione **Profiles** (Perfiles) en el menú izquierdo y, luego, seleccione **+** para crear un perfil. Aparece la pantalla **Register a New Provisioning Profile** (Registrar un nuevo perfil de aprovisionamiento).

1. En la ventana **Select certificates** (Seleccionar certificados), seleccione el certificado de desarrollo que acaba de crear. Después, seleccione **Continuar**.

1. Después, seleccione en los dispositivos que va a usar para la prueba y seleccione **Continue** (Continuar).

1. Para terminar, elija un nombre para el perfil en **Provisioning Profile Name** (Nombre del perfil de aprovisionamiento) y seleccione **Generate** (Generar).

    ![Elegir el nombre del perfil de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Cuando se cree el perfil de aprovisionamiento, seleccione **Download** (Descargar). Recuerde la ubicación donde se ha guardado.

1. Vaya a la ubicación del perfil de aprovisionamiento y haga doble clic en él para instalarlo en la máquina de desarrollo de Xcode.

## <a name="create-a-notification-hub"></a>Creación de un centro de notificaciones

En esta sección, va a crear un centro de notificaciones y configurar la autenticación con APNs mediante el certificado push .p12 que creó anteriormente. Si desea usar un centro de notificaciones ya creado, puede omitir los pasos hasta el paso 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>Configuración de un centro de notificaciones con información de APNs

1. En **Notification Services** (Servicios de notificación), seleccione **Apple (APNS)** .

1. Seleccione **Certificado**.

1. Seleccione el icono del archivo.

1. Seleccione el archivo .p12 que exportó antes y, luego, **Open** (Abrir).

1. Si es necesario, especifique la contraseña correcta.

1. Seleccione el modo **Espacio aislado**. Use el modo **Producción** solo si desea enviar notificaciones push a los usuarios que compraron la aplicación en la tienda.

    ![Configuración del certificado de APNs en Azure Portal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-apple-config-cert.png)

1. Seleccione **Guardar**.

Ahora ya ha configurado el centro de notificaciones con APNs. Tiene también la cadena de conexión para registrar la aplicación y enviar notificaciones push.
