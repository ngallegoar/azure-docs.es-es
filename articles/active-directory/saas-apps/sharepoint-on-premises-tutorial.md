---
title: 'Tutorial: Integración de Azure Active Directory con SharePoint local | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SharePoint local.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: jeedes
ms.openlocfilehash: 22b673ef481593247b6ee1007c13390a498c66be
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048630"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharepoint-on-premises"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SharePoint local

En este tutorial, aprenderá a integrar SharePoint local con Azure Active Directory (Azure AD). Al integrar SharePoint local con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SharePoint local.
* Permitir que los usuarios inicien sesión automáticamente en SharePoint local con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único de SharePoint local.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SharePoint local admite el inicio de sesión único iniciado por **SP**.
* Una vez configurado SharePoint local, puede aplicar controles de sesión, que protegen la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).
* Consulte este [vínculo](https://docs.microsoft.com/archive/blogs/kaevans/sharepoint-2013-user-profile-sync-for-claims-users) para obtener información sobre cómo sincronizar perfiles de usuario de SharePoint local con Azure AD.

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Incorporación de SharePoint local desde la galería

Para configurar la integración de SharePoint local en Azure AD, tiene que agregar SharePoint local desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.

    > [!NOTE]
    > Si el elemento no debe estar disponible, también se puede abrir con el vínculo fijo **Todos los servicios** ubicado en la parte superior del panel de navegación de la izquierda. En la información general siguiente, el vínculo **Azure Active Directory** está en la sección **Identidad** o se puede buscar con el cuadro de texto de filtro.

1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **SharePoint local** en el cuadro de búsqueda.
1. Seleccione **SharePoint local** en el panel de resultados y, luego, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sharepoint-on-premises"></a>Configuración y prueba del inicio de sesión único de Azure AD para SharePoint local

Configure y pruebe el inicio de sesión único de Azure AD con SharePoint local mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de SharePoint local.

Para configurar y probar el inicio de sesión único de Azure AD con SharePoint local, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de SharePoint local](#configure-sharepoint-on-premises-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Creación de un grupo de seguridad de Azure AD en Azure Portal](#create-an-azure-ad-security-group-in-the-azure-portal)** : para habilitar un nuevo grupo de seguridad en Azure AD para el inicio de sesión único.
5. **[Concesión de acceso a un grupo de seguridad local de SharePoint](#grant-access-to-sharepoint-on-premises-security-group)** : para conceder acceso a Azure AD a un grupo concreto.
6. **[Asignación del grupo de seguridad de Azure AD en Azure Portal](#assign-the-azure-ad-security-group-in-the-azure-portal)** : para asignar el grupo concreto a Azure AD para la autenticación.
7. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SharePoint local, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **SharePoint local**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:


    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `urn:sharepoint:federation`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointServerURL>/_trust/default.aspx`

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de atención al cliente de SharePoint local](https://support.office.com/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

    > [!Note]
    > Anote la ruta de acceso del archivo en la que ha descargado el archivo de certificado, ya que la usará más tarde para la configuración en el script de PowerShell.

6. En la sección **Set up SharePoint on-premises** (Configurar SharePoint local), copie las direcciones URL que necesite. En **Dirección URL del servicio de inicio de sesión único**, use un valor del siguiente patrón: `https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ es el identificador de inquilino de la suscripción de Azure AD.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    > [!NOTE]
    > La aplicación SharePoint local usa el token SAML 1.1, por lo que Azure AD espera la solicitud de WS Fed del servidor de SharePoint y, después de la autenticación, emite el token. SAML 1.1.

### <a name="configure-sharepoint-on-premises-single-sign-on"></a>Configuración de inicio de sesión único de SharePoint local

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de SharePoint local como administrador.

2. **Configurar un nuevo proveedor de identidades de confianza en SharePoint Server 2016**

    Inicie sesión en el servidor de SharePoint Server 2016 y abra el Shell de administración de SharePoint 2016. Rellene los valores de $realm (valor de identificador de la sección de dominio y direcciones URL locales de SharePoint de Azure Portal), $wsfedurl (dirección URL del servicio de inicio de sesión único) y $filepath (ruta de acceso del archivo en la que ha descargado el archivo de certificado) en Azure Portal y ejecute los siguientes comandos para configurar un nuevo proveedor de identidades de confianza.

    > [!TIP]
    > Si no está familiarizado con PowerShell o quiere obtener más información sobre el funcionamiento de PowerShell, vea [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps).

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $map5 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4,$map5 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Luego siga estos pasos para habilitar al proveedor de identidades de confianza para la aplicación:

    a. En Administración central, vaya a **Administrar aplicación web** y seleccione la aplicación web que quiere proteger con Azure AD.

    b. En la cinta, haga clic en **Proveedores de autenticación** y elija la zona que quiere usar.

    c. Seleccione **Proveedor de identidad de confianza** y luego seleccione el proveedor de identidades que acaba de registrar, denominado *AzureAD*.

    d. En la opción de dirección URL de la página de inicio de sesión, seleccione **Página de inicio de sesión personalizada** y proporcione el valor "/_trust/".

    e. Haga clic en **OK**.

    ![Configuración del proveedor de autenticación](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Algunos de los usuarios externos no podrán usar esta integración de inicio de sesión único dado que su UPN habrá trastocado el valor de esta forma: `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. En breve permitiremos a los clientes configurar la aplicación para administrar el UPM en función del tipo de usuario. Después de eso, todos los usuarios invitados podrán usar el inicio de sesión único sin problemas igual que los empleados de la organización.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Creación de un grupo de seguridad de Azure AD en Azure Portal

1. Haga clic en **Azure Active Directory > Todos los grupos**.

    ![Creación de un grupo de seguridad de Azure AD](./media/sharepoint-on-premises-tutorial/allgroups.png)

2. Haga clic en **Nuevo grupo**:

    ![Creación de un grupo de seguridad de Azure AD](./media/sharepoint-on-premises-tutorial/newgroup.png)

3. Rellene **Tipo de grupo**, **Nombre del grupo**, **Descripción del grupo** y **Tipo de pertenencia**. Haga clic en la flecha para seleccionar los miembros y, a continuación, busque o haga clic en el miembro que desee agregar al grupo. Haga clic en **Seleccionar** para agregar los miembros seleccionados y, a continuación, haga clic en **Crear**.

    ![Creación de un grupo de seguridad de Azure AD](./media/sharepoint-on-premises-tutorial/addingmembers.png)

    > [!NOTE]
    > Para asignar grupos de seguridad de Azure Active Directory a SharePoint local, será necesario instalar y configurar [AzureCP](https://yvand.github.io/AzureCP/) en la granja de SharePoint local O desarrollar y configurar un proveedor de notificaciones personalizado alternativo para SharePoint.  Consulte la sección Más información al final del documento para crear su propio proveedor de notificaciones personalizado, si no usa AzureCP.

### <a name="grant-access-to-sharepoint-on-premises-security-group"></a>Concesión de acceso al grupo de seguridad local de SharePoint

**Configuración de permisos y grupos de seguridad en el registro de aplicaciones**

1. En Azure Portal, seleccione **Azure Active Directory** y, a continuación, **Registros de aplicaciones**.

    ![Hoja Aplicaciones empresariales](./media/sharepoint-on-premises-tutorial/appregistrations.png)

2. En el cuadro de búsqueda, escriba y seleccione **SharePoint local**.

    ![SharePoint local en la lista de resultados](./media/sharepoint-on-premises-tutorial/appsearch.png)

3. Haga clic en **Manifiesto**.

    ![Opción de manifiesto](./media/sharepoint-on-premises-tutorial/manifest.png)

4. Modifique `groupMembershipClaims`: `NULL`, por `groupMembershipClaims`: `SecurityGroup`. A continuación, haga clic en Guardar

    ![Editar manifiesto](./media/sharepoint-on-premises-tutorial/manifestedit.png)

5. Haga clic en **Configuración** y, a continuación, haga clic en **Permisos necesarios**.

    ![Permisos necesarios](./media/sharepoint-on-premises-tutorial/settings.png)

6. Haga clic en **Agregar** y, a continuación, en **Seleccionar una API**.

    ![Acceso de API](./media/sharepoint-on-premises-tutorial/required_permissions.png)

7. Agregue tanto **Active Directory de Microsoft Azure** como **Microsoft Graph API**. Solo es posible, sin embargo, seleccionar una de las dos opciones cada vez.

    ![Selección de API](./media/sharepoint-on-premises-tutorial/permissions.png)

8. Seleccione Active Directory de Microsoft Azure, active Leer datos de directorio y haga clic en Seleccionar. Vuelva atrás y agregue Microsoft Graph y seleccione Leer datos de directorio para ella también.  Haga clic en Seleccionar y en Listo.

    ![Habilitar acceso](./media/sharepoint-on-premises-tutorial/readpermission.png)

9. Ahora, en Configuración requerida, haga clic en **Conceder permisos** y, a continuación, haga clic en Sí para conceder permisos.

    ![Conceder permisos](./media/sharepoint-on-premises-tutorial/grantpermission.png)

    > [!NOTE]
    > Verifique en las notificaciones para determinar si los permisos se concedieron correctamente.  Si no fue así, AzureCP no funcionará correctamente y no será posible configurar SharePoint local con los grupos de seguridad de Azure Active Directory.

10. Configure AzureCP en la granja local de SharePoint o una solución del proveedor de notificaciones personalizada alternativa.  En este ejemplo, se usa AzureCP.

    > [!NOTE]
    > Tenga en cuenta que AzureCP no es un producto de Microsoft ni soporte técnico de Microsoft lo admite. Descargue, instale y configure de AzureCP en la granja de SharePoint local por [https://yvand.github.io/AzureCP/](https://yvand.github.io/AzureCP/) 

11. **Concesión de acceso al grupo de seguridad de Azure Active Directory en SharePoint local**: debe concederse acceso a los grupos a la aplicación en SharePoint en el entorno local.  Use los siguientes pasos para establecer los permisos a fin de obtener acceso a la aplicación web.

12. En Administración central, haga clic en Administración de aplicaciones y en Administrar aplicaciones web y, a continuación, seleccione la aplicación web para activar la cinta y haga clic en Directiva de usuario.

    ![Administración central](./media/sharepoint-on-premises-tutorial/centraladministration.png)

13. En Directiva de aplicación web, haga clic en Agregar usuarios y, a continuación, seleccione la zona y haga clic en Siguiente.  Haga clic en la libreta de direcciones.

    ![Directiva de aplicación web](./media/sharepoint-on-premises-tutorial/webapp-policy.png)

14. A continuación, busque y agregue el grupo de seguridad de Azure Active Directory y haga clic en Aceptar.

    ![Incorporación del grupo de seguridad](./media/sharepoint-on-premises-tutorial/securitygroup.png)

15. Seleccione los permisos y, a continuación, haga clic en Finalizar.

    ![Incorporación del grupo de seguridad](./media/sharepoint-on-premises-tutorial/permissions1.png)

16. Consulte en Directiva de aplicación web. Se agrega el grupo de Azure Active Directory.  En la notificación de grupo se muestra el identificador de objeto del grupo de seguridad de Azure Active Directory para el nombre de usuario.

    ![Incorporación del grupo de seguridad](./media/sharepoint-on-premises-tutorial/addgroup.png)

17. Vaya a la colección de sitios de SharePoint y agregue el grupo allí también. Haga clic en Configuración del sitio y, a continuación, haga clic en Permisos del sitio y Conceder permisos.  Busque la notificación de rol del grupo, asigne el nivel de permisos y haga clic en Compartir.

    ![Incorporación del grupo de seguridad](./media/sharepoint-on-premises-tutorial/grantpermission1.png)

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Configuración de un proveedor de identidades de confianza para varias aplicaciones web

La configuración funciona para una sola aplicación web, pero se necesitará configuración adicional si va a usar el mismo proveedor de identidades de confianza con varias aplicaciones web. Por ejemplo, suponga que se hubiese ampliado una aplicación web para usar la dirección URL `https://portal.contoso.local` y ahora también quiere autenticar a los usuarios en `https://sales.contoso.local`. Para ello, se deberá actualizar el proveedor de identidades para respetar el parámetro WReply y actualizar el registro de aplicación en Azure AD para agregar una dirección URL de respuesta.

1. En Azure Portal, abra el directorio de Azure AD. Haga clic en **Registros de aplicaciones** y, luego, haga clic en **Ver todas las aplicaciones**. Haga clic en la aplicación que creó anteriormente (integración de SAML de SharePoint).

2. Haga clic en **Configuración**.

3. En la hoja de configuración, haga clic en **URL de respuesta**.

4. Agregue la dirección URL de la aplicación web adicional con `/_trust/default.aspx` anexado a la dirección URL (por ejemplo, `https://sales.contoso.local/_trust/default.aspx`) y haga clic en **Guardar**.

5. En el servidor de SharePoint, abra la **consola de administración de SharePoint 2016** y ejecute los comandos siguientes, utilizando el nombre del emisor de tokens de identidad de confianza que usó anteriormente.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```

6. En Administración Central, vaya a la aplicación web y habilite el proveedor de identidades de confianza existente. No olvide configurar también la dirección URL de la página de inicio de sesión como una página de inicio de sesión personalizada `/_trust/`.

7. En Administración Central, haga clic en la aplicación web y elija **Directiva de usuario**. Agregue un usuario con los permisos adecuados, como se demostró anteriormente en este artículo.

### <a name="fixing-people-picker"></a>Corrección del selector de personas

Los usuarios ahora pueden iniciar sesión en SharePoint 2016 mediante identidades de Azure AD, pero todavía hay oportunidades de mejora de la experiencia del usuario. Por ejemplo, la búsqueda de un usuario da lugar a varios resultados de búsqueda en el selector de personas. Hay un resultado de búsqueda por cada uno de los tres tipos de notificación creados en la asignación de notificación. Para elegir un usuario mediante el selector de personas, debe escribir su nombre de usuario exactamente y elegir el resultado de notificación **Nombre**.

![Resultados de búsqueda de notificaciones](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

No hay ninguna validación de los valores que se buscan, lo cual puede dar lugar a errores ortográficos o a que los usuarios elijan accidentalmente el tipo de notificación incorrecto para asignar, como la notificación **Apellido**. Esto puede evitar que los usuarios accedan correctamente a los recursos.

Para ayudar en este caso, hay una solución de código abierto denominada [AzureCP](https://yvand.github.io/AzureCP/) que proporciona un proveedor de notificaciones personalizado para SharePoint 2016. Usa Microsoft Graph API para solucionar errores en lo que escriben los usuarios y realizar la validación. Más información en [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-security-group-in-the-azure-portal"></a>Asignación del grupo de seguridad de Azure AD en Azure Portal

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, seleccione **SharePoint local**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **SharePoint local**.

    ![Vínculo de SharePoint local en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en **Agregar usuario**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. Busque el grupo de seguridad que desea usar y, a continuación, haga clic en el grupo para agregarlo a la sección Seleccionar miembros. Haga clic en **Seleccionar** y después en **Asignar**.

    ![Búsqueda del grupo de seguridad](./media/sharepoint-on-premises-tutorial/securitygroup1.png)

    > [!NOTE]
    > Compruebe las notificaciones en la barra de menús a fin de que se le informe cuando el grupo se asigne correctamente a la aplicación empresarial en Azure Portal.

### <a name="create-sharepoint-on-premises-test-user"></a>Creación de un usuario de prueba de SharePoint local

En esta sección, creará un usuario llamado Britta Simon en SharePoint local. Trabaje con el [equipo de soporte técnico de SharePoint local](https://support.office.com/) para agregar los usuarios a la plataforma de SharePoint local. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SharePoint local en el panel de acceso, debería iniciar sesión automáticamente en la versión de SharePoint local para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)