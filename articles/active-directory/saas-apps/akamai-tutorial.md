---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Akamai | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Akamai.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1b7e0d7a-e78f-43a5-af93-b626186e2376
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 042dd242285081001ca48c9f17e4d42c2294c0ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74979141"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-akamai"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Akamai

En este tutorial, obtendrá información sobre cómo integrar Akamai con Azure Active Directory (Azure AD). Al integrar Akamai con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Akamai.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Akamai con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en Akamai.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

- Slack admite el inicio de sesión único iniciado por IDP

## <a name="adding-akamai-from-the-gallery"></a>Adición de Akamai desde la galería

Para configurar la integración de Akamai en Azure AD, será preciso agregar Akamai desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Akamai** en el cuadro de búsqueda.
1. Seleccione **Akamai** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-akamai"></a>Configuración y prueba del inicio de sesión único de Azure AD para Akamai

Configure y pruebe el inicio de sesión único de Azure AD con Akamai mediante una usuaria de prueba llamada **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Akamai.

Para configurar y probar el inicio de sesión único de Azure AD con Akamai, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Akamai](#configure-akamai-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de Akamai](#create-akamai-test-user)** , para tener un homólogo de B.Simon en Akamai que esté vinculado a la representación de ella en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Akamai**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<Yourapp>.login.go.akamai-access.com/sp/response`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https:// <Yourapp>.login.go.akamai-access.com/sp/response`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de cliente de Akamai](https://www.akamai.com/us/en/contact-us/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up Akamai** (Configurar Akamai), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a Akamai mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Akamai**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-akamai-sso"></a>Configuración del inicio de sesión único de Akamai

### <a name="setting-up-idp"></a>Configuración de IDP

1. Inicie sesión en la consola de **acceso de aplicaciones empresariales de Akamai**.
1. En la **consola de acceso de aplicaciones empresariales de Akamai**, seleccione **Identity** > **Identity Providers** (Identidad > Proveedores de identidades).

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure01.png)

1. Haga clic en **Add Identity Provider** (Agregar proveedor de identidades).

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure02.png)

    a. Especifique el valor de **Unique Name** (Nombre único).
    
    b. Elija **Third Party SAML** (SAML de terceros) y haga clic en **Create Identity Provider and Configure** (Crear proveedor de identidades y configurar).

### <a name="general-settings"></a>Configuración general

1. **Identity Intercept** (Interceptación de identidad): especifique el nombre de la dirección URL base del SP que se usará para la configuración de Azure AD.

    > [!NOTE]
    > Puede elegir tener su propio dominio personalizado (requerirá una entrada DNS y un certificado). En este ejemplo, vamos a usar el dominio de Akamai.

1. **Akamai Cloud Zone** (Zona de nube de Akamai): seleccione la zona de nube adecuada.
1. **Certificate Validation** (Validación de certificado): compruebe la documentación de Akamai (opcional)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure03.png)

### <a name="authentication-configuration"></a>Configuración de autenticación

1. URL: especifique la misma dirección URL que la que indicó en la interceptación de identidad (aquí es donde se redirigirá a los usuarios después de la autenticación).
2. Logout URL (Dirección URL de cierre de sesión): actualice la dirección URL de cierre de sesión.
3. Sign SAML Request (Firmar solicitud SAML): el valor está desactivado de manera predeterminada.
4. Para el archivo de metadatos de IDP, agregue la aplicación en la consola de Azure AD.

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure04.png)

### <a name="header-based-authentication"></a>Autenticación basada en encabezados

Autenticación basada en encabezados de Akamai

1. Elija **Custom HTTP** (HTTP personalizado) en el Asistente para agregar aplicaciones.

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure05.png)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure06.png)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure07.png)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure08.png)

#### <a name="authentication"></a>Authentication

![Configuración de Akamai](./media/header-akamai-tutorial/configure09.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure10.png)

#### <a name="services"></a>Servicios

1. Haga clic en Save and Go to Authentication (Guardar e ir a Autenticación).

![Configuración de Akamai](./media/header-akamai-tutorial/configure11.png)

#### <a name="advanced-settings"></a>Configuración avanzada

1. En los **Customer HTTP Headers** (Encabezados de HTTP del cliente), especifique **CustomerHeader** y **SAML Attribute** (Atributo de SAML).

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure12.png)

1. Haga clic en el botón **Save and go to Deployment** (Guardar e ir a Implementación).

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure13.png)

#### <a name="deploy-the-application"></a>Implementación de la aplicación

1. Haga clic en el botón **Deploy Application** (Implementar aplicación).

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure14.png)

1. Compruebe que la aplicación se ha implementado correctamente.

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure15.png)

### <a name="kerberos-authentication"></a>Autenticación Kerberos

#### <a name="remote-desktop"></a>Escritorio remoto

1. Elija **RDP** en el Asistente para agregar aplicaciones.

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure16.png)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure17.png)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure18.png)

1. Especifique el conector que funcionará con esta aplicación.

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure19.png)

#### <a name="authentication"></a>Authentication

Haga clic en **Save and go to Services** (Guardar e ir a Servicios).

![Configuración de Akamai](./media/header-akamai-tutorial/configure20.png)

#### <a name="services"></a>Servicios

Haga clic en **Save and go to Advanced Settings** (Guardar e ir a Configuración avanzada).

![Configuración de Akamai](./media/header-akamai-tutorial/configure21.png)

#### <a name="advanced-settings"></a>Configuración avanzada

Haga clic en **Save and go to Deployment** (Guardar e ir a Implementación).

![Configuración de Akamai](./media/header-akamai-tutorial/configure22.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure23.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure24.png)

### <a name="deployment"></a>Implementación

#### <a name="ssh"></a>SSH

1. Vaya a Add Applications (Agregar aplicaciones) y elija **SSH**.

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure25.png)

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure26.png)

1. Configure Application Identity (Identidad de aplicación).

    ![Configuración de Akamai](./media/header-akamai-tutorial/configure27.png)

    a. Especifique un nombre y una descripción.

    b. Especifique la dirección IP y el nombre de dominio completo del servidor de aplicaciones y el puerto para SSH.

    c. Especifique el nombre de usuario y la frase de contraseña de SSH *Consulte la consola de acceso de aplicaciones empresariales de Akamai.

    d. Especifique el nombre de host externo.

    e. Especifique la ubicación del conector y elija el conector.

#### <a name="authentication"></a>Authentication

Haga clic en **Save and go to Services** (Guardar e ir a Servicios).

![Configuración de Akamai](./media/header-akamai-tutorial/configure28.png)

#### <a name="services"></a>Servicios

Haga clic en **Save and go to Advanced Settings** (Guardar e ir a Configuración avanzada).

![Configuración de Akamai](./media/header-akamai-tutorial/configure29.png)

#### <a name="advanced-settings"></a>Configuración avanzada

Haga clic en Save and go to Deployment (Guardar e ir a Implementación).

![Configuración de Akamai](./media/header-akamai-tutorial/configure30.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure31.png)

#### <a name="deployment"></a>Implementación

Haga clic en **Deploy Application** (Implementar aplicación).

![Configuración de Akamai](./media/header-akamai-tutorial/configure32.png)

### <a name="kerberos-applications"></a>Aplicaciones Kerberos

#### <a name="adding-directory"></a>Incorporación de directorio

![Configuración de Akamai](./media/header-akamai-tutorial/configure33.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure34.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure35.png)

![Configuración de Akamai](./media/header-akamai-tutorial/configure36.png)

### <a name="create-akamai-test-user"></a>Creación de un usuario de prueba de Akamai

En esta sección, creará un usuario llamado B.Simon en Akamai. Colabore con el  [equipo de soporte técnico al cliente de Akamai](https://www.akamai.com/us/en/contact-us/) para agregar los usuarios a la plataforma de Akamai. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Akamai en el panel de acceso, iniciará sesión automáticamente en la versión de Akamai para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Prueba de Akamai con Azure AD](https://aad.portal.azure.com/)
