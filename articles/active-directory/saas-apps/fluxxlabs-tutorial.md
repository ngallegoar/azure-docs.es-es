---
title: 'Tutorial: integración de Azure Active Directory con Fluxx Labs | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Fluxx Labs.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: e624520a9d1f39bc8115ac72e9df0398065928f1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67102387"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Tutorial: integración de Azure Active Directory con Fluxx Labs

En este tutorial, obtendrá información sobre cómo integrar Fluxx Labs con Azure Active Directory (Azure AD).
La integración de Fluxx Labs con Azure AD proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a Fluxx Labs.
* Puede permitir que los usuarios inicien sesión automáticamente en Fluxx Labs (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con Fluxx Labs se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en Fluxx Labs

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Fluxx Labs admite el inicio de sesión único iniciado por **IDP**

## <a name="adding-fluxx-labs-from-the-gallery"></a>Adición de Fluxx Labs desde la galería

Para configurar la integración de Fluxx Labs en Azure AD, será preciso que agregue Fluxx Labs desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Fluxx Labs desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Fluxx Labs**, seleccione **Fluxx Labs** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Fluxx Labs en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Fluxx Labs con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Fluxx Labs.

Para configurar y probar el inicio de sesión único de Azure AD con Fluxx Labs, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Fluxx Labs](#configure-fluxx-labs-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Fluxx Labs](#create-fluxx-labs-test-user)** : para tener un homólogo de Britta Simon en Fluxx Labs que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Fluxx Labs, siga los pasos que se indican a continuación:

1. En la página de integración de la aplicación **Fluxx Labs** de [Azure Portal](https://portal.azure.com/), seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la página **Configurar inicio de sesión único con SAML** realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Fluxx Labs](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:

    | Entorno | Patrón de dirección URL|
    |-------------|------------|
    | Producción | `https://<subdomain>.fluxx.io` |
    | Preproducción | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:

    | Entorno | Patrón de dirección URL|
    |-------------|------------|
    | Producción | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Preproducción | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Fluxx Labs](mailto:travis@fluxxlabs.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Fluxx Labs** (Configurar Fluxx Labs), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-fluxx-labs-single-sign-on"></a>Configuración del inicio de sesión único de Fluxx Labs

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Fluxx Labs como administrador.

2. Seleccione **Administrador** después de la sección **Configuración**.

    ![Configuración de Fluxx Labs](./media/fluxxlabs-tutorial/config1.png)

3. En el panel de administración, seleccione **Plug-ins** (Complementos) > **Integrations** (Integraciones) y, a continuación, seleccione **SAML SSO-(Disabled)** [SAML SSO-(Deshabilitado)]

    ![Configuración de Fluxx Labs](./media/fluxxlabs-tutorial/config2.png)

4. En la sección de atributos, realice los siguientes pasos:

    ![Configuración de Fluxx Labs](./media/fluxxlabs-tutorial/config3.png)

    a. Seleccione la casilla de verificación **SAML SSO** (SSO de SAML).

    b. En el cuadro de texto **Request Path** (Ruta de acceso de la solicitud), escriba **/auth/saml**.

    c. En el cuadro de texto **Callback Path** (Ruta de acceso de devolución de llamada), escriba **/auth/saml/callback**.

    d. En el cuadro **Assertion Consumer Service Url (Single Sign-On URL)** (URL del Servicio de consumidor de aserciones (URL de inicio de sesión único)), escriba el valor de **Reply URL** (URL de respuesta) que introdujo en Azure Portal.

    e. En el cuadro de texto **Audience(SP Entity ID)** (Público (ID de entidad SP)), introduzca el valor **Identificador**, que introdujo en Azure Portal.

    f. En el cuadro de texto **Identity Provider SSO Target URL** (Dirección URL de destino del inicio de sesión único del proveedor de identidades), pegue el valor de **Dirección URL de inicio de sesión**  que ha copiado de Azure Portal.

    g. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de identidades**.

    h. En el cuadro de texto **Name identifier Format** (Formato de identificador de nombre), escriba el valor `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. Haga clic en **Save**(Guardar).

    > [!NOTE]
    > Una vez guardado el contenido, el campo aparecerá en blanco por seguridad, pero se ha guardado el valor en la configuración.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Fluxx Labs.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, después, **Fluxx Labs**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Fluxx Labs**.

    ![Vínculo a Fluxx Labs en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-fluxx-labs-test-user"></a>Creación de un usuario de prueba de Fluxx Labs

Para permitir que los usuarios de Azure AD inicien sesión en Fluxx Labs, deben aprovisionarse en Fluxx Labs. En el caso de Fluxx Labs, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en el sitio de la compañía de Fluxx Labs.

2. Haga clic en el **icono** que aparece a continuación.

    ![Configuración de Fluxx Labs](./media/fluxxlabs-tutorial/config6.png)

3. En el panel, haga clic en el icono que se muestra a continuación para abrir la tarjeta **New PEOPLE** (Nueva persona).

    ![Configuración de Fluxx Labs](./media/fluxxlabs-tutorial/config4.png)

4. En la sección **NEW PEOPLE** (Nueva persona), lleve a cabo estos pasos:

    ![Configuración de Fluxx Labs](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs usa la dirección de correo electrónico como identificador único para los inicios de sesión SSO. Rellene el campo **SSO UID** con la dirección de correo electrónico del usuario, que coincide con la dirección de correo electrónico que usan como inicio de sesión con SSO.

    b. Haga clic en **Save**(Guardar).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Fluxx Labs en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Fluxx Labs para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

