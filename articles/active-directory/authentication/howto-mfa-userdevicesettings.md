---
title: 'Administración de usuarios y dispositivos: Azure MFA - Azure Active Directory'
description: Describe cómo los administradores cambian las opciones de la configuración del usuario, como por ejemplo forzar a los usuarios realizar nuevamente el proceso proofup.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25d94fa761980151c420984eb7e8c3254a3509ef
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653490"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Administrar la configuración de usuario con Azure Multi-Factor Authentication en la nube

Como administrador, puede administrar las siguientes opciones de configuración de usuario y de dispositivo:

* Requerir a los usuarios que vuelvan a proporcionar métodos de contacto
* Eliminar contraseñas de aplicación
* Requerir MFA en todos los dispositivos de confianza

## <a name="manage-authentication-methods"></a>Administración de los métodos de autenticación

Como administrador que tiene asignado el rol de administrador de autenticación, puede requerir a los usuarios que restablezcan su contraseña, que vuelvan a registrarse para MFA o que revoquen las sesiones de MFA existentes desde su objeto de usuario.

![Administración de los métodos de autenticación desde Azure Portal](./media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png)

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Elija el usuario en el que quiere realizar una acción y seleccione **Métodos de autenticación**.
   - **Restablecer contraseña** restablecerá la contraseña del usuario y asignará una contraseña temporal que se debe cambiar en el siguiente inicio de sesión.
   - **Requerir volver a registrar MFA** hará que cuando el usuario inicie sesión la próxima vez, se le solicite que configure un nuevo método de autenticación MFA.
   - **Revocar sesiones de MFA** borra las sesiones de MFA recordadas del usuario y le pide que realice MFA la próxima vez que la directiva lo exija en el dispositivo.

## <a name="delete-users-existing-app-passwords"></a>Eliminar las contraseñas de aplicación existentes de los usuarios

Esta configuración elimina todas las contraseñas de aplicación que ha creado un usuario. Las aplicaciones sin explorador asociadas con estas contraseñas de aplicación dejan de funcionar hasta que se cree una nueva contraseña de aplicación. Se necesitan los permisos de administrador global para realizar esta acción.

### <a name="how-to-delete-users-existing-app-passwords"></a>Cómo eliminar las contraseñas de aplicación existentes de los usuarios

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
3. En la derecha, seleccione **Multi-factor Authentication** en la barra de herramientas. Se abre la página de Multi-Factor Authentication.
4. Active la casilla junto al usuario o usuarios que desea administrar. Aparecerá una lista de opciones de paso rápido a la derecha.
5. Seleccione **Administrar configuración de usuario**.
6. Active la casilla **Eliminar todas las contraseñas de aplicación existentes generadas por los usuarios seleccionados**.
   ![Eliminar todas contraseñas de aplicación existentes](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. Haga clic en **guardar**.
8. Haga clic en **Cerrar**.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
- Si los usuarios necesitan ayuda, diríjalos a la [Guía de usuario para la verificación en dos pasos](../user-help/multi-factor-authentication-end-user.md)
