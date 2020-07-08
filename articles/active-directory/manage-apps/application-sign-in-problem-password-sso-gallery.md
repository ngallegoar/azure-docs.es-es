---
title: Problemas al iniciar sesión en una aplicación de la galería de Azure AD configurada para inicio de sesión único | Microsoft Docs
description: Cómo solucionar incidencias relacionadas con aplicaciones de la galería de Azure AD configuradas para inicio de sesión único con contraseña.
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6eebde3a7c6163b7faf92be193fe442cd5b74d2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84759120"
---
# <a name="sign-in-problems-with-an-azure-ad-gallery-app-configured-for-sso"></a>Problemas de información de inicio de sesión con una aplicación de galería de Azure AD configurada para inicio de sesión único

El panel de acceso es un portal basado en la web. Habilita a los usuarios que tienen cuentas profesionales o educativas de Azure Active Directory (Azure AD) el acceso a las aplicaciones basadas en la nube a las que tienen permisos. Los usuarios que tienen ediciones de Azure AD también pueden usar las capacidades de administración de grupos de autoservicio y aplicaciones a través del Panel de acceso.

El Panel de acceso es independiente de Azure Portal. Los usuarios no necesitan una suscripción de Azure para usar el Panel de acceso.

Para usar el inicio de sesión único (SSO) basado en contraseña en el Panel de acceso, se debe instalar la extensión del Panel de acceso en el explorador. Esta extensión se descarga automáticamente cuando se selecciona una aplicación que está configurada para el inicio de sesión único basado en contraseña.

## <a name="browser-requirements-for-access-panel"></a>Requisitos del explorador para el Panel de acceso

El Panel de acceso requiere un explorador compatible con JavaScript y que tenga habilitado CSS.

Los siguientes exploradores admiten el inicio de sesión único basado en contraseña:

- Internet Explorer 8, 9, 10 y 11 en Windows 7 o versiones posteriores

- Chrome en Windows 7 o versiones posteriores y en MacOS X o versiones posteriores

- Firefox 26.0 o versiones posteriores en Windows XP SP2 o versiones posteriores y en Mac OS X 10.6 o versiones posteriores

>[!NOTE]
>La extensión de SSO basado en contraseña está disponible para Microsoft Edge en Windows 10 desde que se admitieron las extensiones de explorador para Microsoft Edge.

## <a name="install-the-access-panel-browser-extension"></a>Instalación de la extensión de explorador del Panel de acceso

Siga estos pasos:

1. Abra [Panel de acceso](https://myapps.microsoft.com) en un explorador compatible e inicie sesión como usuario de Azure AD.

2. Seleccione una aplicación habilitada de inicio de sesión único con contraseña en el Panel de acceso.

3. Cuando se le pida, seleccione **Instalar ahora**.

4. Se le dirigirá al vínculo de descarga en función del explorador. Seleccione **Agregar** para instalar la extensión de explorador.

5. Si se le pide, seleccione **Habilitar** o **Permitir**.

6. Después de la instalación, reinicie el explorador.

7.  Inicie sesión en el Panel de acceso y vea si puede iniciar las aplicaciones habilitadas de inicio de sesión único con contraseña.

También puede descargar directamente las extensiones para Chrome y Firefox a través de estos vínculos:

-   [Extensión del Panel de acceso para Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Extensión del panel de acceso para Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Configuración de una directiva de grupo para Internet Explorer

Puede configurar una directiva de grupo que le permita instalar de forma remota la extensión del Panel de acceso para Internet Explorer en las máquinas de los usuarios.

Estos son los requisitos previos:

-   Se debe configurar [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx) y se deben unir las máquinas de los usuarios a su dominio.

-   Debe tener el permiso "Editar configuración" para editar el objeto de directiva de grupo (GPO). De forma predeterminada, los miembros de los siguientes grupos de seguridad tienen este permiso: administradores de dominio, administradores de empresa y propietarios del creador de directivas de grupo. [Más información](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Vea [Implementación de la extensión del Panel de acceso para Internet Explorer mediante la directiva de grupo](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) para configurar la directiva de grupo e implementarla en los usuarios.

## <a name="troubleshoot-access-panel-in-internet-explorer"></a>Solución de problemas del Panel de acceso en Internet Explorer

Vea [Solución de problemas de la extensión del Panel de acceso para Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting) para acceder a la herramienta de diagnóstico y a las instrucciones sobre cómo configurar la extensión.

## <a name="configure-password-sso-for-an-azure-ad-gallery-app"></a>Configuración del inicio de sesión único con contraseña para una aplicación de la galería de Azure AD

Para configurar una aplicación desde la galería de Azure AD, necesita realizar lo siguiente:

-   Agregar la aplicación desde la galería de Azure AD
-   [Configurar la aplicación para el inicio de sesión único con contraseña](#configure-the-app-for-password-sso)
-   [Asignar usuarios a la aplicación](#assign-users-to-the-app)

### <a name="add-the-app-from-the-azure-ad-gallery"></a>Agregar la aplicación desde la galería de Azure AD

Siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com) e inicie sesión como administrador global o coadministrador.

2. Seleccione **Todos los servicios** en la parte superior del panel del lado izquierdo para abrir la extensión de Azure AD.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda de filtrado y, luego, seleccione **Azure Active Directory**.

4. En el panel de navegación de Azure AD, seleccione **Aplicaciones empresariales**.

5. Seleccione **Agregar** en la esquina superior derecha del panel **Aplicaciones empresariales**.

6. En el cuadro de texto **Escriba un nombre** de la sección **Agregar desde la galería**, escriba el nombre de la aplicación.

7. Seleccione la aplicación para la que quiere configurar el SSO.

8. *Opcional:* antes de agregar la aplicación, puede cambiar el nombre en el cuadro de texto **Nombre**.

9. Haga clic en **Agregar** para agregar la aplicación.

   Después de un pequeño lapso de tiempo, podrá ver el panel de configuración de la aplicación.

### <a name="configure-the-app-for-password-sso"></a>Configuración de la aplicación para inicio de sesión único con contraseña

Siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como administrador global o coadministrador.

2. Seleccione **Todos los servicios** en la parte superior del panel del lado izquierdo para abrir la extensión de Azure AD.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda de filtrado y, luego, seleccione **Azure Active Directory**.

4. En el panel de navegación de Azure AD, seleccione **Aplicaciones empresariales**.

5. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   > [!NOTE]
   > Si no ve la aplicación que busca, use el control **Filtro** en la parte superior de la lista **Todas las aplicaciones**. Establezca la opción **Mostrar** en "Todas las aplicaciones".

6. Seleccione la aplicación para la que quiere configurar el SSO.

7. Después de que la aplicación se cargue, seleccione **Inicio de sesión único** en el panel del lado izquierdo de la aplicación.

8. Seleccione el modo **Inicio de sesión basado en contraseña**.

9. Asigne usuarios a la aplicación.

10. También puede proporcionar credenciales para los usuarios. (En caso contrario, se le pedirá a los usuarios que escriban las credenciales cuando inicien la aplicación). Para ello, seleccione las filas de los usuarios. Luego seleccione **Actualizar credenciales** y escriba los nombres de usuario y contraseñas.

### <a name="assign-users-to-the-app"></a>Asignación de usuarios a la aplicación

Para asignar usuarios directamente a una aplicación, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com/) e inicie sesión como administrador global.

2. En el panel de navegación del lado izquierdo, seleccione **Todos los servicios** para abrir la extensión de Azure AD.

3. Escriba **Azure Active Directory** en el cuadro de búsqueda de filtrado y, luego, seleccione **Azure Active Directory**.

4. En el panel de navegación de Azure AD, seleccione **Aplicaciones empresariales**.

5. Seleccione **Todas las aplicaciones** para ver una lista de las aplicaciones.

   > [!NOTE]
   > Si no ve la aplicación que busca, use el control **Filtro** en la parte superior de la lista **Todas las aplicaciones**. Establezca la opción **Mostrar** en "Todas las aplicaciones".

6. En la lista, seleccione la aplicación a la que quiere asignar un usuario.

7. Una vez se cargue la aplicación, seleccione **Usuarios y grupos** en el panel de navegación del lado izquierdo.

8. Seleccione **Agregar** en la parte superior de la lista **Usuarios y grupos** para abrir el panel **Agregar asignación**.

9. En el panel **Agregar asignación**, seleccione **Usuarios y grupos**.

10. En el cuadro de texto **Buscar por nombre o dirección de correo**, escriba el nombre completo o la dirección de correo electrónico del usuario que quiere asignar.

11. Mantenga el ratón sobre el usuario en la lista. Seleccione la casilla situada junto a la foto o el logotipo del perfil del usuario para agregarlo a la lista **Seleccionados**.

12. *Opcional:* para agregar otro usuario, escriba otra nombre o dirección de correo electrónico en el cuadro de texto **buscar por nombre o dirección de correo electrónico** y, después, seleccione la casilla para agregarlo a la lista **Seleccionados**.

13. Cuando haya terminado de seleccionar usuarios, haga clic en **Seleccionar** para agregarlos a la lista de usuarios y grupos que se van a asignar a la aplicación.

14. *Opcional:* haga clic en **Seleccionar rol** del panel **Agregar asignación** para seleccionar un rol que se asignará a los usuarios que ha seleccionado.

15. Seleccione **Asignar** para asignar la aplicación a los usuarios seleccionados.

    Tras un breve lapso de tiempo, los usuarios podrán acceder a esas aplicaciones desde el Panel de acceso.

## <a name="request-support"></a>Solicitud de soporte técnico 
Si recibe un mensaje de error al configurar el inicio de sesión único y asignar usuarios, abra una incidencia de soporte técnico. De la siguiente información, incluya tanta como sea posible:

-   Id. de error de correlación
-   UPN (dirección de correo electrónico del usuario)
-   TenantID
-   Tipo de explorador
-   Zona horaria y hora o período de tiempo en el que se ha producido el error
-   Seguimientos de Fiddler

## <a name="next-steps"></a>Pasos siguientes
[Proporcionar un inicio de sesión único a las aplicaciones con el proxy de aplicación](application-proxy-configure-single-sign-on-with-kcd.md)
