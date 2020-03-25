---
title: 'Tutorial: Integración de Azure Active Directory con Mimecast Personal Portal | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Mimecast Personal Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 259635613855e4d7687cf569c94bbd3dd04027fe
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160629"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Tutorial: Integración de Azure Active Directory con Mimecast Personal Portal

En este tutorial, obtendrá información sobre cómo integrar Mimecast Personal Portal con Azure Active Directory (Azure AD).
La integración de Mimecast Personal Portal con Azure AD proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a Mimecast Personal Portal.
* Puede permitir que los usuarios inicien sesión automáticamente en Mimecast Personal Portal (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con Mimecast Personal Portal, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción de Mimecast Personal Portal con el inicio de sesión único habilitado

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Mimecast Personal Portal admite el inicio de sesión único iniciado por **SP**

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Incorporación de Mimecast Personal Portal desde la galería

Para configurar la integración de Mimecast Personal Portal en Azure AD, deberá agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Mimecast Personal Portal desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Mimecast Personal Portal**, seleccione **Mimecast Personal Portal** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Mimecast Personal Portal en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Mimecast Personal Portal con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Mimecast Personal Portal.

Para configurar y probar el inicio de sesión único de Azure AD con Mimecast Personal Portal, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Mimecast Personal Portal](#configure-mimecast-personal-portal-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en Mimecast Personal Portal](#create-mimecast-personal-portal-test-user)** : para tener un homólogo de Britta Simon en Mimecast Personal Portal que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Mimecast Personal Portal, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Mimecast Personal Portal**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de Mimecast Personal Portal](common/sp-identifier-reply.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: 

    | Region  |  Value | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | Sudáfrica    | `https://za-api.mimecast.com/login/saml`|
    | Australia       | `https://au-api.mimecast.com/login/saml`|
    | Internacional        | `https://jer-api.mimecast.com/login/saml`|

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:

    | Region  |  Value | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Estados Unidos   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Sudáfrica    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Australia       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Internacional        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL: 

    | Region  |  Value | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | Sudáfrica    | `https://za-api.mimecast.com/login/saml`|
    | Australia       | `https://au-api.mimecast.com/login/saml`|
    | Internacional        | `https://jer-api.mimecast.com/login/saml`|

    > [!NOTE]
    > El valor del identificador no es real. Actualícelo con el identificador real. Póngase en contacto con el [equipo de soporte de cliente de Mimecast Personal Portal](https://www.mimecast.com/customer-success/technical-support/) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Mimecast Personal Portal** (Configurar Mimecast Personal Portal), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-mimecast-personal-portal-single-sign-on"></a>Configuración del inicio de sesión único en Mimecast Personal Portal

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de Mimecast Personal Portal.

2. Vaya a **Servicios \> Aplicaciones**.
   
    ![Aplicaciones](./media/mimecast-personal-portal-tutorial/ic794998.png "APLICACIONES")

3. Haga clic en **Authentication Profiles**(Perfiles de autenticación).
   
    ![Authentication Profiles (Perfiles de autenticación)](./media/mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")

4. Haga clic en **New Authentication Profile**(Nuevo perfil de autenticación).
   
    ![New Authentication Profile (Nuevo perfil de autenticación)](./media/mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile (Nuevo perfil de autenticación)")

5. En la sección **Authentication Profile** (Perfil de autenticación), realice estos pasos:
   
    ![Authentication Profile (Perfil de autenticación)](./media/mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")
   
    a. En el cuadro de texto **Description** (Descripción), escriba un nombre para la configuración.
   
    b. Seleccione **Aplicar la autenticación SAML a Mimecast Personal Portal**.
   
    c. Como **Provider** (Proveedor), seleccione **Azure Active Directory**.
   
    d. En el cuadro de texto **Issuer URL** (URL del emisor), pegue el valor de **Identificador de Azure AD** , que ha copiado de Azure Portal.
   
    e. En el cuadro de texto **Login URL**, (Dirección URL de inicio de sesión) pegue el valor de **Dirección URL de inicio de sesión**, que ha copiado de Azure Portal.
   
    f. En el cuadro de texto **Logout URL** (Dirección URL de cierre de sesión), pegue el valor de **Dirección URL de cierre de sesión**, que ha copiado de Azure Portal.

    g. Abra el certificado codificado en **base 64** descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de identidades (metadatos)** .

    h. Seleccione **Permitir inicio de sesión único**.
   
    i. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Mimecast Personal Portal.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Mimecast Personal Portal**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba o seleccione **Mimecast Personal Portal**.

    ![Vínculo a Mimecast Personal Portal en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-mimecast-personal-portal-test-user"></a>Creación de un usuario de prueba en Mimecast Personal Portal

Para permitir que los usuarios de Azure AD inicien sesión en Mimecast Personal Portal, deben aprovisionarse en Mimecast Personal Portal. En el caso de Mimecast Personal Portal, el aprovisionamiento es una tarea manual.

Deberá registrar un dominio para poder crear los usuarios.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en **Mimecast Personal Portal** como administrador.

2. Vaya a **Directorios\> Interno**.
   
    ![Directorios](./media/mimecast-personal-portal-tutorial/ic795003.png "Directorios")

3. Haga clic en **Register New Domain**(Registrar nuevo dominio).
   
    ![Register New Domain (Registrar nuevo dominio)](./media/mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")

4. Una vez creado el nuevo dominio, haga clic en **New Address**(Nueva dirección).
   
    ![New Address (Nueva dirección)](./media/mimecast-personal-portal-tutorial/ic795005.png "New Address")

5. En el cuadro de diálogo de dirección nueva, lleve a cabo los pasos siguientes para la cuenta de Azure AD válida que desea aprovisionar:
   
    ![Guardar](./media/mimecast-personal-portal-tutorial/ic795006.png "Save")
   
    a. En el cuadro de texto **Email Address** (Dirección de correo electrónico), escriba la **dirección de correo electrónico** del usuario, en este caso **BrittaSimon\@contoso.com**.
    
    b. En el cuadro de texto **Nombre global**, escriba el **nombre de usuario** como **BrittaSimon**.

    c. En los cuadros de texto **Contraseña** y **Confirmar contraseña**, escriba la **contraseña** del usuario.
   
    b. Haga clic en **Save**(Guardar).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Mimecast Personal Portal ofrecida por Mimecast Personal Portal para aprovisionar cuentas de usuario de Azure AD.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Mimecast Personal Portal del panel de acceso, debería iniciar sesión automáticamente en la versión de Mimecast Personal Portal para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

