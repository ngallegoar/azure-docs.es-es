---
title: 'Tutorial: Integración de Azure Active Directory con SAP Cloud Platform Identity Authentication | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAP Cloud Platform Identity Authentication.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/28/2020
ms.author: jeedes
ms.openlocfilehash: fc5e0270c4533eaae1310dc84b616d675a79c8ea
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92895102"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-cloud-platform-identity-authentication"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SAP Cloud Platform Identity Authentication

En este tutorial, aprenderá a integrar SAP Cloud Platform Identity Authentication con Azure Active Directory (Azure AD). Al integrar SAP Cloud Platform Identity Authentication con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SAP Cloud Platform Identity Authentication.
* Puede permitir que los usuarios inicien sesión automáticamente en SAP Cloud Platform Identity Authentication con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en SAP Cloud Platform Identity Authentication.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SAP Cloud Platform Identity Authentication admite el inicio de sesión único iniciado por **SP** e **IDP**
* Una vez configurado SAP Cloud Platform Identity Authentication, puede aplicar controles de sesión, que protegen la filtración y la infiltración de la información confidencial de la organización en tiempo real. Los controles de sesión proceden del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

Antes de adentrarse en los detalles técnicos, es fundamental entender los conceptos vamos a tratar. Los Servicios de federación de Active Directory y SAP Cloud Platform Identity Authentication le permiten implementar el inicio de sesión único en aplicaciones o servicios protegidos por Azure AD (como un IdP) con aplicaciones y servicios de SAP protegidos por SAP Cloud Platform Identity Authentication.

Actualmente, SAP Cloud Platform Identity Authentication actúa como un proveedor de identidades de proxy en las aplicaciones de SAP. A su vez, Azure Active Directory funciona como el proveedor de identidades principal en esta configuración. 

En el siguiente diagrama, se ilustra esta relación:

![Creación de un usuario de prueba de Azure AD](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Con esta configuración, su inquilino de SAP Cloud Platform Identity Authentication se configura como una aplicación de confianza en Azure Active Directory.

Todos los servicios y las aplicaciones de SAP que desea proteger de esta manera se configuran posteriormente en la consola de administración de SAP Cloud Platform Identity Authentication.

Por tanto, la autorización de concesión de acceso a servicios y aplicaciones de SAP debe realizarse en SAP Cloud Platform Identity Authentication (en lugar de en Azure Active Directory).

Al configurar SAP Cloud Platform Identity Authentication como una aplicación a través de Azure Active Directory Marketplace, no necesita configurar notificaciones individuales o instrucciones de aserción de SAML.

> [!NOTE]
> Actualmente, el SSO web solo se ha probado en ambas soluciones. Los flujos que son necesarios para establecer comunicación de aplicación a API o de API a API deberían funcionar todavía no se han probado. Sin embargo, sí que se hará durante las actividades posteriores.

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Adición de SAP Cloud Platform Identity Authentication desde la galería

Para configurar la integración de SAP Cloud Platform Identity Authentication en Azure AD, debe agregar SAP Cloud Platform Identity Authentication desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory** .
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones** .
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación** .
1. En la sección **Agregar desde la Galería** , escriba **SAP Cloud Platform Identity Authentication** en el cuadro de búsqueda.
1. Selecciones **SAP Cloud Platform Identity Authentication** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-cloud-platform-identity-authentication"></a>Configuración y prueba del inicio de sesión único de Azure AD para SAP Cloud Platform Identity Authentication

Configure y pruebe el inicio de sesión único de Azure AD con SAP Cloud Platform Identity Authentication mediante un usuario de prueba llamado **B.Simon** . Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de SAP Cloud Platform Identity Authentication.

Para configurar y probar el inicio de sesión único de Azure AD con SAP Cloud Platform Identity Authentication, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en SAP Cloud Platform Identity Authentication](#configure-sap-cloud-platform-identity-authentication-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de SAP Cloud Platform Identity Authentication](#create-sap-cloud-platform-identity-authentication-test-user)** , para tener un homólogo de B.Simon en SAP Cloud Platform Identity Authentication vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **SAP Cloud Platform Identity Authentication** , busque la sección **Administrar** y seleccione **Inicio de sesión único** .
1. En la página **Seleccione un método de inicio de sesión único** , elija **SAML** .
1. En la página **Configurar el inicio de sesión único con SAML** , haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML** , si desea configurar en el modo iniciado por **IDP** , siga estos pasos:

    ![Captura de pantalla que muestra la sección "Configuración básica de S A M L" con los cuadros de texto "Identificador" y "Dirección U R L de respuesta" resaltados y el botón "Guardar" seleccionado.](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador** , escriba una dirección URL con el patrón siguiente: `<IAS-tenant-id>.accounts.ondemand.com`

    b. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de SAP Cloud Platform Identity Authentication](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) para obtener estos valores. Si no comprende el valor del identificador, consulte la documentación de SAP Cloud Platform Identity Authentication sobre la [configuración de inquilino de SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP** :

    ![Información de dominio y direcciones URL de inicio de sesión único de SAP Cloud Platform Identity Authentication](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión** , escriba una dirección URL con el siguiente patrón: `{YOUR BUSINESS APPLICATION URL}`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Use la dirección URL de inicio de sesión de su aplicación empresarial específica. Si tiene alguna duda, póngase en contacto con el [equipo de soporte técnico de SAP Cloud Platform Identity Authentication](https://cloudplatform.sap.com/capabilities/security/trustcenter.html).

1. La aplicación SAP Cloud Platform Identity Authentication espera que las aserciones de SAML estén en un formato específico, lo cual requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, SAP Cloud Platform Identity Authentication espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ---------------| --------------- |
    | firstName | user.givenname |

8. En la página **Configurar el inicio de sesión único con SAML** , en la sección **Certificado de firma de SAML** , haga clic en **Descargar** para descargar **XML de metadatos** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

9. En la sección **Set up SAP Cloud Platform Identity Authentication** (Configurar SAP Cloud Platform Identity Authentication), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory** , **Usuarios** y **Todos los usuarios** .
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario** , siga estos pasos:
   1. En el campo **Nombre** , escriba `B.Simon`.  
   1. En el campo **Nombre de usuario** , escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña** .
   1. Haga clic en **Crear** .

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección va a permitir que B.Simon acceda a SAP Cloud Platform Identity Authentication mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones** .
1. En la lista de aplicaciones, seleccione **SAP Cloud Platform Identity Authentication** .
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos** .

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario** . A continuación, en el cuadro de diálogo **Agregar asignación** , seleccione **Usuarios y grupos** .

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos** , seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** , seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación** , haga clic en el botón **Asignar** .

## <a name="configure-sap-cloud-platform-identity-authentication-sso"></a>Configuración del inicio de sesión único de SAP Cloud Platform Identity Authentication

1. Para configurar el SSO en la aplicación, vaya a la consola de administración de SAP Cloud Platform Identity Authentication. La dirección URL tiene el siguiente patrón: `https://<tenant-id>.accounts.ondemand.com/admin`. Después, lea la documentación sobre SAP Cloud Platform Identity Authentication en [Integration with Microsoft Azure AD](https://developers.sap.com/tutorials/cp-ias-azure-ad.html) (Integración con Microsoft Azure AD).

2. En Azure Portal, seleccione el botón **Guardar** .

3. Siga los pasos siguientes solo si desea agregar y habilitar SSO para otra aplicación de SAP. Repita los pasos de la sección **Adición de SAP Cloud Platform Identity Authentication desde la galería** .

4. En Azure Portal, en la página de integración de la aplicación **SAP Cloud Platform Identity Authentication** , seleccione **Inicio de sesión vinculado** .

    ![Configuración del inicio de sesión vinculado](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

5. Guarde la configuración.

> [!NOTE]
> La nueva aplicación aprovecha la configuración de inicio de sesión único para la aplicación de SAP anterior. Asegúrese de usar los mismos proveedores de identidades corporativos en la consola de administración de SAP Cloud Platform Identity Authentication.

### <a name="create-sap-cloud-platform-identity-authentication-test-user"></a>Creación del usuario de prueba de SAP Cloud Platform Identity Authentication

No hace falta crear un usuario en SAP Cloud Platform Identity Authentication. Los usuarios que están en el almacén de usuarios de Azure AD pueden utilizar la funcionalidad de inicio de sesión único (SSO).

SAP Cloud Platform Identity Authentication es compatible con la opción de federación de identidades. Esta opción permite a la aplicación comprobar si los usuarios autenticados mediante el proveedor de identidades corporativas se encuentran en el almacén de usuarios de SAP Cloud Platform Identity Authentication.

De manera predeterminada, la opción de federación de identidades está deshabilitada. Si se habilita, solo los usuarios que se importan de SAP Cloud Platform Identity Authentication pueden tener acceso a la aplicación.

Para más información sobre cómo habilitar o deshabilitar la federación de identidades con SAP Cloud Platform Identity Authentication, consulte la sección Enable Identity Federation with SAP Cloud Platform Identity Authentication (Activación de la federación de identidades con SAP Cloud Platform Identity Authentication) del artículo [Configure Identity Federation with the User Store of SAP Cloud Platform Identity Authentication](https://help.sap.com/viewer/6d6d63354d1242d185ab4830fc04feb1/Cloud/c029bbbaefbf4350af15115396ba14e2.html) (Configuración de la federación de identidades con el almacén de usuarios de SAP Cloud Platform Identity Authentication).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SAP Cloud Platform Identity Authentication del panel de acceso, debe iniciar sesión automáticamente en la aplicación SAP Cloud Platform Identity Authentication para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe SAP Cloud Platform Identity Authentication con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Protección de SAP Cloud Platform Identity Authentication con controles y visibilidad avanzados](/cloud-app-security/proxy-intro-aad)