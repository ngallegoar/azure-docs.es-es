---
title: 'Tutorial: Integración de Azure Active Directory con Atlassian Cloud | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Atlassian Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: celested
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a3a54b096019e9e38bc800ae313016a430062dab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74964334"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>Tutorial: Integración de Atlassian Cloud con Azure Active Directory

En este tutorial, aprenderá a integrar Atlassian Cloud con Azure Active Directory (Azure AD). Al integrar Atlassian Cloud con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Atlassian Cloud.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Atlassian Cloud con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Atlassian Cloud.
* Para habilitar el inicio de sesión único en Lenguaje de marcado de aserción de seguridad (SAML) para productos de Atlassian Cloud, debe configurar Atlassian Access. Más información sobre [Atlassian Access]( https://www.atlassian.com/enterprise/cloud/identity-manager).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. 

* Atlassian Cloud admite SSO iniciado por **SP e IDP**
* Atlassian Cloud admite el [aprovisionamiento y desaprovisionamiento automático de usuarios](atlassian-cloud-provisioning-tutorial.md).

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Incorporación de Atlassian Cloud desde la galería

Para configurar la integración de Atlassian Cloud en Azure AD, necesita agregar Atlassian Cloud desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Atlassian Cloud** en el cuadro de búsqueda.
1. Seleccione **Atlassian Cloud** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Atlassian Cloud utilizando un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Atlassian Cloud.

Para configurar y probar el inicio de sesión único de Azure AD con Atlassian Cloud, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Atlassian Cloud](#configure-atlassian-cloud-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba en Atlassian Cloud](#create-atlassian-cloud-test-user)** , para tener un homólogo de B.Simon en Atlassian Cloud que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Atlassian Cloud**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://auth.atlassian.com/saml/<unique ID>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Haga clic en **Establecer direcciones URL adicionales**.

    d. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL que siga este patrón: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Los valores anteriores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Puede obtener estos valores reales en la pantalla de configuración de **SAML de Atlassian Cloud**, que se explica más adelante en el tutorial **Configuración del inicio de sesión único de Atlassian Cloud**.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Pegue el valor de la instancia que quiere usar para iniciar sesión en el portal de administración de Atlassian Cloud.

    ![Configurar inicio de sesión único](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-10.png)

1. La aplicación Atlassian Cloud espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación Atlassian Cloud espera que **nameidentifier** se asigne con **user.mail**, por lo que debe editar la asignación de atributos haciendo clic en el icono **Editar** y cambiar dicha asignación.

    ![imagen](common/edit-attribute.png)

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Atlassian Cloud**, copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

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

En esta sección, va a conceder acceso a B.Simon a Atlassian Cloud para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Atlassian Cloud**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-atlassian-cloud-sso"></a>Configuración del inicio de sesión único de Atlassian Cloud

1. Para automatizar la configuración en Atlassian Cloud, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Install the extension** (Instalar la extensión).

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

1. Después de agregar la extensión al explorador, haga clic en **Setup Atlassian Cloud** (Configurar Atlassian Cloud) para ir a la aplicación Atlassian Cloud. Desde allí, proporcione las credenciales de administrador para iniciar sesión en Atlassian Cloud. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 7.

    ![Configuración](common/setup-sso.png)

1. Si quiere configurar Atlassian Cloud manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de Atlassian Cloud como administrador y realice los pasos siguientes:

1. Debe comprobar el dominio antes de configurar el inicio de sesión único. Para más información, consulte el documento [Atlassian domain verification](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) (Comprobación del dominio Atlassian).

1. En el panel de la izquierda, seleccione **Seguridad** > **Inicio de sesión único de SAML**. Si no lo ha hecho todavía, suscríbase a Identity Manager de Atlassian.

    ![Configurar inicio de sesión único](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-11.png)

1. En la ventana **Agregar configuración de SAML**, haga lo siguiente:

    ![Configurar inicio de sesión único](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-12.png)

    a. En el cuadro **Identity provider Entity ID** (Id. de entidad del proveedor de identidades), pegue el **identificador de Azure AD** que copió de Azure Portal.

    b. En el cuadro **Identity provider SSO URL** (URL de inicio de sesión único del proveedor de identidades), pegue la **dirección URL de inicio de sesión** que copió de Azure Portal.

    c. Abra el certificado descargado de Azure Portal en un archivo .txt, copie el valor (sin las líneas *Begin Certificate* (Inicio del certificado) ni *End Certificate* (Fin del certificado)) y, después, péguelo en el cuadro **Certificado Public X509**.

    d. Haga clic en **Guardar configuración**.

1. Para asegurarse de que ha configurado las direcciones URL correctas, actualice la configuración de Azure AD mediante los pasos siguientes:

    ![Configurar inicio de sesión único](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-13.png)

    a. En la ventana SAML, copie el valor de **Id. de identidad de SP** y, después, en Azure Portal, en la sección **Configuración básica de SAML** de Atlassian Cloud, péguelo en el cuadro **Identificador**.

    b. En la ventana SAML, copie el valor de **SP Assertion Consumer Service URL** (URL del Servicio de consumidor de aserciones de SP) y, después, en Azure Portal, en la sección **Configuración básica de SAML** de Atlassian Cloud, péguelo en el cuadro **URL de respuesta**. La dirección URL de inicio de sesión es la dirección URL del inquilino de Atlassian Cloud.

    > [!NOTE]
    > Si es un cliente existente, después de actualizar los valores de **Id. de identidad de SP** y de **SP Assertion Consumer Service URL** (URL del Servicio de consumidor de aserciones de SP) en Azure Portal, seleccione **Yes, update configuration** (Sí, actualizar configuración). Si es un cliente nuevo, puede omitir este paso.

### <a name="create-atlassian-cloud-test-user"></a>Creación de un usuario de prueba de Atlassian Cloud

Para permitir que los usuarios de Azure AD inicien sesión en Atlassian Cloud, aprovisione las cuentas de usuario manualmente en Atlassian Cloud haciendo lo siguiente:

1. En el panel **Administración**, seleccione **Usuarios**.

    ![Vínculo de usuarios de Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

1. Para crear un usuario en Atlassian Cloud, seleccione **Invitar usuario**.

    ![Creación de un usuario de Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

1. En el cuadro **Dirección de correo electrónico**, escriba la dirección de correo electrónico del usuario y asigne el acceso a la aplicación.

    ![Creación de un usuario de Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

1. Para enviar una invitación por correo electrónico al usuario, seleccione **Invitar a usuarios**. Se envía una invitación por correo electrónico al usuario y, después de aceptar la invitación, el usuario se activa en el sistema.

> [!NOTE]
> También puede crear usuarios de forma masiva mediante la selección del botón **Bulk Create** (Crear de forma masiva) en la sección **Usuarios**.

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de Atlassian Cloud en el panel de acceso y debería iniciar sesión automáticamente en la versión de Atlassian Cloud para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe Atlassian Cloud con Azure AD](https://aad.portal.azure.com/)
