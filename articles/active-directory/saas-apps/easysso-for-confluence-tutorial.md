---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con EasySSO for Confluence | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y EasySSO for Confluence.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 959019be-c7f6-41cc-b0ff-c5927ce64a74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/18/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a0ac6d86c1ef4fb8f2f5b75ac47f1a25e2cda0b
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83748177"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-confluence"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con EasySSO for Confluence

En este tutorial aprenderá a integrar EasySSO for Confluence con Azure Active Directory (Azure AD). Al integrar EasySSO for Confluence con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a EasySSO for Confluence.
* Permitir que los usuarios inicien sesión automáticamente en EasySSO for Confluence con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción de EasySSO for Confluence con el inicio de sesión único habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* EasySSO for Confluence admite el inicio de sesión único iniciado por **SP e IDP**.
* EasySSO for Confluence admite el aprovisionamiento de usuarios **Just-In-Time**.
* Una vez configurado EasySSO for Confluence, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-easysso-for-confluence-from-the-gallery"></a>Incorporación de EasySSO for Confluence desde la galería

Para configurar la integración de EasySSO for Confluence en Azure AD, tiene que agregar EasySSO for Confluence desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **EasySSO for Confluence** en el cuadro de búsqueda.
1. Seleccione **EasySSO for Confluence** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-easysso-for-confluence"></a>Configuración y prueba del inicio de sesión único de Azure AD para EasySSO for Confluence

Configure y pruebe el inicio de sesión único de Azure AD con EasySSO for Confluence mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de EasySSO for Confluence.

Para configurar y probar el inicio de sesión único de Azure AD con EasySSO for Confluence, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en EasySSO for Confluence](#configure-easysso-for-confluence-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de EasySSO for Confluence](#create-easysso-for-confluence-test-user)** , para tener un homólogo de B.Simon en EasySSO for Confluence vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **EasySSO for Confluence**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<server-base-url>/plugins/servlet/easysso/saml`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/easysso/saml`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de EasySSO for Confluence](mailto:support@techtime.co.nz) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación EasySSO for Confluence espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación EasySSO for Confluence espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre |  Atributo de origen|
    | --------------- | --------- |
    | urn:oid:2.16.840.1.113730.3.1.241 | user.displayname |
    | urn:oid:2.5.4.42 | user.givenname |
    | urn:oid:2.5.4.4 | user.surname |
    | urn:oid:0.9.2342.19200300.100.1.1 | user.userprincipalname|
    | urn:oid:0.9.2342.19200300.100.1.3 | user.userprincipalname |
    | | |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

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

En esta sección va a permitir que B.Simon acceda a EasySSO for Confluence mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **EasySSO for Confluence**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-easysso-for-confluence-sso"></a>Configuración del inicio de sesión único en EasySSO for Confluence

1. Inicie sesión en la instancia de EasySSO for Confluence con privilegios de administrador y vaya a la sección **Manage Apps** (Administrar aplicaciones).

    ![Administración de aplicaciones](./media/easysso-for-confluence-tutorial/jira-admin-1.png)

1. Haga clic en **EasySSO**.

    ![EasySSO](./media/easysso-for-confluence-tutorial/jira-admin-2.png)

1. Seleccione la opción **SAML**. Esto le llevará a la sección de configuración de SAML.

    ![SAML](./media/easysso-for-confluence-tutorial/jira-admin-3.png)

1. Seleccione la pestaña **Certificates** (Certificados) en la parte superior y aparecerá la siguiente pantalla. Busque el **certificado (Base 64)** o el **archivo de metadatos** que guardó en los pasos anteriores de configuración del **inicio de sesión único de Azure AD**. Dispone de las siguientes opciones para continuar:

    ![URL de metadatos](./media/easysso-for-confluence-tutorial/jira-admin-4.png)

    a. Use el **archivo de metadatos** de federación de la aplicación que descargó en un archivo local del equipo. Seleccione el botón de radio **Upload** (Cargar) y siga el cuadro de diálogo de carga de archivos específico del sistema operativo.

    **OR**

    b. Abra el **archivo de metadatos** de federación de la aplicación para ver el contenido del archivo (en cualquier editor de texto sin formato) y cópielo en el Portapapeles. Seleccione la opción **Input** (Entrada) y pegue el contenido del Portapapeles en el campo de texto.

    **OR**

    c. Configuración totalmente manual. Abra el **certificado (Base 64)** de federación de la aplicación para ver el contenido del archivo (en cualquier editor de texto sin formato) y cópielo en el Portapapeles. Péguelo en el campo de texto **IdP Token Signing Certificates** (Certificado de firma de tokens del IdP). A continuación, vaya a la pestaña **General** y rellene los campos **POST Binding URL** (Dirección URL de enlace POST) y **Entity ID** (Identificador de entidad) con los valores respectivos de **Dirección URL de inicio de sesión** e **Identificador de Azure AD** que guardó anteriormente.

1. Haga clic en el botón **Save** (Guardar) en la parte inferior de la página. Verá que el contenido de los archivos de metadatos o del certificado se analiza en los campos de configuración. Se ha completado la configuración de EasySSO for Confluence.

1. Para obtener la mejor experiencia de prueba, vaya a la pestaña **Look & Feel** (Apariencia y aspecto) y active la opción **SAML Login Button** (Botón de inicio de sesión de SAML). Esto habilitará un botón independiente en la pantalla de inicio de sesión de Jira para probar específicamente la integración de SAML de Azure AD de un extremo a otro. También puede dejar este botón activo y configurar su selección de ubicación, color y traducción para el modo de producción.

    ![Look & Feel (Apariencia y aspecto)](./media/easysso-for-confluence-tutorial/jira-admin-5.png)

    > [!NOTE]
    > Si tiene algún problema, póngase en contacto con el [equipo de soporte técnico de EasySSO](mailto:support@techtime.co.nz).

### <a name="create-easysso-for-confluence-test-user"></a>Creación de un usuario de prueba de EasySSO for Confluence

En esta sección, se crea un usuario llamado B.Simon en EasySSO for Confluence. EasySSO for Confluence admite el aprovisionamiento de usuarios Just-In-Time, que está **deshabilitado** de forma predeterminada. Para habilitar el aprovisionamiento de usuarios, debe marcar explícitamente la opción **Create user on successful login** (Crear usuario después de un inicio de sesión correcto) en la sección General de la configuración del complemento de EasySSO. Si el usuario no existe aún en EasySSO for Confluence, se crea después de la autenticación.

Sin embargo, si no desea habilitar el aprovisionamiento automático de usuarios en el primer inicio de sesión del usuario, los usuarios deben existir en los directorios de usuarios de back-end para que la instancia de EasySSO for Confluence los utilice, como LDAP o Atlassian Crowd.

![Aprovisionamiento de usuarios](./media/easysso-for-confluence-tutorial/jira-admin-6.png)

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de EasySSO for Confluence en el panel de acceso, debería iniciar sesión automáticamente en la versión de EasySSO for Confluence para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe EasySSO for Confluence con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Protección de EasySSO for Confluence con controles y visibilidad avanzados](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

