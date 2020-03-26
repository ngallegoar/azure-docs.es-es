---
title: 'Tutorial: Integración de Azure Active Directory con Dropbox for Business | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Dropbox for Business.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: df7fc366c5087e66c3022c212870397d77e6e34d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77046759"
---
# <a name="tutorial-integrate-dropbox-for-business-with-azure-active-directory"></a>Tutorial: Integración de Azure Active Directory con Dropbox for Business

En este tutorial, aprenderá a integrar Dropbox for Business con Azure Active Directory (Azure AD). Si integra Dropbox for Business con Azure AD, podrá:

* Controle en Azure AD quién tiene acceso a Dropbox for Business.
* Permita que los usuarios inicien sesión automáticamente en Dropbox for Business con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Suscripción habilitada para el inicio de sesión único en Dropbox for Business.

## <a name="scenario-description"></a>Descripción del escenario

* En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. Dropbox for Business admite inicio de sesión único iniciado por **SP**.

* Dropbox for Business admite el [aprovisionamiento y desaprovisionamiento de usuarios de forma automática](dropboxforbusiness-tutorial.md).
* Una vez configurado Dropbox, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad).

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Adición de Dropbox for Business desde la galería

Para configurar la integración de Dropbox for Business en Azure AD, es preciso agregar Dropbox for Business desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Dropbox for Business** en el cuadro de búsqueda.
1. Seleccione **Dropbox for Business** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Dropbox for Business mediante un usuario de prueba llamado **Britta Simon**. Para que el SSO funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Dropbox for Business.

Para configurar y probar el inicio de sesión único de Azure AD con Dropbox for Business, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.    
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Dropbox for Business](#configure-dropbox-for-business-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Dropbox for Business](#create-dropbox-for-business-test-user)** : para tener un homólogo de Britta Simon en Dropbox for Business que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Dropbox for Business**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.dropbox.com/sso/<id>`

    b. En el cuadro de texto **Identificador (Id. de entidad)** , escriba un valor: `Dropbox`

    > [!NOTE]
    > El valor de URL de inicio de sesión anterior no es real. El valor se actualizará con la dirección URL de inicio de sesión real, que se explica más adelante en el tutorial.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up Dropbox for Business** (Configurar Dropbox for Business), copie las direcciones URL correctas según sus requisitos.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión


### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado Britta Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `Britta Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `BrittaSimon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Dropbox for Business para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Dropbox for Business**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En la lista de usuarios del cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-dropbox-for-business-sso"></a>Configuración del inicio de sesión único de Dropbox for Business

1. Para automatizar la configuración en Dropbox for Business, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Configurar Dropbox for Business** para ir a la aplicación Dropbox for Business. Desde allí, proporcione las credenciales del administrador para iniciar sesión en Dropbox for Business. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 8.

    ![Configuración](common/setup-sso.png)

3. Si desea configurar Dropbox for Business manualmente, abra una nueva ventana del explorador web y vaya al inquilino de Dropbox for Business, inicie sesión en él y realice los siguientes pasos:

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/ic769509.png "Configurar inicio de sesión único")

4. Haga clic en el **icono de usuario** y seleccione la pestaña **Settings** (Configuración).

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/configure1.png "Configurar inicio de sesión único")

5. En el panel de navegación de la izquierda, haga clic en **Admin Console**(Consola de administración).

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/configure2.png "Configurar inicio de sesión único")

6. En **Admin Console** (Consola de administración), haga clic en la opción **Settings** (Configuración) del panel de navegación izquierdo.

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/configure3.png "Configurar inicio de sesión único")

7. Seleccione la opción **Single sign-on** (Inicio de sesión único) en la sección **Authentication** (Autenticación).

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/configure4.png "Configurar inicio de sesión único")

8. En la sección **Single sign-on** (Inicio de sesión único), siga estos pasos:  

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/configure5.png "Configurar inicio de sesión único")

    a. Seleccione **Required** (Obligatorio) como opción en el menú desplegable para **Single sign-on** (Inicio de sesión único).

    b. Haga clic en **Add sign-in URL** (Agregar inicio de sesión único) y en el cuadro de texto **Identity provider sign-in URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **Login URL** (Dirección URL de inicio de sesión) que ha copiado de Azure Portal y luego seleccione **Done** (Listo).

    ![Configuración del inicio de sesión único](./media/dropboxforbusiness-tutorial/configure6.png "Configurar inicio de sesión único")

    c. Haga clic en **Upload certificate** (Cargar certificado) y, a continuación, vaya al **archivo de certificado codificado en Base64** que ha descargado de Azure Portal.

    d. Haga clic en **Copy link** (Copiar vínculo) y pegue el valor copiado en el cuadro de texto **Dirección URL de inicio de sesión** en la sección de **Dominio y direcciones URL de Dropbox for Business** en Azure Portal.

    e. Haga clic en **Save**(Guardar).

### <a name="create-dropbox-for-business-test-user"></a>Creación de un usuario de prueba de Dropbox for Business

En esta sección, creará un usuario llamado a Britta Simon en Dropbox for Business. Dropbox for Business admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en Dropbox for Business, se crea uno nuevo después de la autenticación.

>[!Note]
>Si necesita crear un usuario de forma manual, póngase en contacto con el [equipo de soporte de cliente de Dropbox for Business](https://www.dropbox.com/business/contact)

### <a name="test-sso"></a>Prueba de SSO

Cuando selecciona el icono de Dropbox for Business en el Panel de acceso, debería iniciar sesión automáticamente en la versión de Dropbox for Business para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Protección de Dropbox con controles y visibilidad avanzados](https://docs.microsoft.com/cloud-app-security/protect-dropbox)