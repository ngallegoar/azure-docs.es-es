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
ms.date: 01/15/2020
ms.author: miguego
ms.openlocfilehash: 7e47891a74feb60b0f3e4594bd1621e8b62d64d1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867107"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharepoint-on-premises"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SharePoint local

En este tutorial, aprenderá a integrar SharePoint local con Azure Active Directory (Azure AD). Al integrar SharePoint local con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SharePoint local.
* Permitir que los usuarios inicien sesión automáticamente en SharePoint local con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure Active Directory con SharePoint local, necesita los siguientes elementos:

* Una suscripción a Azure Active Directory. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una granja de servidores de SharePoint 2013, o posterior.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure Active Directory en un entorno de prueba. Los usuarios de Azure Active Directory podrán acceder a su instancia de SharePoint local.

## <a name="create-the-enterprise-applications-in-azure-portal"></a>Creación de las aplicaciones empresariales en Azure Portal

Para configurar la integración de SharePoint local en Azure AD, tiene que agregar SharePoint local desde la galería a la lista de aplicaciones SaaS administradas.

Para agregar SharePoint local desde la galería, realice los pasos siguientes:

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

 > [!NOTE]
 > Si el elemento no debe estar disponible, también se puede abrir con el vínculo fijo **Todos los servicios** ubicado en la parte superior del panel de navegación de la izquierda. En la información general siguiente, el vínculo **Azure Active Directory** está en la sección **Identidad** o se puede buscar con el cuadro de texto de filtro.

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

4. En el cuadro de búsqueda, escriba **SharePoint local** y seleccione **SharePoint local** en el panel de resultados.

    <kbd>![SharePoint local en la lista de resultados](./media/sharepoint-on-premises-tutorial/search-new-app.png)</kbd>

1. Especifique un nombre para la instancia de SharePoint local y haga clic en el botón **Agregar** para agregar la aplicación.

1. En la nueva aplicación empresarial, haga clic en Propiedades y compruebe el valor de **Asignación de usuario necesaria**.

   <kbd>![SharePoint local en la lista de resultados](./media/sharepoint-on-premises-tutorial/user-assignment-required.png)</kbd>

En nuestro escenario, este valor está establecido en **No**.

## <a name="configure-and-test-azure-ad"></a>Configuración y prueba del inicio de sesión único de Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con SharePoint local.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SharePoint local.

Para configurar y probar el inicio de sesión único de Azure Active Directory con SharePoint local, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
1. **[Configuración de SharePoint local](#configure-sharepoint-on-premises)** : para configurar los valores de inicio de sesión único en la aplicación.
1. **[Creación de un usuario de prueba de Azure AD en Azure Portal](#create-an-azure-ad-test-user-in-the-azure-portal)** : para crear un nuevo usuario en Azure AD para el inicio de sesión único.
1. **[Creación de un grupo de seguridad de Azure AD en Azure Portal](#create-an-azure-ad-security-group-in-the-azure-portal)** : para crear un nuevo grupo de seguridad en Azure AD para el inicio de sesión único.
1. **[Concesión de permisos a la cuenta de Azure Active Directory en SharePoint local](#grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises)** : para conceder permisos al usuario de Azure AD.
1. **[Concesión de permisos al grupo de Azure AD en SharePoint local](#grant-permissions-to-azure-ad-group-in-sharepoint-on-premises)** : para conceder permisos al grupo de Azure AD.
1. **[Concesión de acceso a una cuenta de invitado a SharePoint local en Azure Portal](#grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal)** : para conceder permisos a la cuenta de invitado en Azure AD para SharePoint local.
1. **[Configuración del proveedor de identidades de confianza para varias aplicaciones web](#configuring-the-trusted-identity-provider-for-multiple-web-applications)** : cómo usar el mismo proveedor de identidades de confianza para varias aplicaciones web.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con SharePoint local, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), abra el directorio de Azure AD, haga clic en **Aplicaciones empresariales**, haga clic en el **nombre de aplicación empresarial creado anteriormente** y haga clic en **Inicio de sesión único**.

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, haga clic en el modo **SAML** para habilitar el inicio de sesión único.
 
3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de inicio de sesión único de Dominio y direcciones URL de SharePoint local](./media/sharepoint-on-premises-tutorial/sp-identifier-reply.png)

    1. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `urn:<sharepointFarmName>:<federationName>`

    1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointSiteURL>/_trust/`

    1. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YourSharePointSiteURL>/`
    1. Haga clic en Guardar.

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](./media/sharepoint-on-premises-tutorial/certificatebase64.png)

6. En la sección **Set up SharePoint on-premises** (Configurar SharePoint local), copie las direcciones URL que necesite.
    
    1. **Dirección URL de inicio de sesión**
    
        Copie la dirección URL de inicio de sesión y reemplace **/saml2** al final por **/wsfed**, aparecerá algo similar a **https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed** (esta dirección URL no es exacta).

    1. **Identificador de Azure AD**
    1. **Dirección URL de cierre de sesión**
    > [!NOTE]
    > Esta dirección URL no se puede usar como en SharePoint: debe reemplazar **/saml2** por **/wsfed**. La aplicación SharePoint local usa el token SAML 1.1, por lo que Azure AD espera la solicitud de WS Fed del servidor de SharePoint y, después de la autenticación, emite el token. SAML 1.1.

### <a name="configure-sharepoint-on-premises"></a>Configuración de SharePoint local

1. **Creación de un nuevo proveedor de identidades de confianza en SharePoint Server 2016**

    Inicie sesión en el servidor de SharePoint y abra el shell de administración de SharePoint. Rellene los valores:
    1. **$realm** (valor del identificador de la sección Dominio y direcciones URL de SharePoint local en Azure Portal).
    1. **$wsfedurl** (Dirección URL del servicio de inicio de sesión único).
   1. **$filepath** (ruta de acceso en la que ha descargado el archivo de certificado desde Azure Portal).

    Ejecute los siguientes comandos para configurar un nuevo proveedor de identidades de confianza.

    > [!TIP]
    > Si no está familiarizado con PowerShell o quiere obtener más información sobre el funcionamiento de PowerShell, vea [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps).


    ```
    $realm = "urn:sharepoint:sps201x"
    $wsfedurl="https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed"
    $filepath="C:\temp\SharePoint 2019 OnPrem.cer"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map1 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "Azure AD SharePoint server 201x" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map1,$map2 -SignInUrl $wsfedurl -IdentifierClaim $map1.InputClaimType
    ```
1. **Habilitación del proveedor de identidades de confianza para la aplicación**

    a. En Administración central, vaya a **Administrar aplicación web** y seleccione la aplicación web que quiere proteger con Azure AD.

    b. En la cinta, haga clic en **Proveedores de autenticación** y elija la zona que quiere usar.

    c. Seleccione **Proveedor de identidad de confianza** y luego seleccione el proveedor de identidades que acaba de registrar, denominado *AzureAD*.

    d. Haga clic en **OK**.

    ![Configuración del proveedor de autenticación](./media/sharepoint-on-premises-tutorial/config-auth-provider.png)

### <a name="create-an-azure-ad-test-user-in-the-azure-portal"></a>Creación de un usuario de prueba de Azure AD en Azure Portal

El objetivo de esta sección es crear un usuario de prueba en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** y, en el panel **Administrar**, seleccione **Usuarios**.

2. A continuación, seleccione **Todos los usuarios** seguido de **Nuevo usuario** en la parte superior de la pantalla.

3. Seleccione la opción **Crear usuario** y, en las propiedades del usuario, realice los pasos siguientes.  
   Debería poder crear usuarios en Azure AD mediante el uso del sufijo de inquilino o cualquier dominio comprobado. 

    a. En el campo **Nombre**, escriba el nombre de usuario; aquí usaremos **TestUser**.
  
    b. En el campo **Nombre de usuario**, escriba `TestUser@yourcompanydomain.extension`.  
    Por ejemplo: TestUser@contoso.com

    ![Cuadro de diálogo Usuario](./media/sharepoint-on-premises-tutorial/user-properties.png)

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

    e. Ahora puede compartir el sitio con TestUser@contoso.com y permitir que este usuario tenga acceso a él.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Creación de un grupo de seguridad de Azure AD en Azure Portal

1. Haga clic en **Azure Active Directory > Grupos**.

2. Haga clic en **Nuevo grupo**:

3. Rellene **Tipo de grupo**, **Nombre del grupo**, **Descripción del grupo** y **Tipo de pertenencia**. Haga clic en la flecha para seleccionar los miembros y, a continuación, busque o haga clic en el miembro que desee agregar al grupo. Haga clic en **Seleccionar** para agregar los miembros seleccionados y, a continuación, haga clic en **Crear**.

![Creación de un grupo de seguridad de Azure AD](./media/sharepoint-on-premises-tutorial/new-group.png)

### <a name="grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises"></a>Concesión de permisos a la cuenta de Azure Active Directory en SharePoint local

Para conceder acceso al usuario de Azure Active Directory en el sitio de SharePoint local, debe compartir la colección de sitios o agregar el usuario de Azure Active Directory a uno de los grupos de la colección de sitios. Los usuarios ahora pueden iniciar sesión en SharePoint 201x con identidades de Azure AD, pero hay más oportunidades para mejorar la experiencia del usuario. Por ejemplo, la búsqueda de un usuario da lugar a varios resultados de búsqueda en el selector de personas. Hay un resultado de búsqueda por cada uno de los tipos de notificación creados en la asignación de notificaciones. Para elegir un usuario mediante el selector de personas, debe escribir su nombre de usuario exactamente y elegir el resultado de notificación **Nombre**.

![Resultados de búsqueda de notificaciones](./media/sharepoint-on-premises-tutorial/claims-search-results.png)

No hay ninguna validación de los valores que se buscan, lo cual puede dar lugar a errores ortográficos o a que los usuarios elijan accidentalmente el tipo de notificación incorrecto. Esto puede evitar que los usuarios accedan correctamente a los recursos.

**Para corregir el selector de personas** en este escenario, hay una solución de código abierto llamada [AzureCP](https://yvand.github.io/AzureCP/) que proporciona un proveedor de notificaciones personalizado para SharePoint 2013, 2016 y 2019. Usa Microsoft Graph API para solucionar errores en lo que escriben los usuarios y realizar la validación. Más información en [AzureCP](https://yvand.github.io/AzureCP/).

  > [!NOTE]
  > Sin AzureCP, puede agregar grupos mediante la adición del identificador del grupo de Azure AD, pero esto no es fácil ni confiable para el usuario. El aspecto es el siguiente:  
  >   
  >![Adición de un grupo de Azure AD a un grupo de SharePoint](./media/sharepoint-on-premises-tutorial/adding-group-by-id.png)
  
### <a name="grant-permissions-to-azure-ad-group-in-sharepoint-on-premises"></a>Concesión de permisos al grupo de Azure AD en SharePoint local

Para asignar grupos de seguridad de Azure Active Directory a SharePoint local, será necesario usar un proveedor de notificaciones personalizado para el servidor de SharePoint. En nuestro ejemplo, hemos utilizado AzureCP.

 > [!NOTE]
 > Tenga en cuenta que AzureCP no es un producto de Microsoft ni soporte técnico de Microsoft lo admite. Descargue, instale y configure AzureCP en la granja de SharePoint local desde https://yvand.github.io/AzureCP/. 

1. Configure AzureCP en la granja local de SharePoint o una solución del proveedor de notificaciones personalizada alternativa. Los pasos de configuración de AzureCP están disponibles en https://yvand.github.io/AzureCP/Register-App-In-AAD.html.

1. En Azure Portal, abra el directorio de Azure AD. Haga clic en **Aplicaciones empresariales**, haga clic en el **nombre de aplicación empresarial creado anteriormente** y haga clic en **Inicio de sesión único**.

1. En la página **Configurar el inicio de sesión único con SAML**, edite la sección **Atributos y notificaciones de usuario**.

1. Haga clic en **Agregar una notificación de grupo**.

1. Seleccione los grupos asociados al usuario que se deben devolver en la notificación; en nuestro caso, seleccione **Todos los grupos** y, en la sección de atributos de origen, seleccione **Identificador de grupo** y haga clic en **Guardar**.

Para conceder acceso al grupo de seguridad de Azure Active Directory al sitio de SharePoint local, debe compartir la colección de sitios o agregar el grupo de seguridad de Azure Active Directory a uno de los grupos de la colección de sitios.

1. Vaya a la colección de sitios de SharePoint y, en la opción Configuración del sitio de la colección de sitios, haga clic en "Personas y grupos". Seleccione el grupo de SharePoint y, a continuación, haga clic en Nuevo y en "Agregar usuarios a este grupo", empiece a escribir el nombre del grupo y el selector de personas mostrará el grupo de seguridad de Azure Active Directory.

    ![Adición de un grupo de Azure AD a un grupo de SharePoint](./media/sharepoint-on-premises-tutorial/permission-azure-ad-group.png)

### <a name="grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal"></a>Concesión de acceso a una cuenta de invitado a SharePoint local en Azure Portal

Ahora es posible conceder acceso al sitio de SharePoint a una cuenta de invitado de una manera coherente. Lo que ocurre es que se modifica el UPN. Un usuario con el nombre jdoe@outlook.com se representará como `jdoe_outlook.com#ext#@TENANT.onmicrosoft.com`. Es posible tener una experiencia sin problemas al compartir su sitio con usuarios externos, para lo que sería necesario realizar algunas modificaciones en la sección **Atributos y notificaciones de usuario** de Azure Portal.

1. En Azure Portal, abra el directorio de Azure AD. Haga clic en **Aplicaciones empresariales**, haga clic en el **nombre de aplicación empresarial creado anteriormente** y haga clic en **Inicio de sesión único**.

1. En la página **Configurar el inicio de sesión único con SAML**, edite la sección **Atributos y notificaciones de usuario**.

1. En la zona **Notificaciones necesarias**, haga clic en **Identificador de usuario único (Identificador de nombre)** .

1. Cambie la propiedad **Atributo de origen** con el valor **user.localuserprincipalname** y seleccione **Guardar**.

    ![Estado inicial de Atributos y notificaciones de usuario](./media/sharepoint-on-premises-tutorial/manage-claim.png)

1. Mediante la cinta de opciones, vuelva a **Inicio de sesión basado en SAML**; ahora la sección **Atributos y notificaciones de usuario** tendría el siguiente aspecto: 

    ![Estado final de Atributos y notificaciones de usuario](./media/sharepoint-on-premises-tutorial/user-attributes-claims-final.png)  

    > [!NOTE]
    > No se necesitan los apellidos ni el nombre propio en esta configuración.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** y, a continuación, seleccione **Usuarios**.

1. Haga clic en **Nuevo usuario invitado**.

1. Seleccione la opción **Invitar usuario**, rellene las propiedades del usuario y haga clic en **Invitar**.

1. Ahora puede compartir el sitio con MyGuestAccount@outlook.com y permitir que este usuario tenga acceso a él.

    ![Compartición de un sitio con una cuenta de invitado](./media/sharepoint-on-premises-tutorial/sharing-guest-account.png)

### <a name="configuring-the-trusted-identity-provider-for-multiple-web-applications"></a>Configuración de un proveedor de identidades de confianza para varias aplicaciones web

La configuración funciona para una sola aplicación web, pero se necesitará configuración adicional si va a usar el mismo proveedor de identidades de confianza con varias aplicaciones web. Por ejemplo, suponga que se hubiese ampliado una aplicación web para usar la dirección URL `https://sales.contoso.com` y ahora también quiere autenticar a los usuarios en `https://marketing.contoso.com`. Para ello, se deberá actualizar el proveedor de identidades para respetar el parámetro WReply y actualizar el registro de aplicación en Azure AD para agregar una dirección URL de respuesta.

1. En Azure Portal, abra el directorio de Azure AD. Haga clic en **Aplicaciones empresariales**, haga clic en el **nombre de aplicación empresarial creado anteriormente** y haga clic en **Inicio de sesión único**.

2. En la página **Configurar el inicio de sesión único con SAML**, edite la sección **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](./media/sharepoint-on-premises-tutorial/add-reply-url.png)

3. En **Dirección URL de respuesta (dirección URL del Servicio de consumidor de aserciones)** , agregue la dirección URL de las aplicaciones web adicionales y haga clic en **Guardar**.

    ![Edición de la configuración básica de SAML](./media/sharepoint-on-premises-tutorial/reply-url-for-web-application.png)

4. En el servidor de SharePoint, abra el **shell de administración de SharePoint 201x** y ejecute los comandos siguientes, utilizando el nombre del emisor de tokens de identidad de confianza que usó anteriormente.
    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
5. En Administración Central, vaya a la aplicación web y habilite el proveedor de identidades de confianza existente.

Es posible que tenga otro escenario en el que quiera conceder acceso a SharePoint local para los usuarios internos. En este escenario, tendría que implementar Microsoft Azure Active Directory Connect, que permite sincronizar los usuarios locales con Azure Active Directory. Esta configuración forma parte de otro artículo. 

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [¿Qué es la identidad híbrida con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
