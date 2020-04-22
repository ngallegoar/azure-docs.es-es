---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Trend Micro Web Security (TMWS) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Trend Micro Web Security (TMWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 827285d3-8e65-43cd-8453-baeda32ef174
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a4c2cddbc9086c80922fcf9c5d96cd197ab4778
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425284"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-trend-micro-web-securitytmws"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Trend Micro Web Security (TMWS)

En este tutorial aprenderá a integrar Trend Micro Web Security (TMWS) con Azure Active Directory (Azure AD). Al integrar Trend Micro Web Security (TMWS) con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Trend Micro Web Security (TMWS).
* Permitir que los usuarios inicien sesión automáticamente en Trend Micro Web Security (TMWS) con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Trend Micro Web Security (TMWS).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Trend Micro Web Security (TMWS) admite el inicio de sesión único iniciado por **SP**.
* Una vez configurado Trend Micro Web Security (TMWS), puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-trend-micro-web-securitytmws-from-the-gallery"></a>Incorporación de Trend Micro Web Security (TMWS) desde la galería

Para configurar la integración de Trend Micro Web Security (TMWS) en Azure AD, es preciso agregar Trend Micro Web Security (TMWS) desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Trend Micro Web Security (TMWS)** en el cuadro de búsqueda.
1. Seleccione **Trend Micro Web Security (TMWS)** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-trend-micro-web-securitytmws"></a>Configuración y prueba del inicio de sesión único de Azure AD para Trend Micro Web Security (TMWS)

Configure y pruebe el inicio de sesión único de Azure AD con Trend Micro Web Security (TMWS) mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Trend Micro Web Security (TMWS).

Para configurar y probar el inicio de sesión único de Azure AD con Trend Micro Web Security (TMWS), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
    1. **[Configuración de las opciones de sincronización de usuarios y grupos en Azure AD](#configure-user-and-group-synchronization-settings-in-azure-ad)** , para configurar la sincronización de usuarios y grupos en Azure AD.
1. **[Configuración del inicio de sesión único de Trend Micro Web Security (TMWS)](#configure-trend-micro-web-security-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Trend Micro Web Security (TMWS)** , busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://auth.iws-hybrid.trendmicro.com/([0-9a-f]{16})`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL: `https://auth.iws-hybrid.trendmicro.com/simplesaml/module.php/saml/sp/saml2-acs.php/ics-sp`

    > [!NOTE]
    > El valor del identificador no es real. Actualícelo con el identificador real. Póngase en contacto con el [equipo de soporte técnico del cliente de Trend Micro Web Security (TMWS)](https://success.trendmicro.com/contact-support-north-america) para obtener el valor del identificador. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación Trend Micro Web Security (TMWS) espera las aserciones de SAML en un formato específico, lo cual requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación Trend Micro Web Security (TMWS) espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.
    
    | Nombre | Atributo de origen|
    | --------------- | --------- |
    | sAMAccountName | user.onpremisessamaccountname |
    | uPN | user.userprincipalname |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Trend Micro Web Security (TMWS)** , copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

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

En esta sección va a permitir que B.Simon acceda a Trend Micro Web Security (TMWS) mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Trend Micro Web Security (TMWS)** .
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="configure-user-and-group-synchronization-settings-in-azure-ad"></a>Configuración de las opciones de sincronización de usuarios y grupos en Azure AD

1. En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

1. En **Administrar**, haga clic en **Registros de aplicaciones** y, a continuación, haga clic en la nueva aplicación empresarial en el área **Todas las aplicaciones**.

1. En **Administrar**, haga clic en **Certificados y secretos**.

1. En el área de secretos de cliente que aparece, haga clic en **Nuevo secreto de cliente**.

1. En la pantalla Add a client secret (Agregar un secreto de cliente) que aparece puede agregar una descripción y seleccionar un período de expiración para este secreto de cliente; a continuación, haga clic en **Agregar**. En el área de secretos de cliente aparece el secreto de cliente recién agregado.

1. Tome nota del valor. Más adelante, escribirá la información en TMWS.

1. En **Administrar**, seleccione **Permisos de API**. 

1. En la pantalla de permisos de API que aparece, haga clic en **Agregar un permiso**.

1. En la pestaña API de Microsoft de la pantalla Solicitud de permisos de API que aparece, haga clic en **Microsoft Graph** y, a continuación, en **Permisos de la aplicación**.

1. Localice y agregue los siguientes permisos: 

    * Group.Read.All
    * User.Read.All

1. Haga clic en **Agregar permisos**. Aparece un mensaje para confirmar que la configuración se guardó correctamente. Los permisos recién agregados aparecen en la pantalla Permisos de API.

1. En el área Otorgar consentimiento, haga clic en **Conceder consentimiento de administrador para <la cuenta de administrador> (directorio predeterminado)** y, a continuación, en **Sí**. Aparece un mensaje para confirmar que se ha otorgado correctamente el consentimiento del administrador para los permisos solicitados.

1. Haga clic en **Descripción general**. 

1. En el panel derecho que aparece, tome nota del identificador de la aplicación (cliente) y el identificador de directorio (inquilino). Más adelante, escribirá la información en TMWS. También puede hacer clic en **Nombres de dominio personalizados** en Azure **Active Directory > Administrar** y tomar nota del nombre de dominio en el panel derecho.

## <a name="configure-trend-micro-web-security-sso"></a>Configuración del inicio de sesión único de Trend Micro Web Security (TMWS)

Para configurar el inicio de sesión único en **Trend Micro Web Security (TMWS)** , debe enviar el **certificado (Base64)** descargado y las direcciones URL correspondientes copiadas de Azure Portal al [equipo de soporte técnico de Trend Micro Web Security (TMWS)](https://success.trendmicro.com/contact-support-north-america). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

## <a name="test-sso"></a>Prueba de SSO 

Una vez que haya configurado correctamente el servicio Azure AD y haya especificado Azure AD como método de autenticación de usuarios, puede iniciar sesión en el servidor proxy de TMWS para verificar la configuración. Después de que el inicio de sesión de Azure AD verifique la cuenta, puede visitar Internet.

> [!NOTE]
> TMWS no admite la prueba del inicio de sesión único desde el portal de Azure AD, en Información general > Inicio de sesión único > Configuración del inicio de sesión único con SAML > Prueba de la nueva aplicación empresarial.

1. Borre todas las cookies del explorador y, a continuación, reinícielo. 

1. Apunte el explorador al servidor proxy de TMWS. Para más información, consulte [Reenvío de tráfico mediante archivos PAC](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/pac-files/traffic-forwarding-u.aspx#GUID-A4A83827-7A29-4596-B866-01ACCEDCC36B).

1. Visite cualquier sitio web de Internet. TMWS le dirigirá al portal cautivo de TMWS.

1. Especifique una cuenta de Active Directory (formato: domain\sAMAccountName o sAMAccountName@domain), una dirección de correo electrónico o un UPN y, a continuación, haga clic en **Log On** (Iniciar sesión). TMWS le envía al inicio de sesión de Azure AD.

1. En el inicio de sesión de Azure AD, escriba sus credenciales de cuenta de AD. Debe iniciar sesión correctamente en TMWS.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Pruebe Trend Micro Web Security (TMWS) con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Protección de Trend Micro Web Security (TMWS) con controles y visibilidad avanzados](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

