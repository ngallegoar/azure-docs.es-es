---
title: 'Tutorial: Integración de Azure Active Directory con New Relic | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y New Relic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: jeedes
ms.openlocfilehash: dfa5fbcf5df8ee314aab3e4a8228d81e7e14565b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74233517"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Tutorial: Integración de Azure Active Directory con New Relic

En este tutorial, aprenderá a integrar New Relic con Azure Active Directory (Azure AD).
La integración de New Relic con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a New Relic.
* Puede permitir que los usuarios inicien sesión automáticamente en New Relic (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con New Relic, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en New Relic

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* New Relic admite el inicio de sesión único iniciado por **SP**

## <a name="adding-new-relic-from-the-gallery"></a>Adición de New Relic desde la galería

Para configurar la integración de New Relic en Azure AD, es precios agregar dicha solución desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar New Relic desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **New Relic**, seleccione **New Relic** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![New Relic en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con New Relic con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de New Relic.

Para configurar y probar el inicio de sesión único de Azure AD con New Relic, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en New Relic](#configure-new-relic-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de New Relic](#create-new-relic-test-user)** : el objetivo es tener un homólogo de Britta Simon en New Relic que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con New Relic, realice los pasos siguientes:

1. En la página de integración de la aplicación [New Relic](https://portal.azure.com/) de **Azure Portal**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de New Relic](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL que se ajuste a este patrón: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login`. No olvide utilizar su propio identificador de cuenta de New Relic.

    b. En el cuadro de texto **Identificador (Id. de entidad)** , escriba una dirección URL: `rpm.newrelic.com`

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up New Relic** (Configurar New Relic), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-new-relic-single-sign-on"></a>Configuración del inicio de sesión único de New Relic

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de **New Relic** como administrador.

2. En el menú de la parte superior, haga clic en **Configuración de cuenta**.
   
    ![Account Settings (Configuración de cuenta)](./media/new-relic-tutorial/ic797036.png "Configuración de cuenta")

3. Haga clic en la pestaña **Seguridad y autenticación** y, luego, haga clic en la pestaña **Inicio de sesión único**.
   
    ![Inicio de sesión único](./media/new-relic-tutorial/ic797037.png "Inicio de sesión único")

4. En la página de diálogo SAML, realice los pasos siguientes:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
    a. Haga clic en **Elegir archivo** para cargar el certificado de Azure Active Directory descargado.

    b. En el cuadro de texto **Dirección URL de inicio de sesión remoto**, pegue el valor de **dirección de inicio de sesión** que copió de Azure Portal.
   
    c. En el cuadro de texto **Logout landing URL** (Dirección URL de cierre de sesión), pegue el valor de **dirección URL de cierre de sesión** que copió de Azure Portal.

    d. Haga clic en **Guardar los cambios**.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a New Relic.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **New Relic**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **New Relic**.

    ![Vínculo a New Relic en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-new-relic-test-user"></a>Creación de un usuario de prueba de New Relic

Para permitir que los usuarios de Azure Active Directory inicien sesión en New Relic, deben aprovisionarse en New Relic. En el caso de New Relic, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario a New Relic, realice los siguientes pasos:**

1. Inicie sesión como administrador en el sitio de la compañía **New Relic** .

2. En el menú de la parte superior, haga clic en **Configuración de cuenta**.
   
    ![Account Settings (Configuración de cuenta)](./media/new-relic-tutorial/ic797040.png "Configuración de cuenta")

3. En el panel **Cuenta** que se encuentra en el lado izquierdo, haga clic en **Resumen** y, luego, haga clic en **Agregar usuario**.
   
    ![Account Settings (Configuración de cuenta)](./media/new-relic-tutorial/ic797041.png "Configuración de cuenta")

4. En el cuadro de diálogo **Usuarios activos** , realice los pasos siguientes:
   
    ![Usuarios activos](./media/new-relic-tutorial/ic797042.png "Usuarios activos")
   
    a. En el cuadro de texto **Correo electrónico** , escriba la dirección de correo electrónico de un usuario válido de Azure Active Directory que quiera aprovisionar.

    b. Como **Rol**, seleccione **Usuario**.

    c. Haga clic en **Agregar este usuario**.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de New Relic ofrecida por New Relic para aprovisionar cuentas de usuario de Azure AD.
> 

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de New Relic en el panel de acceso, debería iniciar sesión automáticamente en la versión de New Relic para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

