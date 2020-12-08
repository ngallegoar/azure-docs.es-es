---
title: 'Tutorial: Integración de Azure Active Directory con LoginRadius | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y LoginRadius.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.openlocfilehash: 1376dcb76c22bcd70937f533d337ee9679e9dc59
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96455820"
---
# <a name="tutorial-azure-active-directory-integration-with-loginradius"></a>Tutorial: Integración de Azure Active Directory con LoginRadius

En este tutorial, aprenderá a integrar LoginRadius con Azure Active Directory (Azure AD).

La integración de LoginRadius con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a LoginRadius.
* Puede permitir que los usuarios inicien sesión automáticamente en LoginRadius (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con LoginRadius, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único en LoginRadius.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* LoginRadius admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-loginradius-from-the-gallery"></a>Adición de LoginRadius desde la galería

Para configurar la integración de LoginRadius con Azure AD, será preciso que agregue LoginRadius desde la galería a la lista de aplicaciones SaaS administradas.

**Incorporación de LoginRadius desde la galería:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , seleccione el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **LoginRadius**, seleccione **LoginRadius** en el panel de resultados y, a continuación, seleccione el botón **Agregar** para incorporar la aplicación.

    ![LoginRadius en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con LoginRadius con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de LoginRadius.

Para configurar y probar el inicio de sesión único de Azure AD con LoginRadius, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de LoginRadius](#configure-loginradius-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de LoginRadius](#create-loginradius-test-user)** : para tener un homólogo de Britta Simon en LoginRadius que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con LoginRadius, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **LoginRadius**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el panel **Seleccione un método de inicio de sesión único**, elija el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configuración del inicio de sesión único con SAML**, seleccione el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**:

   ![Información de dominio y direcciones URL de inicio de sesión único de LoginRadius](common/sp-identifier.png)

   1. En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://secure.loginradius.com/login`

   1. En el cuadro de texto **Identificador (Id. de entidad)** , escriba la dirección URL: `https://lr.hub.loginradius.com/`

   1. En el cuadro de texto **Dirección URL de respuesta (URL del Servicio de consumidor de aserciones)** , escriba la URL de ACS de LoginRadius: `https://lr.hub.loginradius.com/saml/serviceprovider/AdfsACS.aspx` 

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdalo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up LoginRadius** (Configurar LoginRadius), copie las direcciones URL apropiadas en función de sus necesidades.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

   - URL de inicio de sesión

   - Identificador de Azure AD

   - URL de cierre de sesión

## <a name="configure-loginradius-single-sign-on"></a>Configuración del inicio de sesión único de LoginRadius

En esta sección, habilitará el inicio de sesión único de Azure AD en la consola de administración de LoginRadius.

1. Inicie sesión en su cuenta de la [consola de administración](https://adminconsole.loginradius.com/login) de LoginRadius.

2. Vaya a la sección **Team Management** (Administración del equipo) en la [consola de administración de LoginRadius](https://secure.loginradius.com/account/team).

3. Seleccione la pestaña **Inicio de sesión único** y, a continuación, **Azure AD**:

   ![Captura de pantalla que muestra el menú de inicio de sesión único en la consola de administración del equipo de LoginRadius.](./media/loginradius-tutorial/azure-ad.png)
4. En la página de configuración de Azure AD, complete los pasos siguientes:

   ![Captura de pantalla que muestra la configuración de Azure Active Directory en la consola de administración del equipo de LoginRadius.](./media/loginradius-tutorial/single-sign-on.png)

    1. En **ID Provider Location** (Ubicación del proveedor de identidades), escriba el punto de conexión de inicio de sesión que obtenga de la cuenta de Azure AD.

    1. En **ID Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), escriba el punto de conexión de cierre de sesión que obtenga de la cuenta de Azure AD.
 
    1. En **Certificado del proveedor de identidades**, escriba el certificado de Azure AD que obtenga de la cuenta de Azure AD. Escriba el valor del certificado con el encabezado y el pie de página. Ejemplo: `-----BEGIN CERTIFICATE-----<certifciate value>-----END CERTIFICATE-----`

    1. En **Service Provider Certificate** (Certificado del proveedor de servicios) y **Server Provider Certificate Key** (Clave de certificado del proveedor del servidor), escriba el certificado y la clave. 

       Puede crear un certificado autofirmado mediante la ejecución de los siguientes comandos en la línea de comandos (Linux/Mac):

       - Comando para obtener la clave del certificado de SP: `openssl genrsa -out lr.hub.loginradius.com.key 2048`

       - Comando para obtener el certificado de SP: `openssl req -new -x509 -key lr.hub.loginradius.com.key -out lr.hub.loginradius.com.cert -days 3650 -subj /CN=lr.hub.loginradius.com`
     
       > [!NOTE]
       > Asegúrese de especificar los valores de certificado y de clave del certificado con el encabezado y el pie de página:
       > - Formato de ejemplo del valor de certificado: `-----BEGIN CERTIFICATE-----<certifciate value>-----END CERTIFICATE-----`
       > - Formato de ejemplo del valor de clave de certificado: `-----BEGIN RSA PRIVATE KEY-----<certifciate key value>-----END RSA PRIVATE KEY-----`

5. En la sección **Asignación de datos**, seleccione los campos (campos SP) y especifique los campos correspondientes (campos IdP) de Azure AD.

    A continuación, se enumeran algunos nombres de campo para Azure AD.

    | Fields    | Clave de perfil                                                          |
    | --------- | -------------------------------------------------------------------- |
    | Email     | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |
    | Nombre | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`    |
    | Apellidos  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`      |

    > [!NOTE]
    > La asignación de campos **Email** es obligatoria. Las asignaciones de campos **FirstName** y **LastName** son opcionales.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades de  **usuario**, siga estos pasos que se indican a continuación.

   ![Cuadro de diálogo Usuario](common/user-properties.png)

   1. En el campo **Nombre**, escriba **BrittaSimon**.
  
   1. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`. Por ejemplo, BrittaSimon@contoso.com.

   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.

   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a LoginRadius.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **LoginRadius**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **LoginRadius**.

    ![El vínculo de LoginRadius en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y seleccione **Usuarios y grupos** en el panel **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de **usuarios**. A continuación, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera algún valor de rol en la aserción de SAML, en el panel **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. A continuación, elija el botón **Seleccionar** situado en la parte inferior de la pantalla.

7. En el panel **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="create-loginradius-test-user"></a>Creación de un usuario de prueba de LoginRadius

1. Inicie sesión en su cuenta de la [consola de administración](https://adminconsole.loginradius.com/login) de LoginRadius.

2. Vaya a la sección de administración del equipo en la consola de administración de LoginRadius.

   ![Captura de pantalla que muestra la consola de administración de LoginRadius.](./media/loginradius-tutorial/team-management.png)
3. Seleccione **Agregar un miembro del equipo** en el menú lateral para abrir el formulario. 

4. En el formulario **Agregar un miembro del equipo**, creará un usuario llamado Britta Simon en el sitio de LoginRadius al proporcionar los detalles del usuario y asignar los permisos que desea para el usuario. Para obtener más información sobre los permisos basados en roles, consulte la sección de [permisos de acceso de rol](https://www.loginradius.com/docs/api/v2/admin-console/team-management/manage-team-members#roleaccesspermissions0) del documento de [administración de miembros del equipo](https://www.loginradius.com/docs/api/v2/admin-console/team-management/manage-team-members#roleaccesspermissions0). Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

1. En un explorador, vaya a https://accounts.loginradius.com/auth.aspx y seleccione **Fed SSO log in** (Inicio de sesión único Fed).
2. Escriba el nombre de la aplicación LoginRadius y, a continuación, seleccione **inicio de sesión**.
3. Debería abrirse una ventana emergente para solicitarle que inicie sesión en su cuenta de Azure AD.
4. Después de la autenticación, se cerrará la ventana emergente y se iniciará sesión en la consola de administración de LoginRadius.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)
