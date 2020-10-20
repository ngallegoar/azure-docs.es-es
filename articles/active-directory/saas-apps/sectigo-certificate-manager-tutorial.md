---
title: 'Tutorial: Integración de Azure Active Directory con Sectigo Certificate Manager | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Sectigo Certificate Manager.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.openlocfilehash: 905ca5fd92a09b209bf099bfac0862132ec679a4
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91875616"
---
# <a name="tutorial-azure-active-directory-integration-with-sectigo-certificate-manager"></a>Tutorial: Integración de Azure Active Directory con Sectigo Certificate Manager

En este tutorial, aprenderá a integrar Sectigo Certificate Manager (también llamado SCM) con Azure Active Directory (Azure AD).

La integración de Sectigo Certificate Manager con Azure AD proporciona las siguientes ventajas:

* Puede usar Azure AD para controlar quién tiene acceso a Sectigo Certificate Manager.
* Los usuarios pueden iniciar sesión automáticamente en Sectigo Certificate Manager (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información acerca de la integración de aplicaciones SaaS (software como servicio) con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con Sectigo Certificate Manager, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción de Azure AD, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una cuenta de Sectigo Certificate Manager.

> [!NOTE]
> Sectigo ejecuta varias instancias de Sectigo Certificate Manager. La instancia principal de Sectigo Certificate Manager es **https:\//cert-manager.com** y esta es la dirección URL que se usa en este tutorial.  Si su cuenta se encuentra en otra instancia, debe ajustar las direcciones URL en consecuencia.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba e integrar Sectigo Certificate Manager con Azure AD.

Sectigo Certificate Manager admite las características siguientes:

* **Inicio de sesión único iniciado por SP**
* **Inicio de sesión único iniciado por IDP**

## <a name="add-sectigo-certificate-manager-in-the-azure-portal"></a>Adición de Sectigo Certificate Manager en Azure Portal

Para integrar Sectigo Certificate Manager con Azure AD, debe agregar Sectigo Certificate Manager a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Azure Active Directory** en el menú izquierdo.

    ![Opción de Azure Active Directory](common/select-azuread.png)

1. Seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Panel Aplicaciones empresariales](common/enterprise-applications.png)

1. Para agregar una aplicación, seleccione **Nueva aplicación**.

    ![Opción Nueva aplicación](common/add-new-app.png)

1. En el cuadro de búsqueda, escriba **Sectigo Certificate Manager**. En los resultados de búsqueda, seleccione **Sectigo Certificate Manager** y, a continuación, seleccione **Agregar**.

    ![Sectigo Certificate Manager en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Sectigo Certificate Manager con un usuario de prueba llamado **Britta Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Sectigo Certificate Manager.

Para configurar y probar el inicio de sesión único de Azure AD con Sectigo Certificate Manager, debe completar los siguientes bloques de creación:

| Tarea | Descripción |
| --- | --- |
| **[Configuración del inicio de sesión único en Azure AD](#configure-azure-ad-single-sign-on)** | Permite que los usuarios usen esta característica. |
| **[Configuración del inicio de sesión único en Sectigo Certificate Manager](#configure-sectigo-certificate-manager-single-sign-on)** | Configura los valores de inicio de sesión único en la aplicación. |
| **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** | Prueba el inicio de sesión único de Azure AD con el usuario Britta Simon. |
| **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** | Permite que Britta Simon use el inicio de sesión único de Azure AD. |
| **[Creación de un usuario de prueba de Sectigo Certificate Manager](#create-a-sectigo-certificate-manager-test-user)** | Crea un homólogo de Britta Simon en Sectigo Certificate Manager que está vinculado a la representación del usuario en Azure AD. |
| **[Prueba de inicio de sesión único](#test-single-sign-on)** | Comprueba que la configuración funciona. |

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, configurará el inicio de sesión único de Azure AD con Sectigo Certificate Manager en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en el panel de integración de aplicaciones de **Sectigo Certificate Manager**, haga clic en **Inicio de sesión único**.

    ![Configuración de la opción de inicio de sesión único](common/select-sso.png)

1. En el panel **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** o **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

1. En el panel **Configurar el inicio de sesión único con SAML**, seleccione **Editar** (icono de lápiz) para abrir el panel **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    1. En el cuadro de texto **Identificador (id. de entidad)** , para la instancia principal de Sectigo Certificate Manager, escriba **https:\//cert-manager.com/shibboleth**.

    1. En el cuadro de texto **Dirección URL de respuesta**, para la instancia principal de Sectigo Certificate Manager, escriba **https:\//cert-manager.com/Shibboleth.sso/SAML2/POST**.
        
    > [!NOTE]
    > Aunque en general la **dirección URL de inicio de sesión** es obligatoria para el *modo Iniciado por SP*, no es necesario para iniciar sesión en Sectigo Certificate Manager.        

1. Opcionalmente, en la sección **Configuración básica de SAML**, para configurar el *modo iniciado por IDP* y para que la opción **Probar** funcione, siga estos pasos:

    1. Seleccione **Establecer direcciones URL adicionales**.

    1. En el cuadro de texto **Estado de la retransmisión**, escriba su dirección URL específica de cliente de Sectigo Certificate Manager. Para la instancia principal de Sectigo Certificate Manager, escriba **https:\//cert-manager.com/customer/\<customerURI\>/idp**.

    ![Información de direcciones URL de inicio de sesión único y dominio de Sectigo Certificate Manager](common/idp-relay.png)

1. En la sección **Atributos y notificaciones de usuario**, realice estos pasos:

    1. Elimine todas las **notificaciones adicionales**.
    
    1. Seleccione **Agregar nueva notificación** y agregue las cuatro notificaciones siguientes:
    
        | Nombre | Espacio de nombres | Source | Atributo de origen | Descripción |
        | --- | --- | --- | --- | --- |
        | eduPersonPrincipalName | empty | Atributo | user.userprincipalname | Debe coincidir con el campo **IdP Person ID** de Sectigo Certificate Manager para administradores. |
        | mail | empty | Atributo | user.mail | Requerido |
        | givenName | empty | Atributo | user.givenname | Opcional |
        | sn | empty | Atributo | user.surname | Opcional |

       ![Sectigo Certificate Manager: agregar cuatro nuevas notificaciones](media/sectigo-certificate-manager-tutorial/additional-claims.png)

1. En la sección **Certificado de firma de SAML**, al lado de **XML de metadatos de federación**, seleccione **Descargar**. Guarde el archivo XML en el equipo.

    ![Opción de descarga del XML de metadatos de federación](common/metadataxml.png)

### <a name="configure-sectigo-certificate-manager-single-sign-on"></a>Configuración del inicio de sesión único en Sectigo Certificate Manager

Para configurar el inicio de sesión único en Sectigo Certificate Manager, envíe el archivo XML de metadatos de federación descargado al [equipo de soporte técnico de Sectigo Certificate Manager](https://sectigo.com/support). El equipo de soporte técnico de Sectigo Certificate Manager usa la información que le envíe para asegurarse de que la conexión de inicio de sesión único de SAML está configurada correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

En esta sección, creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.

    ![Opciones Usuarios y Todos los usuarios](common/users.png)

1. Seleccione **Nuevo usuario**.

    ![Opción Nuevo usuario](common/new-user.png)

1. En el panel **Usuario**, siga estos pasos:

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.
  
    1. En el cuadro **Nombre de usuario**, escriba **brittasimon\@\<your-company-domain>.\<extension\>** . Por ejemplo, **brittasimon\@contoso.com**.

    1. Active la casilla de verificación **Mostrar contraseña**. Anote el valor que se muestra en el cuadro **Contraseña**.

    1. Seleccione **Crear**.

    ![Panel Usuario](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a conceder acceso a Britta Simon a Sectigo Certificate Manager para que pueda usar el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones** > **Sectigo Certificate Manager**.

    ![Panel Aplicaciones empresariales](common/enterprise-applications.png)

1. En la lista de aplicaciones, seleccione **Sectigo Certificate Manager**.

    ![Sectigo Certificate Manager en la lista de aplicaciones](common/all-applications.png)

1. En el menú, seleccione **Usuarios y grupos**.

    ![Opción Usuarios y grupos](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. Después, en el panel **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![El panel Agregar asignación](common/add-assign-user.png)

1. En el panel **Usuarios y grupos**, en la lista de usuarios, seleccione **Britta Simon**. Elija **Seleccionar**.

1. Si espera algún valor de rol en la aserción de SAML, en el panel **Seleccionar rol**, seleccione el rol adecuado para el usuario de la lista. Elija **Seleccionar**.

1. En el panel **Agregar asignación**, seleccione **Asignar**.

### <a name="create-a-sectigo-certificate-manager-test-user"></a>Creación de un usuario de prueba de Sectigo Certificate Manager

En esta sección, se crea un usuario llamado Britta Simon en Sectigo Certificate Manager. Trabaje con el [equipo de soporte técnico de Sectigo Certificate Manager](https://sectigo.com/support) para agregar a los usuarios en la plataforma de Sectigo Certificate Manager. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD.

#### <a name="test-from-sectigo-certificate-manager-sp-initiated-single-sign-on"></a>Prueba desde Sectigo Certificate Manager (inicio de sesión único iniciado por SP)

Vaya a la dirección URL específica de cliente para la instancia principal de Sectigo Certificate Manager, https:\//cert-manager.com/customer/\<customerURI\>/ y seleccione el botón que hay debajo de **Or Sign In With** (O bien, inicie sesión con).  Si se ha configurado correctamente, iniciará sesión automáticamente en Sectigo Certificate Manager.

#### <a name="test-from-azure-single-sign-on-configuration-idp-initiated-single-sign-on"></a>Prueba desde la configuración de inicio de sesión único de Azure (inicio de sesión único iniciado por IDP)

En el panel de integración de la aplicación **Sectigo Certificate Manager**, seleccione **Inicio de sesión único** y seleccione el botón **Probar**.  Si se ha configurado correctamente, iniciará sesión automáticamente en Sectigo Certificate Manager.

#### <a name="test-by-using-the-my-apps-portal-idp-initiated-single-sign-on"></a>Prueba mediante el portal Aplicaciones (inicio de sesión único iniciado por IDP)

Seleccione **Sectigo Certificate Manager** en el portal Aplicaciones.  Si se ha configurado correctamente, iniciará sesión automáticamente en Sectigo Certificate Manager. Para más información acerca del portal Aplicaciones, consulte [Acceso y uso de aplicaciones en el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte estos artículos:

- [Lista de tutoriales para integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Inicio de sesión único en aplicaciones en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


