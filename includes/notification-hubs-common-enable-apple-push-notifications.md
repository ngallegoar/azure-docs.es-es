---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 72065ce602c6d29255a3500e26d8c6f36a19ebed
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095511"
---
### <a name="register-your-ios-app-for-push-notifications"></a>Registro de una aplicación iOS para notificaciones push

Para enviar notificaciones push a una aplicación iOS, registre la aplicación en Apple y también regístrese para el uso de notificaciones push.  

1. Si todavía no ha registrado la aplicación, vaya al [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456) en el centro para desarrolladores de Apple. Inicie sesión en el portal con su identificador de Apple, vaya a **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles) y, a continuación, seleccione **Identifiers** (Identificadores). Haga clic en **+** para registrar una nueva aplicación.

    ![Página de identificadores de aplicaciones del portal de aprovisionamiento de iOS](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-ios-appids.png)

1. En la pantalla **Register a New Identifier** (Registrar un nuevo identificador), seleccione el botón de radio **App IDs** (Id. de aplicaciones). Después, seleccione **Continuar**.

    ![Portal de aprovisionamiento de iOS: página de registro de un nuevo identificador](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-ios-appids-new.png)

1. Actualice los tres campos siguientes de la nueva aplicación y, después, seleccione **Continue** (Continuar):

   * **Descripción**: escriba un nombre descriptivo para la aplicación.

   * **Identificador de agrupación**: Escriba un identificador de lote con el formato **com.<identificadorDeOrganización>.<nombreDeProducto>** , como se menciona en la [guía de distribución de aplicaciones](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). En la captura de pantalla que aparece a continuación, el valor `mobcat` se usa como identificador de una organización, mientras que el valor **PushDemo** se usa como el nombre del producto.

      ![Portal de aprovisionamiento de iOS: página de registro del identificador de aplicación](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-bundle.png)

   * **Notificaciones push**: marque la opción **Push Notifications** (Notificaciones push) en la sección **Capabilities** (Capacidades).

      ![Formulario para registrar un nuevo identificador de aplicación](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-push.png)

      De esta forma, se genera el identificador de la aplicación y se solicita que confirme la información. Seleccione **Continue** (Continuar) y, luego, **Register** (Registrar) para confirmar el nuevo identificador de aplicación.

      ![Confirmación del nuevo identificador de aplicación](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-register.png)

      Tras seleccionar **Register**, (Registrar), verá el nuevo identificador de aplicación como un elemento de línea en la página **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles).

1. En la página **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles) en **Identifiers** (Identificadores), busque el elemento de línea de identificador de aplicación que ha creado. A continuación, seleccione su fila para mostrar la pantalla **Edit your App ID Configuration** (Editar la configuración del identificador de aplicación).

#### <a name="creating-a-certificate-for-notification-hubs"></a>Creación de un certificado para Notification Hubs

Para permitir que el centro de notificaciones funcione con **Apple Push Notification Service (APNs)** es necesario un certificado que se puede proporcionar de una de estas dos maneras:

1. [Creación de un certificado push p12 que se pueda cargar directamente en Notification Hubs](#option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub) (*el enfoque original*)

1. [Creación de un certificado p8 que se puede usar para la autenticación basada en token](#option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication) (*el enfoque más reciente y recomendado*)

El enfoque más reciente tiene varias ventajas, como se documenta en [Autenticación basada en token (HTTP/2) para APNs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-http2-token-authentification). Requiere menos pasos, pero además se exige para ciertos escenarios concretos. De todas formas, se han proporcionado pasos para ambos enfoques, ya que ambos funcionan para los fines de este tutorial.

##### <a name="option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub"></a>OPCIÓN 1: Creación de un certificado push p12 que se pueda cargar directamente en Notification Hubs.

1. En el Mac, ejecute la herramienta Acceso a llaves. Se puede activar desde la carpeta **Utilities** (Utilidades) o la carpeta **Other** (Otros) en el panel de inicio.

1. Seleccione **Keychain Access** (Acceso con cadena de claves), expanda **Certificate Assistant** (Asistente para certificados) y, después, seleccione **Request a Certificate from a Certificate Authority** (Solicitar un certificado a una entidad de certificación).

    ![Use Acceso a llaves para solicitar un nuevo certificado](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

   > [!NOTE]
   > De forma predeterminada, Keychain Access (Acceso con cadena de claves) selecciona el primer elemento de la lista. Esto puede ser un problema si está en la categoría **Certificates** (Certificados) y **Apple Worldwide Developer Relations Certification Authority** (Entidad de certificación de Apple Worldwide Developer Relations) no es el primer elemento de la lista. Antes de generar la solicitud de firma de certificado, asegúrese de que tiene un elemento que no sea clave o de que esté seleccionada la clave **Apple Worldwide Developer Relations Certification Authority** (Entidad de certificación de Apple Worldwide Developer Relations).

1. Seleccione una **dirección de correo electrónico de usuario**, escriba un valor en **Common Name** (Nombre común), asegúrese de que especifica **Saved to disk** (Guardar en disco) y, finalmente, seleccione **Continue** (Continuar). Deje en blanco **CA Email Address** (Dirección de correo de la entidad de certificación), ya que no es obligatorio.

    ![Información esperada del certificado](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. Escriba un nombre para el **archivo de solicitud de firma de certificado (CSR)** en **Save As** (Guardar como), seleccione la ubicación en **Where** (Dónde) y, a continuación, haga clic en **Save** (Guardar).

    ![Elección de un nombre de archivo para el certificado](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Esta acción guarda el **archivo CSR** en la ubicación seleccionada. La ubicación predeterminada es **Escritorio**. Recuerde la ubicación seleccionada para este archivo.

1. De nuevo en la página **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles) en el [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456), desplácese hacia abajo hasta la opción **Push Notifications** (Notificaciones push) y, a continuación, seleccione **Configure** (Configurar) para crear el certificado.

    ![Editar página de identificadores de aplicaciones](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-edit-appid.png)

1. Aparece la ventana **Apple Push Notification service TLS/SSL Certificates** (Certificados TLS/SSL de Apple Push Notification Service). Seleccione el botón **Create Certificate** (Crear certificado) en la sección **Development TLS/SSL Certificate** (Certificado TLS/SSL de desarrollo).

    ![Crear certificado para el botón de identificador de aplicación](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Se muestra la pantalla **Create a new Certificate** (Crear un nuevo certificado).

    > [!NOTE]
    > Este tutorial usa un certificado de desarrollo. Se usa el mismo proceso cuando se registra un certificado de producción. Asegúrese de usa el mismo tipo de certificado cuando envíe notificaciones.

1. Seleccione **Choose File** (Elegir archivo), vaya a la ubicación donde guardó el **archivo CSR** y haga doble clic en el nombre del certificado para cargarlo. Después, seleccione **Continuar**.

1. Una vez que el portal cree el certificado, seleccione el botón **Download** (Descargar). Guarde el certificado y recuerde la ubicación donde se ha guardado.

    ![Página de descarga de certificados generados](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    El certificado se descarga y se guarda en el equipo en la carpeta de **descargas**.

    ![Busque el archivo de certificado en la carpeta de descargas](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > De manera predeterminada, el certificado de desarrollo descargado se denomina **aps_development.cer**.

1. Haga doble clic en el certificado de inserción **aps_development.cer** descargado. Esta operación instala el certificado nuevo en la cadena de llaves, tal como se muestra en la siguiente imagen:

    ![Lista de certificados de Acceso a llaves en la que aparece el nuevo certificado](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Aunque el nombre del certificado puede ser diferente, llevará el prefijo **Apple Development iOS Push Services** (Servicios push de desarrollo de Apple iOS) y tendrá el identificador del lote asociado.

1. En el acceso Keychain, **Control** + **Clic** en el nuevo certificado push que creó, en la categoría **Certificates** (Certificados). Seleccione **Export** (Exportar), asigne un nombre al archivo, seleccione el formato **p12** y, después, haga clic en **Save** (Guardar).

    ![Exportación del certificado en formato p12](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Puede proteger el certificado con una contraseña, pero el uso de contraseña es opcional. Haga clic en **OK** (Aceptar) si quiere omitir la creación de una contraseña. Anote el nombre de archivo y la ubicación del certificado p12 exportado. Estos datos se usan para habilitar la autenticación con APNs.

    > [!NOTE]
    > El nombre y la ubicación del archivo p12 pueden ser diferentes a los que se ilustran en este tutorial.

##### <a name="option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication"></a>OPCIÓN 2: Creación de un certificado p8 que se pueda usar para autenticación basada en tokens.

1. Anote los siguientes datos:

    * **App ID Prefix** (Prefijo de identificador de aplicación), se trata de un **identificador de equipo**
    * **Identificador de lote**

1. De vuelta en **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles), haga clic en **Keys** (Claves).

   > [!NOTE]
   > Si ya tiene configurada una clave para **APNs**, puede volver a usar el certificado p8 que descargó justo después de crearlo. En tal caso, puede omitir los pasos **3** a **5**.

1. Haga clic en el botón **+** [o en el botón **Create a key** (Crear una clave)] para crear una nueva clave.
1. Proporcione un valor de **Key Name** (Nombre de clave) adecuado, active la opción **Apple Push Notifications service (APNs)** y, a continuación, haga clic en **Continue** (Continuar), seguido de **Register** (Registrar) en la pantalla siguiente.
1. Haga clic en **Download** (Descargar) y, a continuación, mueva el archivo **p8** (con el prefijo *AuthKey_* ) a un directorio local seguro y, a continuación, haga clic en **Done** (Listo).

   > [!NOTE]
   > Asegúrese de mantener el archivo p8 en un lugar seguro (y guardar una copia de seguridad). Después de descargar la clave, no se puede volver a descargar, ya que se quita la copia del servidor.
  
1. En **Keys** (Claves), haga clic en la clave que ha creado (o en una clave existente si decide utilizarla en su lugar).
1. Anote el valor de **Key ID** (Id. de clave).
1. Abra el certificado p8 en una aplicación adecuada, como [**Visual Studio Code**](https://code.visualstudio.com). Apunte el valor clave (el que aparece entre **-----BEGIN PRIVATE KEY-----** y **-----END PRIVATE KEY-----** ).

    -----BEGIN PRIVATE KEY-----  
    <valorDeClave>  
    -----END PRIVATE KEY-----

    > [!NOTE]
    > Este es el **valor de token** que se usará más adelante para configurar el **centro de notificaciones**.

Al final de estos pasos debe tener la siguiente información para usarla más adelante en [Configuración de un centro de notificaciones con información de APNs](#configure-your-notification-hub-with-apns-information):

* **Identificador de equipo** (consulte el paso 1)
* **Identificador de lote** (consulte el paso 1)
* **Identificador de clave** (consulte el paso 7)
* **Valor de token** (el valor de clave de p8 obtenido en el paso 8)

### <a name="create-a-provisioning-profile-for-the-app"></a>Creación de un perfil de aprovisionamiento para la aplicación

1. Vuelva al [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456), seleccione **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles), seleccione **Profiles** (Perfiles) en el menú izquierdo y, luego, seleccione **+** para crear un perfil. Aparece la pantalla **Register a New Provisioning Profile** (Registrar un nuevo perfil de aprovisionamiento).

1. En **Development** (Desarrollo), seleccione **iOS App Development** (Desarrollo de aplicaciones de iOS) como tipo de perfil de aprovisionamiento y, después, haga clic en **Continue** (Continuar).

    ![Lista de perfiles de aprovisionamiento](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Una vez hecho esto, seleccione el identificador de la aplicación que acaba de crear en la lista desplegable **App ID** (Id. de la aplicación) y, a continuación, seleccione **Continuar**.

    ![Seleccionar el identificador de la aplicación](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. En la ventana **Select certificates** (Seleccionar certificados), seleccione el certificado de desarrollo que se usa para la firma del código y, después, seleccione **Continue** (Continuar).

    > [!NOTE]
    > Este certificado no es el certificado push que creó en el [paso anterior](#creating-a-certificate-for-notification-hubs). Este es el certificado de desarrollo. Si no existe uno, tiene que crearlo, ya que se trata de un [requisito previo](#prerequisites) para este tutorial. Los certificados de desarrollador se pueden crear en el [portal para desarrolladores de Apple](https://developer.apple.com), a través de [Xcode](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/YourFirstAppStoreSubmission/ProvisionYourDevicesforDevelopment/ProvisionYourDevicesforDevelopment.html) o en [Visual Studio](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/).

1. Vuelva a la página **Certificates, Identifiers & Profiles** (Certificados, identificadores y perfiles), seleccione **Profiles** (Perfiles) en el menú izquierdo y, luego, seleccione **+** para crear un perfil. Aparece la pantalla **Register a New Provisioning Profile** (Registrar un nuevo perfil de aprovisionamiento).

1. En la ventana **Select certificates** (Seleccionar certificados), seleccione el certificado de desarrollo que acaba de crear. Después, seleccione **Continuar**.

1. Después, seleccione en los dispositivos que va a usar para la prueba y seleccione **Continue** (Continuar).

1. Para terminar, elija un nombre para el perfil en **Provisioning Profile Name** (Nombre del perfil de aprovisionamiento) y seleccione **Generate** (Generar).

    ![Elegir el nombre del perfil de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Cuando se cree el perfil de aprovisionamiento, seleccione **Download** (Descargar). Recuerde la ubicación donde se ha guardado.

1. Vaya a la ubicación del perfil de aprovisionamiento y haga doble clic en él para instalarlo en la máquina de desarrollo.
