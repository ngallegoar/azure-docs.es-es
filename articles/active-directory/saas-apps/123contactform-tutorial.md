---
title: 'Tutorial: Integración de Azure Active Directory con 123ContactForm | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y 123ContactForm.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e8328ac4008950d67cf5645c6a48c48d1196085
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73154925"
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Tutorial: integración de Azure Active Directory con 123ContactForm

En este tutorial, obtendrá información sobre cómo integrar 123ContactForm con Azure Active Directory (Azure AD).
La integración de 123ContactForm con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a 123ContactForm.
* Puede permitir que los usuarios inicien sesión automáticamente en 123ContactForm (inicio de sesión único) con su cuenta de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con 123ContactForm, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en 123ContactForm

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* 123ContactForm admite el inicio de sesión único iniciado por **SP e IDP**
* 123ContactForm admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-123contactform-from-the-gallery"></a>Agregar 123ContactForm desde la galería

Para configurar la integración de 123ContactForm en Azure AD, será preciso que agregue 123ContactForm desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar 123ContactForm desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **123ContactForm**, seleccione **123ContactForm** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![123ContactForm en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con 123ContactForm con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de 123ContactForm.

Para configurar y probar el inicio de sesión único de Azure AD con 123ContactForm, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en 123ContactForm](#configure-123contactform-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba en 123ContactForm](#create-123contactform-test-user)** : para tener un homólogo de Britta Simon en 123ContactForm que esté vinculado a la representación de Azure AD de usuario.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único en Azure AD con 123ContactForm, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **123ContactForm**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de 123ContactForm](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de 123ContactForm](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizar este valor con las direcciones URL y el identificador reales, como se explica más adelante en el tutorial.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En la sección **Set up 123ContactForm** (Configurar 123ContactForm), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-123contactform-single-sign-on"></a>Configuración del inicio de sesión único en 123ContactForm

1. Para configurar el inicio de sesión único en **123ContactForm**, vaya a [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) y siga estos pasos:

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/submit.png) 

    a. En el cuadro de texto de **Correo electrónico**, escriba la dirección de correo electrónico del usuario, por ejemplo, **BrittaSimon\@Contoso.com**.

    b. Haga clic en **Upload** (Cargar) y vaya al archivo XML de metadatos que ha descargado de Azure Portal.

    c. Haga clic en **ENVIAR FORMULARIO**.

2. En **Microsoft Azure AD - Inicio de sesión único - Configurar las opciones de la aplicación**, realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/url3.png)

    a. Si desea configurar la aplicación en el **modo iniciado por IDP**, copie el valor de **IDENTIFIER** (IDENTIFICADOR) de la instancia y péguelo en el cuadro de texto **Identificador** en la sección **Configuración básica de SAML** en Azure Portal.

    b. Si desea configurar la aplicación en el **modo iniciado por IDP**, copie el valor de **REPLY URL** (URL DE RESPUESTA) de la instancia y péguelo en el cuadro de texto **URL de respuesta** en la sección **Configuración básica de SAML**  en Azure Portal.

    c. Si desea configurar la aplicación en el **modo iniciado por SP**, copie el valor de **SIGN ON URL** (URL DE INICIO DE SESIÓN) de la instancia y péguelo en el cuadro de texto **URL de inicio de sesión** en la sección **Configuración básica de SAML**  en Azure Portal.

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a 123ContactForm.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **123ContactForm**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **123ContactForm**.

    ![Vínculo de 123ContactForm en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-123contactform-test-user"></a>Creación de un usuario de prueba en 123ContactForm

En esta sección, se crea un usuario llamado Britta Simon en 123ContactForm. 123ContactForm admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en 123ContactForm, se crea otro después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de 123ContactForm en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de 123ContactForm para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)