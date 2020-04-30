---
title: 'Tutorial: Integración de Azure Active Directory con Mitel Connect | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Mitel Connect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 204f540b-09f1-452b-a52f-78143710ef76
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.openlocfilehash: 26a761708f56ff7aba8daf86d2991579e60291cb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81870206"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect"></a>Tutorial: Integración de Azure Active Directory con Mitel MiCloud Connect

En este tutorial, aprenderá a integrar Mitel MiCloud Connect con Azure Active Directory (Azure AD). La integración de MiCloud Connect con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a aplicaciones de MiCloud Connect con sus credenciales de empresa.
* Puede permitir que los usuarios de su cuenta inicien sesión automáticamente en MiCloud Connect (inicio de sesión único) con sus cuentas de Azure AD.


Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con MiCloud Connect, se necesitan los siguientes elementos:

* Una suscripción de Azure AD

  Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una cuenta de Mitel MiCloud Connect

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial configurará y probará el inicio de sesión único (SSO) de Azure AD.

* Mitel Connect admite SSO iniciado por **SP**

## <a name="adding-mitel-connect-from-the-gallery"></a>Adición de Mitel Connect desde la galería

Para configurar la integración de Mitel Connect en Azure AD, será preciso que agregue Mitel Connect desde la galería a la lista de aplicaciones SaaS administradas en Azure Portal.

**Para agregar Mitel Connect desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y, a continuación, haga clic en **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Haga clic en **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. Escriba **Mitel Connect** en el campo de búsqueda, haga clic en **Mitel Connect** en el panel de resultados y, a continuación, haga clic en **Agregar**.

     ![Mitel Connect en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con MiCloud Connect con un usuario de prueba llamado **Britta Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de MiCloud Connect.

Para configurar y probar el inicio de sesión único de Azure AD con MiCloud Connect, es preciso completar los siguientes pasos:

1. **[Configuración de MiCloud Connect para SSO con Azure AD](#configure-micloud-connect-for-sso-with-azure-ad)** , para permitir que los usuarios utilicen esta característica y configuren las opciones de inicio de sesión único en el lado de la aplicación.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
4. **[Creación de un usuario de prueba de Mitel MiCloud Connect](#create-a-mitel-micloud-connect-test-user)** , para tener un homólogo de Britta Simon en la cuenta de MiCloud Connect que esté vinculado a su representación en Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>Configuración de MiCloud Connect para SSO con Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD para MiCloud Connect en Azure Portal y configurará su cuenta de MiCloud Connect para permitir el inicio de sesión único con Azure AD.

Para configurar el SSO de MiCloud Connect para Azure AD, lo más fácil es abrir Azure Portal y el portal de Mitel Account en paralelo. Deberá copiar información desde Azure Portal al portal de Mitel Account y desde el portal de Mitel Account a Azure Portal.


1. Para abrir la página de configuración en [Azure Portal](https://portal.azure.com/), haga lo siguiente:

    a. En la página de integración de aplicaciones de **Mitel Connect**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

    b. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **SAML**.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)
    
    Aparece la página de inicio de sesión basada en SAML.

2. Para abrir el cuadro de diálogo de configuración en el portal de Mitel Account, haga lo siguiente:

    a. En el menú **Phone System** (Sistema telefónico), haga clic en **Add-On Features** (Características complementarias).

    b. A la derecha de **Single Sign-On** (Inicio de sesión único), haga clic en **Activate** (Activar) o **Settings** (Configuración).
    
    Aparece el cuadro de diálogo de configuración del inicio de sesión único de Connect.
    
3. Active la casilla **Enable Single Sign-On** (Habilitar el inicio de sesión único).
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_Enable.png)


4. En Azure Portal, haga clic en el icono **Editar** en la sección **Configuración básica de SAML**.
    ![image](common/edit-urls.png)

    Aparece el cuadro de diálogo de configuración básica de SAML.

5.  Copie la dirección URL del campo **Mitel Identifier (Entity ID)** (Identificador de Mitel [id. de entidad]) del portal de Mitel Account y péguelo en el campo **Identificador (Id. de entidad)** en Azure Portal.

6. Copie la dirección URL del campo **Reply URL (Assertion Consumer Service URL)** (URL de respuesta [URL del servicio de consumidor de aserciones]) del portal de Mitel Account y péguelo en el campo **URL de respuesta (URL del Servicio de consumidor de aserciones)** de Azure Portal.  
   ![image](./media/mitel-connect-tutorial/Mitel_Azure_BasicConfig.png)

7. En el cuadro de texto **Sign on URL** (Dirección URL de inicio de sesión), escriba una de las siguientes direcciones URL:

    * **https://portal.shoretelsky.com** : para usar el portal de Mitel Account como aplicación Mitel predeterminada
    * **https://teamwork.shoretel.com** : para usar Teamwork como aplicación Mitel predeterminada

    **NOTA**: La aplicación de Mitel predeterminada es la aplicación a la que se accede cuando un usuario hace clic en el icono de Mitel Connect en el Panel de acceso. También es la aplicación a la que se accede al realizar una instalación de prueba desde Azure AD.

8. Haga clic en **Guardar** en el cuadro de diálogo **Configuración básica de SAML** de Azure Portal.

9. En la sección **Certificado de firma SAML** de la página **Inicio de sesión basado en SAML** de Azure Portal, haga clic en **Descargar** junto a **Certificado (Base64)** para descargar el **Certificado de firma** y guardarlo en su equipo.
    ![image](./media/mitel-connect-tutorial/Azure_SigningCert.png)

10. Abra el archivo del certificado de firma en un editor de texto, copie todos los datos del archivo y, a continuación, pegue los datos en el campo **Signing Certificate** (Certificado de firma) del portal de Mitel Account. 
    ![image](./media/mitel-connect-tutorial/Mitel_Connect_SigningCert.png)

11. En la sección **Configurar Mitel Connect** de la página **Inicio de sesión basado en SAML** de Azure Portal, haga lo siguiente:

    a. Copie la dirección URL del campo **URL de inicio de sesión** y péguela en el campo **Sign-in URL** (URL de inicio de sesión) del portal de Mitel Account.

    b. Copie la dirección URL del campo **Identificador de Azure AD** y péguela en el campo **Entity ID** (Id. de entidad) del portal de Mitel Account.
    ![image](./media/mitel-connect-tutorial/Mitel_Azure_SetupConnect.png)

12. Haga clic en **Save** (Guardar) en el cuadro de diálogo **Connect Single Sign-On Settings** (Configuración del inicio de sesión único de Connect) en el portal Mitel Account.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

En esta sección creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, haga clic en **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Haga clic en **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo Propiedades del usuario, siga estos pasos:

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@\<dominiodesuempresa\>.\<extensión\>.  
Por ejemplo, BrittaSimon@contoso.com.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Mitel Connect.

1. En Azure Portal, haga clic en **Aplicaciones empresariales** y, a continuación, haga clic en **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, haga clic en **Mitel Connect**.

    ![Vínculo a Mitel Connect en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en **Agregar usuario** y, a continuación, en **Usuarios y grupos** del cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista **Usuarios** y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario de la lista y, después, haga clic en **Seleccionar** en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en **Asignar**.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Creación de un usuario de prueba de Mitel MiCloud Connect

En esta sección, creará un usuario llamado Britta Simon en su cuenta de MiCloud Connect. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

Para obtener más información sobre cómo agregar usuarios en el portal de Mitel Account, consulte el artículo [Adding a user](https://oneview.mitel.com/s/article/Adding-a-User-092815) (Adición de un usuario en la base de conocimientos de Mitel).

Cree un usuario en su cuenta de MiCloud Connect con los detalles siguientes:

  * **Nombre:** Britta Simon

* **Dirección de correo electrónico empresarial:** `brittasimon@<yourcompanydomain>.<extension>`   
(Ejemplo: [brittasimon@contoso.com](mailto:brittasimon@contoso.com))

* **Nombre de usuario:** `brittasimon@<yourcompanydomain>.<extension>`  
(Ejemplo: [brittasimon@contoso.com](mailto:brittasimon@contoso.com); el nombre de usuario suele ser el mismo que la dirección de correo electrónico empresarial del usuario)

**NOTA:** El nombre del usuario de MiCloud Connect del usuario debe ser idéntico a la dirección de correo electrónico del usuario en Azure.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono Mitel Connect del Panel de acceso, se le redirigirá automáticamente para iniciar sesión en la aplicación de MiCloud Connect que configuró como predeterminada en el campo **Sign on URL** (Dirección URL de inicio de sesión). Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
