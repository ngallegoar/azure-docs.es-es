---
title: 'Administración de la configuración para Azure Multi-Factor Authentication: Azure Active Directory'
description: Aprenda a definir la configuración de usuarios de Azure Active Directory para Azure Multi-Factor Authentication
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 048224a55c2bbcbc99281d070d88d34e2dc77168
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309770"
---
# <a name="manage-user-settings-for-azure-multi-factor-authentication"></a>Administración de la configuración de usuarios para Azure Multi-Factor Authentication

Para ayudar a administrar los usuarios de Azure Multi-Factor Authentication, puede requerirles que restablezcan su contraseña, que vuelvan a registrarse para MFA o que revoquen las sesiones de MFA existentes. En el caso de los usuarios que hayan definido contraseñas de aplicación, también puede eliminar esas contraseñas, lo que provocará un error en la autenticación heredada en esas aplicaciones. Estas acciones pueden ser necesarias si necesita proporcionar asistencia a un usuario o desea restablecer su estado de seguridad.

## <a name="manage-user-authentication-options"></a>Administración de opciones de autenticación de usuarios

Si tiene asignado el rol de *administrador de autenticación*, puede requerir a los usuarios que restablezcan su contraseña, que vuelvan a registrarse para MFA o que revoquen las sesiones de MFA existentes desde su objeto de usuario. Para administrar la configuración de usuario, complete los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Elija el usuario en el que quiere realizar una acción y seleccione **Métodos de autenticación**. En la parte superior de la ventana, elija una de las siguientes opciones para el usuario:
   - **Restablecer contraseña** restablece la contraseña del usuario y asigna una contraseña temporal que se debe cambiar en el siguiente inicio de sesión.
   - **Requerir volver a registrar MFA** hace que cuando el usuario inicie sesión la próxima vez, se le solicite que configure un nuevo método de autenticación MFA.
   - **Revocar sesiones de MFA** borra las sesiones de MFA recordadas del usuario y le pide que realice MFA la próxima vez que la directiva lo exija en el dispositivo.

   ![Administración de los métodos de autenticación desde Azure Portal](./media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png)

## <a name="delete-users-existing-app-passwords"></a>Eliminar las contraseñas de aplicación existentes de los usuarios

Si es necesario, puede eliminar todas las contraseñas de aplicación que ha creado un usuario. Las aplicaciones sin explorador asociadas con estas contraseñas de aplicación dejan de funcionar hasta que se cree una nueva contraseña de aplicación. Se necesitan los permisos de *administrador global* para realizar esta acción.

Para eliminar las contraseñas de aplicación de un usuario, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Seleccione **Multi-Factor Authentication**. Es posible que tenga que desplazarse hacia la derecha para ver esta opción de menú. Seleccione la siguiente captura de pantalla de ejemplo para ver la ventana de Azure Portal completa y la ubicación del menú: [![](media/howto-mfa-userstates/selectmfa-cropped.png "Selección de Multi-Factor Authentication desde la ventana Usuarios en Azure AD")](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Active la casilla junto al usuario o usuarios que desea administrar. Aparecerá una lista de opciones de paso rápido a la derecha.
1. Seleccione **Administrar configuración de usuario** y active la casilla **Eliminar todas las contraseñas de aplicación existentes generadas por los usuarios seleccionados**, tal como se muestra en el ejemplo siguiente: ![Eliminar todas contraseñas de aplicación existentes](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
1. Seleccione **Guardar** y luego **Cerrar**.

## <a name="next-steps"></a>Pasos siguientes

Este artículo le ayudó a configurar opciones de usuario individuales. Para configurar las opciones del servicio Azure Multi-Factor Authentication, consulte [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md).

Si los usuarios necesitan ayuda, consulte la [guía del usuario de Azure Multi-Factor Authentication](../user-help/multi-factor-authentication-end-user.md).
