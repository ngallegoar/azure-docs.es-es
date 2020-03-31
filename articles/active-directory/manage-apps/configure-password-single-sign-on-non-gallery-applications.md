---
title: Procedimiento para configurar el inicio de sesión único con contraseña para aplicaciones de Azure AD | Microsoft Docs
description: Procedimiento para configurar el inicio de sesión único (SSO) en aplicaciones empresariales que no están en la plataforma de identidad de Microsoft (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 563bda275b73f76b042b5e57a9909ca78c504bb3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063533"
---
# <a name="configure-password-single-sign-on"></a>Configuración del inicio de sesión único con contraseña

Al [agregar una aplicación de la galería](add-gallery-app.md) o una [aplicación web que no es de la galería](add-non-gallery-app.md) a sus aplicaciones empresariales de Azure AD, una de las opciones de inicio de sesión único disponibles es el [inicio de sesión único basado en contraseña](what-is-single-sign-on.md#password-based-sso). Esta opción está disponible para cualquier web con una página de inicio de sesión HTML. El SSO basado en contraseña, también conocido como almacenamiento de contraseñas, permite administrar el acceso y las contraseñas de los usuarios en aplicaciones web que no admiten la federación de identidades. También es útil para escenarios en los que varios usuarios necesitan compartir una sola cuenta, como las cuentas de aplicaciones de redes sociales de la organización. 

El SSO basado en contraseña representa una excelente manera de empezar a integrar aplicaciones en Azure AD rápidamente, y le permite lo siguiente:

-   Habilitar el **inicio de sesión único para los usuarios** almacenando y reproduciendo de forma segura los nombres de usuario y contraseñas para la aplicación que ha integrado con Azure AD.

-   **Admitir aplicaciones que requieren varios campos de inicio de sesión** para aplicaciones que no solo requieren los campos de nombre de usuario y contraseña para iniciar sesión.

-   **Personalizar las etiquetas** de los campos de entrada de nombre de usuario y contraseña que los usuarios ven en el [panel de acceso de la aplicación](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) cuando escriben sus credenciales

-   Permitir a los **usuarios** proporcionar sus propios nombres de usuario y contraseñas para las cuentas de aplicación existentes que están escribiendo manualmente en el [panel de acceso de la aplicación](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Permitir que un **miembro del grupo de negocios** especifique los nombres de usuario y contraseñas que se asignan a un usuario mediante el uso de la característica [Acceso de autoservicio a las aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access)

-   Permitir que un **administrador** especifique un nombre de usuario y una contraseña que usarán los usuarios o grupos al iniciar sesión en la aplicación mediante la característica Actualizar credenciales 

## <a name="before-you-begin"></a>Antes de empezar

Si la aplicación no se ha agregado a su inquilino de Azure AD, consulte [Incorporación de una aplicación de la galería](add-gallery-app.md) o [Incorporación de una aplicación que no es de la galería](add-non-gallery-app.md).

## <a name="open-the-app-and-select-password-single-sign-on"></a>Abra la aplicación y seleccione Inicio de sesión único con contraseña.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de aplicaciones en la nube o administrador de aplicaciones para el inquilino de Azure AD.

2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales**. Se mostrará una muestra aleatoria de las aplicaciones en su inquilino de Azure AD. 

3. En el menú **Tipo de aplicación**, seleccione **Todas las aplicaciones** y, después, **Aplicar**.

4. Escriba el nombre de la aplicación en el cuadro de búsqueda y seleccione la aplicación en los resultados.

5. En la sección **Administrar**, seleccione **Inicio de sesión único**. 

6. Seleccione **Basado en contraseñas**.

7. Escriba la dirección URL de la página de inicio de sesión basada en web de la aplicación. Esta cadena debe ser la página que incluye el campo de entrada del nombre de usuario.

   ![Inicio de sesión único con contraseña](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

8. Seleccione **Guardar**. Azure AD intenta analizar la página de inicio de sesión para detectar si se han escrito un nombre de usuario y una contraseña. Si el intento se realiza correctamente, habrá terminado. 
 
> [!NOTE]
> El siguiente paso será la [Asignación de usuarios y grupos a la aplicación](methods-for-assigning-users-and-groups.md). Después de asignar usuarios y grupos, puede proporcionar las credenciales que se usarán en nombre de un usuario cuando inicie sesión en la aplicación. Seleccione **Usuarios y grupos**, active la casilla correspondiente a la fila del usuario o del grupo y, después, haga clic en **Actualizar credenciales**. Después, escriba el nombre de usuario y la contraseña que se usarán en nombre del usuario o grupo. En caso contrario, se solicitará a los usuarios que especifiquen ellos mismos las credenciales al inicio.
 

## <a name="manual-configuration"></a>Configuración manual

Si se produce un error en el intento de análisis de Azure AD, puede configurar el inicio de sesión manualmente.

1. En **Configuración de \<nombreDeAplicación>** , seleccione **Establecer configuración de inicio de sesión único con contraseña de \<nombreDeAplicación> para mostrar la página** **Configurar inicio de sesión**. 

2. Seleccione **Detectar campos de inicio de sesión manualmente**. Aparecen instrucciones adicionales que describen la detección manual de los campos de inicio de sesión.

   ![Configuración manual del inicio de sesión único con contraseña](./media/configure-password-single-sign-on/password-configure-sign-on.png)
3. Seleccione **Capturar campos de inicio de sesión**. Se abre una página de estado de la captura en una nueva pestaña, que muestra el mensaje **Actualmente la captura de metadatos está en curso**.

4. Si la casilla que indica que se **requiere una extensión del panel de acceso** aparece en una nueva pestaña, seleccione **Instalar ahora** para instalar la extensión del explorador **Extensión de inicio de sesión seguro de mis aplicaciones**. (La extensión del explorador requiere Microsoft Edge, Chrome o Firefox). A continuación, instale, inicie y active la extensión, y actualice la página de estado de la captura.

   La extensión del explorador abre otra pestaña que muestra la dirección URL especificada.
5. En la pestaña con la dirección URL especificada, recorra el proceso de inicio de sesión. Rellene los campos de nombre de usuario y contraseña e intente iniciar sesión. (No es necesario proporcionar la contraseña correcta).

   Se le pedirá que guarde los campos de inicio de sesión capturados.
6. Seleccione **Aceptar**. La extensión del explorador actualiza la página de estado de la captura con el mensaje **Los metadatos se han actualizado para la aplicación**. La pestaña del explorador se cierra.

7. En la página **Configurar inicio de sesión** de Azure AD, seleccione **Bien. Pude iniciar sesión en la aplicación correctamente**.

8. Seleccione **Aceptar**.

Después de capturar la página de inicio de sesión, puede asignar los usuarios y grupos, y puede configurar las directivas de credenciales como si fueran [aplicaciones normales de inicio de sesión único con contraseña](what-is-single-sign-on.md).

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de usuarios y grupos a la aplicación](methods-for-assigning-users-and-groups.md)
- [Configuración del aprovisionamiento automático de cuentas de usuario](../app-provisioning/configure-automatic-user-provisioning-portal.md)
