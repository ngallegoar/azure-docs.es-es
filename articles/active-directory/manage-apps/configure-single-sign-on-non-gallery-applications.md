---
title: 'Inicio de sesión único de SAML en aplicaciones que no están en la galería: plataforma de identidad de Microsoft | Microsoft Docs'
description: Configuración del inicio de sesión único (SSO) en aplicaciones que no estén en la galería en la plataforma de identidad de Microsoft (Azure AD)
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 06/08/2020
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3cee2b9a0ea32a3b331849263c8a97f55930542d
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86024238"
---
# <a name="configure-saml-based-single-sign-on-to-non-gallery-applications"></a>Configuración del inicio de sesión único basado en SAML en aplicaciones que no están en la galería

Al [agregar una aplicación de la galería](add-gallery-app.md) o una [aplicación web que no está en la galería](add-non-gallery-app.md) a sus aplicaciones empresariales de Azure AD, una de las opciones de inicio de sesión único disponibles es el [inicio de sesión único basado en SAML](what-is-single-sign-on.md#saml-sso). Elija SAML siempre que sea posible para las aplicaciones que realizan la autenticación mediante uno de los protocolos SAML. Con el inicio de sesión único de SAML, Azure AD se autentica en la aplicación mediante el uso de la cuenta de Azure AD del usuario. Azure AD comunica la información de inicio de sesión a la aplicación a través de un protocolo de conexión. Puede asignar los usuarios a roles de aplicación específicos según las reglas que defina en las notificaciones de SAML. En este artículo se describe cómo configurar el inicio de sesión único basado en SAML para una aplicación que no es de la galería. 

> [!NOTE]
> ¿Desea agregar una aplicación de galería? Consulte las instrucciones de configuración paso a paso en la [lista de tutoriales de aplicaciones SaaS](../saas-apps/tutorial-list.md)

Para configurar un inicio de sesión único de SAML para una aplicación que no esté en la galería sin escribir código, debe tener una suscripción a Azure AD y la aplicación debe ser compatible con SAML 2.0. Para más información acerca de las versiones de Azure AD, visite [Precios de Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="before-you-begin"></a>Antes de empezar

Si la aplicación no se ha agregado a su inquilino de Azure AD, consulte [Incorporación de una aplicación que no es de la galería](add-non-gallery-app.md).

## <a name="step-1-edit-the-basic-saml-configuration"></a>Paso 1. Edición de la configuración básica de SAML

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de aplicaciones en la nube o administrador de aplicaciones para el inquilino de Azure AD.

2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales** y seleccione la aplicación en la lista. 
   
   - Para buscar la aplicación, en el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y, después, **Aplicar**. Escriba el nombre de la aplicación en el cuadro de búsqueda y seleccione la aplicación en los resultados.

3. En la sección **Administrar**, seleccione **Inicio de sesión único**. 

   - Tenga en cuenta que hay algunos escenarios en los que no estará presente la opción de **Inicio de sesión único**. Por ejemplo, si la aplicación se registró mediante **Registros de aplicaciones**, la capacidad de inicio de sesión único está configurada para usar OAuth de OIDC de forma predeterminada. En este caso, la opción de **Inicio de sesión único** no se mostrará en la navegación en **Aplicaciones empresariales**. Cuando use **Registros de aplicaciones** para agregar su aplicación personalizada, configure las opciones en el archivo de manifiesto. Para más información acerca del archivo de manifiesto, consulte (https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest). Para más información acerca de los estándares de SSO, consulte (https://docs.microsoft.com/azure/active-directory/develop/authentication-vs-authorization#authentication-and-authorization-using-microsoft-identity-platform). Otros escenarios en los que falta **inicio de sesión único** en la navegación incluyen cuándo se hospeda una aplicación en otro inquilino o si su cuenta no tiene los permisos necesarios (Administrador global, Administrador de aplicaciones en la nube, Administrador de la aplicación o propietario de la entidad de servicio). Los permisos también pueden provocar un escenario en el que puede abrir **Inicio de sesión único** pero no podrá guardar. Para más información acerca de los roles administrativos de Azure AD, consulte (https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

4. Seleccione **SAML**. Se muestra la página **Configurar el inicio de sesión único con SAML (versión preliminar)** .

   ![Paso 1: Edición de la configuración básica de SAML](media/configure-single-sign-on-non-gallery-applications/step-one-basic-saml-config.png)

5. Para editar las opciones de configuración básicas de SAML, seleccione el icono **Editar** (un lápiz) en la esquina superior derecha de la sección **Configuración básica de SAML**.

1. Escriba la siguiente configuración. El proveedor de la aplicación le proporcionará estos valores. Puede especificar los valores manualmente o cargar un archivo de metadatos para extraer el valor de los campos.

    | Opción de configuración básica de SAML | Iniciado por el proveedor de servicios | Iniciado por IdP | Descripción |
    |:--|:--|:--|:--|
    | **Identificador (identificador de entidad)** | Obligatorio para algunas aplicaciones | Obligatorio para algunas aplicaciones | Identifica de forma única la aplicación. Azure AD envía el identificador a la aplicación como el parámetro Audiencia del token SAML. Se espera que la aplicación lo valide. Este valor también aparece como el id. de entidad en los metadatos SAML proporcionados por la aplicación. Escriba una dirección URL que use el siguiente modelo: "https://<subdomain>.contoso.com" *Puede encontrar este valor como el elemento **Issuer** (Emisor) en el elemento **AuthnRequest** (solicitud SAML) enviado por la aplicación.* |
    | **URL de respuesta** | Obligatorio | Obligatorio | Especifica el lugar donde la aplicación espera recibir el token SAML. La dirección URL de respuesta también se conoce como dirección URL del Servicio de consumidor de aserciones (ACS). Puede usar los campos adicionales de URL de respuesta para especificar varias direcciones URL de respuesta. Por ejemplo, puede que necesite direcciones URL de respuesta adicionales para varios subdominios. O bien, con fines de prueba, puede especificar varias direcciones URL de respuesta (host local y direcciones URL públicas) al mismo tiempo. |
    | **URL de inicio de sesión** | Obligatorio | No especificar | Cuando un usuario abre esta dirección URL, el proveedor de servicios lo redirige a Azure AD para autenticar el usuario e iniciar sesión. Azure AD usa la dirección URL para iniciar la aplicación desde el Panel de acceso de Azure AD u Office 365. Si está en blanco, Azure AD realiza un inicio de sesión iniciado por IdP cuando el usuario inicia la aplicación desde Office 365, el Panel de acceso de Azure AD o la dirección URL de inicio de sesión único de Azure AD.|
    | **Estado de la retransmisión** | Opcional | Opcional | Especifica a la aplicación a dónde debe redirigir al usuario una vez completada la autenticación. Normalmente, el valor es una dirección URL válida para la aplicación. Sin embargo, algunas aplicaciones usan este campo de forma diferente. Para más información, pregunte al proveedor de la aplicación.
    | **Dirección URL de cierre de sesión** | Opcional | Opcional | Se usa para devolver las respuestas de cierre de sesión SAML a la aplicación.

Para más información, consulte [Protocolo SAML de inicio de sesión único](../develop/single-sign-on-saml-protocol.md).

## <a name="step-2-configure-user-attributes-and-claims"></a>Paso 2. Configuración de atributos y notificaciones de usuario 

Cuando un usuario se autentique en la aplicación, Azure AD emite un token SAML a la aplicación con información (o notificaciones) sobre el usuario que lo identifica de forma única. De forma predeterminada, dicha información incluye el nombre de usuario, la dirección de correo electrónico, el nombre y los apellidos del usuario. Es posible que tenga que personalizar estas notificaciones si, por ejemplo, la aplicación requiere valores de notificación específicos o un formato de **nombre** distinto del nombre de usuario. Los requisitos para las aplicaciones de la galería se describen en los [tutoriales específicos de cada aplicación](../saas-apps/tutorial-list.md), o puede solicitar la información al proveedor de la aplicación. A continuación se describen los pasos generales para configurar los atributos y las notificaciones de usuario.

1. En la esquina superior derecha de la sección **Atributos y notificaciones de usuario**, seleccione el icono **Editar** (un lápiz).

   ![Paso 2: Configuración de atributos y notificaciones de usuario](media/configure-single-sign-on-non-gallery-applications/step-two-user-attributes-claims.png)

2. Compruebe el **Valor de identificador de nombre**. El valor predeterminado es *user.principalname*. El identificador de usuario permite identificar de manera exclusiva a cada usuario dentro de la aplicación. Por ejemplo, si la dirección de correo electrónico es a la vez el nombre de usuario y el identificador único, establezca el valor en *user.mail*.

3. Para modificar el **valor de identificador de nombre**, seleccione el icono **Editar** (un lápiz) del campo **Valor de identificador de nombre**. Realice los cambios que considere necesarios en el formato y el origen del identificador. Para más información, consulte [Edición de NameId](../develop/active-directory-saml-claims-customization.md#editing-nameid). Al acabar, guarde los cambios. 
 
4. Para configurar notificaciones de grupo, seleccione el icono **Editar** para el campo **Grupos devueltos en la notificación**. Para más información, consulte [Configuración de notificaciones de grupos](../hybrid/how-to-connect-fed-group-claims.md).

5. Para agregar una notificación, seleccione **Agregar nueva notificación** en la parte superior de la página. Escriba el **nombre** y seleccione el origen adecuado. Si selecciona el origen **Atributo**, deberá elegir el **Atributo de origen** que quiere usar. Si selecciona el origen **Traducción**, deberá elegir los valores de **Transformación** y **Parámetro 1** que quiere usar. Para más información consulte [Incorporación de notificaciones específicas de la aplicación](../develop/active-directory-saml-claims-customization.md#adding-application-specific-claims). Al acabar, guarde los cambios. 

6. Seleccione **Guardar**. La nueva notificación aparece en la tabla.

   > [!NOTE]
   > Para más formas de personalizar el token SAML desde Azure AD a la aplicación, consulte los siguientes recursos.
   >- Para crear roles personalizados a través de Azure Portal, consulte [Configuración de notificaciones de rol](../develop/active-directory-enterprise-app-role-management.md).
   >- Para personalizar las notificaciones a través de PowerShell, consulte [Personalización de notificaciones: PowerShell](../develop/active-directory-claims-mapping.md).
   >- Para modificar el manifiesto de aplicación para configurar notificaciones opcionales, consulte [Configuración de notificaciones opcionales](../develop/active-directory-optional-claims.md).
   >- Para establecer directivas de vigencia de los tokens de actualización, tokens de acceso, tokens de sesión y tokens de identificador consulte [Configuración de la vigencia de los tokens](../develop/active-directory-configurable-token-lifetimes.md). O bien, para restringir las sesiones de autenticación a través del acceso condicional de Azure AD, consulte las [funcionalidades de administración de sesiones de autenticación](https://go.microsoft.com/fwlink/?linkid=2083106).

## <a name="step-3-manage-the-saml-signing-certificate"></a>Paso 3. Administración del certificado de firma SAML

Azure AD usa un certificado para firmar los tokens SAML que envía a la aplicación. Necesita este certificado para establecer la confianza entre Azure AD y la aplicación. Para obtener detalles sobre el formato del certificado, consulte la documentación de SAML de la aplicación. Para más información, consulte [Administración de certificados para el inicio de sesión único federado](manage-certificates-for-federated-single-sign-on.md) y [Opciones avanzadas de firma de certificados en el token SAML](certificate-signing-options.md).

Desde Azure AD, puede descargar el certificado activo en formato Base64 o Raw directamente desde la página principal **Configurar el inicio de sesión único con SAML**. Además, puede obtener el certificado activo al descargar el archivo XML de metadatos de la aplicación o mediante el uso de la URL de metadatos de la federación de aplicaciones. Para ver, crear o descargar certificados (activos o inactivos), siga estos pasos.

1. Vaya a la sección **Certificado de firma de SAML**. 

   ![Paso 3: Administración del certificado de firma SAML](./media/configure-single-sign-on-non-gallery-applications/step-three-certificate.png)

2. Compruebe que el certificado tiene:

   - *La fecha de expiración deseada.* Puede configurar la fecha de expiración hasta tres años en el futuro.
   - *Un estado de activo para el certificado que desee.* Si el estado es **Inactivo**, cambie el estado a **Activo**. Para cambiar el estado, haga clic con el botón derecho en la fila del certificado deseado y seleccione **Activar el certificado**.
   - *La opción de firma y el algoritmo correctos.*
   - *Los correos electrónicos de notificación correctos.* Cuando el certificado activo esté cerca de la fecha de expiración, Azure AD envía una notificación a la dirección de correo electrónico configurada en este campo.

2. Para descargar el certificado, seleccione una de las opciones de formato Base64, formato sin procesar o XML de metadatos de federación. Azure AD también proporciona la **dirección URL de metadatos de federación de la aplicación**, en donde puede acceder a los `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`metadatos específicos de la aplicación en el formato.

3. Para administrar, crear o importar un certificado, seleccione el icono de **edición** (un lápiz) en la esquina superior derecha de la sección **Certificado de firma de SAML**.

   ![Certificado de firma de SAML](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)


   Para ello, realice cualquiera de las siguientes acciones:

   - Para crear un certificado nuevo, seleccione **Nuevo certificado**, seleccione la **Fecha de expiración** y, a continuación, seleccione **Guardar**. Para activar el certificado, seleccione el menú contextual ( **...** ) y seleccione **Activar el certificado**.
   - Para cargar un certificado con una clave privada y credenciales pfx, seleccione **Importar certificado** y búsquelo. Escriba la **Contraseña PFX**, y seleccione **Agregar**.  
   - Para configurar las opciones avanzadas de firma de certificados, utilice las opciones siguientes. Para ver las descripciones de estas opciones consulte el artículo [Opciones avanzadas de firma de certificado](certificate-signing-options.md).
      - En la lista desplegable **Opción de firma**, seleccione **Firmar respuesta SAML**, **Firmar aserción SAML** o **Firmar respuesta y aserción SAML**.
      - En la lista desplegable **Algoritmo de firma**, elija **SHA-1** o **SHA-256**.
   - Para notificar a otras personas cuando el certificado activo esté cerca de su fecha de expiración, escriba las direcciones de correo electrónico en los campos **Direcciones de correo electrónico de notificación**.

4. Si realizó cambios, seleccione **Guardar** en la parte superior de **Certificado de firma de SAML**. 

## <a name="step-4-set-up-the-application-to-use-azure-ad"></a>Paso 4. Configuración de la aplicación para que use Azure AD

En la sección **Configurar \<applicationName>** se muestran los valores que deben configurarse en la aplicación para que usen Azure AD como proveedor de identidades de SAML. Los valores necesarios varían dependiendo de la aplicación. Para más información, consulte la documentación de SAML de la aplicación. Para encontrar la documentación, vaya al encabezado **Configurar \<application name>** y seleccione **Ver instrucciones paso a paso**. La documentación aparece en la página **Configurar inicio de sesión**. Esta página le guiará para rellenar los valores de la **Dirección URL de inicio de sesión**, **Identificador de Azure AD** y **URL de cierre de sesión** en el encabezado **Configurar\<application name>** .

1. Desplácese hacia abajo hasta la sección **Configurar\<applicationName>** . 
   
   ![Paso 4: Configuración de la aplicación](media/configure-single-sign-on-non-gallery-applications/step-four-app-config.png)

1. Copie el valor de cada fila en esta sección según sea necesario, y siga las instrucciones específicas de la aplicación para agregar el valor a la aplicación. En el caso de las aplicaciones de la galería, puede ver la documentación seleccionando **Ver instrucciones detalladas**. 
   - Los valores **Dirección URL de inicio de sesión** y **URL de cierre de sesión** se resuelven en el mismo punto de conexión, que es el punto de conexión de control de solicitudes SAML de su instancia de Azure AD. 
   - **Identificador de Azure AD** es el valor del **Emisor** en el token SAML emitido a la aplicación.
2. Cuando haya pegado todos los valores en los campos correspondientes, seleccione **Guardar**.

## <a name="step-5-validate-single-sign-on"></a>Paso 5. Validación del inicio de sesión único

Una vez que haya configurado la aplicación para que use Azure AD como proveedor de identidades basado en SAML, puede probar la configuración para ver si el inicio de sesión único funciona para su cuenta. 

2. Desplácese hasta la sección **Validate single sign-on with <applicationName>** (Validar el inicio de sesión único con <applicationName>).

   ![Paso 5: Validación del inicio de sesión único](media/configure-single-sign-on-non-gallery-applications/step-five-validate.png)

3. Seleccione **Validar**. Aparecen las opciones de pruebas.

4. Seleccione **Iniciar sesión en nombre del usuario actual**. 

Si el inicio de sesión se realiza correctamente, está listo para asignar usuarios y grupos a la aplicación SAML.
Si aparece un mensaje de error, realice los pasos siguientes:

1. Copie y pegue los detalles en el cuadro **What does the error look like?** (¿Qué aspecto tiene el error?).

    ![Obtención de instrucciones para la resolución](media/configure-single-sign-on-non-gallery-applications/error-guidance.png)

2. Seleccione **Obtenga instrucciones para la resolución**. Se muestran la causa principal e instrucciones para la resolución.  En este ejemplo, el usuario no se asignó a la aplicación.

3. Lea las instrucciones de resolución y, después, si es posible, solucione el problema.

4. Vuelva a ejecutar la prueba hasta que se complete correctamente.

Para más información, consulte [Depuración del inicio de sesión único basado en SAML en aplicaciones de Azure Active Directory](../azuread-dev/howto-v1-debug-saml-sso-issues.md).

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de usuarios y grupos a la aplicación](methods-for-assigning-users-and-groups.md)
- [Configuración del aprovisionamiento automático de cuentas de usuario](../app-provisioning/configure-automatic-user-provisioning-portal.md)
