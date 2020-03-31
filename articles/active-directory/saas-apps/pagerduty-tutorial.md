---
title: 'Tutorial: Integración de Azure Active Directory con PagerDuty | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y PagerDuty.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: b7a522aaf35303bbd87e7aafe65b1302f1b98bc0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67095330"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Tutorial: Integración de Azure Active Directory con PagerDuty

En este tutorial, aprenderá a integrar PagerDuty con Azure Active Directory (Azure AD).
La integración de PagerDuty con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a PagerDuty.
* Puede permitir que los usuarios inicien sesión automáticamente en PagerDuty (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con PagerDuty, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en PagerDuty

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* PagerDuty admite el inicio de sesión único iniciado por **SP**

## <a name="adding-pagerduty-from-the-gallery"></a>Agregación de PagerDuty desde la galería

Para configurar la integración de PagerDuty en Azure AD, deberá agregar PagerDuty desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar PagerDuty desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **PagerDuty**, seleccione **PagerDuty** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![PagerDuty en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con PagerDuty con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de PagerDuty.

Para configurar y probar el inicio de sesión único de Azure AD con PagerDuty, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de PagerDuty](#configure-pagerduty-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de PagerDuty](#create-pagerduty-test-user)** : para tener un homólogo de Britta Simon en PagerDuty que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con PagerDuty, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **PagerDuty**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de PagerDuty](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.pagerduty.com`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<tenant-name>.pagerduty.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de PagerDuty](https://www.pagerduty.com/support/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up PagerDuty** (Configurar PagerDuty), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-pagerduty-single-sign-on"></a>Configuración del inicio de sesión único de PagerDuty

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía Pagerduty como administrador.

2. En el menú de la parte superior, haga clic en **Configuración de cuenta**.

    ![Account Settings (Configuración de cuenta)](./media/pagerduty-tutorial/ic778535.png "Configuración de cuenta")

3. Haga clic en **Inicio de sesión único**.

    ![Inicio de sesión único](./media/pagerduty-tutorial/ic778536.png "Inicio de sesión único")

4. En la página **Habilitar inicio de sesión único (SSO)** , siga estos pasos:

    ![Habilitar el inicio de sesión único](./media/pagerduty-tutorial/ic778537.png "Habilitar el inicio de sesión único")

    a. Abra el certificado codificado en Base 64 que descargó de Azure Portal en el Bloc de notas, copie el contenido en el Portapapeles y, luego, péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509)
  
    b. En el cuadro de texto **URL de inicio de sesión**, pegue la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.
  
    c. En el cuadro de texto **URL de cierre de sesión**, pegue la **dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    d. Seleccione **Allow username/password login** (Permitir inicio de sesión con nombre de usuario/contraseña).

    e. Seleccione la casilla de verificación **Require EXACT authentication context comparison** (Requerir comparación de contexto de autenticación EXACT).

    f. Haga clic en **Guardar cambios**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a PagerDuty.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **PagerDuty**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **PagerDuty**.

    ![Vínculo a PagerDuty en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-pagerduty-test-user"></a>Creación de un usuario de prueba de PagerDuty

Para permitir que los usuarios de Azure AD inicien sesión en PagerDuty, deben aprovisionarse en PagerDuty.  
En el caso de PagerDuty, el aprovisionamiento es una tarea manual.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de PagerDuty ofrecida por PagerDuty para aprovisionar cuentas de usuario de Azure Active Directory.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **Pagerduty** .

2. En el menú de la parte superior, haga clic en **Usuarios**.

3. Haga clic en **Agregar usuarios**.
   
    ![Incorporación de usuarios](./media/pagerduty-tutorial/ic778539.png "Agregar usuarios")

4.  En el cuadro de diálogo **Invite your team** (Invitar a su equipo), realice los pasos siguientes:
   
    ![Invitar a su equipo](./media/pagerduty-tutorial/ic778540.png "Invitar a su equipo")

    a. En el cuadro de texto **First and Last Name** (Nombre y apellidos), escriba el nombre de un usuario, por ejemplo, **Britta Simon**. 
   
    b. Escriba la dirección de **correo electrónico** del usuario, por ejemplo **brittasimon\@contoso.com**.
   
    c. Haga clic en **Add** (Agregar) y después en **Send Invites** (Enviar invitaciones).
   
    >[!NOTE]
    >Todos los usuarios agregados recibirán una invitación para crear una cuenta de PagerDuty.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de PagerDuty en el panel de acceso y debería iniciar sesión automáticamente en la versión de PagerDuty para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

