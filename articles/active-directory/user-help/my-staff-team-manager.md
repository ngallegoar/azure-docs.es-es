---
title: 'Administración de contraseñas y números de teléfono con Mi personal (versión preliminar): Azure AD | Microsoft Docs'
description: Administración de las contraseñas y los números de teléfono para sus usuarios con Mi personal
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.topic: end-user-help
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: sahenry
ms.custom: oldportal;it-pro;
ms.openlocfilehash: d692302df1f5c03a7dce60858b31e0ca66de4c80
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744536"
---
# <a name="delegate-user-management-with-my-staff-preview"></a>Delegación de la administración de usuarios con Mi personal (versión preliminar)

Una organización puede usar **Mi personal** para delegar las tareas de administración de usuarios a figuras de autoridad, como un administrador de tienda o un líder de equipo, para ayudar a que los miembros de su personal accedan a las aplicaciones que necesitan. Si un miembro del equipo no puede acceder a una aplicación porque olvidó una contraseña, se pierde la productividad. Esto también impulsa los costos de soporte técnico y genera un cuello de botella en los procesos administrativos.  Con Mi personal, un miembro del equipo que no puede tener acceso a su cuenta puede recuperar el acceso con tan solo un par de clics, sin necesidad de ayuda por parte del administrador.

## <a name="manage-your-staff-in-my-staff"></a>Administración del personal en Mi personal

Administrar los miembros de un equipo en Mi personal es sencillo. Para empezar, [vaya a Mi personal](https://aka.ms/mystaff), seleccione un equipo o una ubicación y, luego, seleccione un usuario. Las ubicaciones y los miembros del equipo en una ubicación vienen determinados por el administrador de TI y no se pueden modificar.

Si administra más de una ubicación, cuando vaya a Mi personal debe seleccionar una ubicación para ver el miembro del equipo que está asignado a la ubicación.

Si todavía no tiene los permisos suficientes para acceder a Mi personal, verá el mensaje siguiente: "Oops, seems you're not authorized to see My Staff at this time. Please contact your admin for more information" ("Lo sentimos, pero no está autorizado para ver Mi personal en este momento. Póngase en contacto con el administrador para más información").

### <a name="find-a-staff-member-in-my-staff"></a>Buscar un miembro del personal en Mi personal

Debe abrir el perfil de un miembro del personal para poder empezar a administrarlos.

1. [Abra Mi personal](https://aka.ms/mystaff) y, si es necesario, seleccione una ubicación.

    ![Selección de una ubicación para un miembro del equipo en Mi personal](media/my-staff-team-manager/allaus.png)

1. Abra el perfil de un miembro del equipo.

    ![Selección de uno de los usuarios de una ubicación en Mi personal](media/my-staff-team-manager/aupage.png)

## <a name="reset-a-user-password"></a>Restablecimiento de la contraseña del usuario

Si la organización le dio permiso, puede restablecer las contraseñas de los miembros del personal.

1. [Abra Mi personal](https://aka.ms/mystaff).
1. Abra el perfil de un miembro del equipo.
1. Seleccione **Restablecer contraseña**.

    ![Restablecimiento de una contraseña de usuario en Mi personal](media/my-staff-team-manager/resetpassword1.png)

1. Genere o escriba la contraseña nueva. Es posible que se muestre una contraseña temporal generada automáticamente o que se le pida que escriba una contraseña temporal para el usuario.

    ![Copia de la contraseña de usuario temporal después de un restablecimiento en Mi personal](media/my-staff-team-manager/resetpassword2.png)

Una vez que haya restablecido la contraseña del usuario, proporciónele la contraseña temporal al usuario. Cuando el usuario inicie sesión con su contraseña temporal, deberá cambiarla.

## <a name="manage-a-users-phone-number"></a>Administración del número de teléfono de un usuario

Si la organización le dio permiso, puede administrar los números de teléfono de los miembros del personal.

### <a name="add-a-phone-number"></a>Agregar un número de teléfono

1. [Abra Mi personal](https://aka.ms/mystaff).
1. Abra el perfil de un miembro del equipo.
1. Seleccione **Agregar número de teléfono**.

    ![Agregar el número de teléfono de un usuario en Mi personal](media/my-staff-team-manager/addphone1.png)

1. Agregue el número de teléfono y seleccione **Guardar**.

    ![Guardar el número de teléfono del usuario agregado en Mi personal](media/my-staff-team-manager/addphone2.png)

Una vez que registre un número de teléfono para un usuario, este puede usarlo para iniciar sesión con SMS, realizar la verificación en dos pasos o restablecer su contraseña por sí solo, según la configuración de la organización.

![Número de teléfono nuevo registrado con Mi personal](media/my-staff-team-manager/addphone3.png)

### <a name="edit-a-phone-number"></a>Editar un número de teléfono

1. [Abra Mi personal](https://aka.ms/mystaff).
1. Abra el perfil de un miembro del equipo.
1. Seleccione **Editar número de teléfono**.

    ![Seleccionar Editar desde el perfil de usuario en Mi personal](media/my-staff-team-manager/editphone2.png)

1. Escriba el número de teléfono nuevo y seleccione **Guardar**.

    ![Editar el número de teléfono de un miembro del personal en Mi personal](media/my-staff-team-manager/editphone1.png)

### <a name="enable-phone-number-sign-in-for-a-user"></a>Habilitar el inicio de sesión mediante número de teléfono para un usuario

Si el inicio de sesión con un número de teléfono como nombre de usuario (inicio de sesión por SMS) está habilitado en su organización, puede agregar esta autenticación a un número de teléfono de usuario existente.

1. [Abra Mi personal](https://aka.ms/mystaff).
1. Abra el perfil de un miembro del equipo.
1. Si en la parte inferior de la pantalla aparece un mensaje que indica que tiene disponible la posibilidad de iniciar sesión con el número de teléfono como nombre de usuario, seleccione **Habilitar** para empezar el proceso. Este mensaje aparece si el usuario está habilitado para iniciar sesión con su número de teléfono.

    ![Ver el mensaje cuando se admite el inicio de sesión en el teléfono en una ubicación de Mi personal](media/my-staff-team-manager/enableforms1.png)

1. Seleccione **Aceptar** cuando haya terminado.

    ![Quitar el número de teléfono de un miembro del personal en Mi personal](media/my-staff-team-manager/enableforms2.png)

### <a name="remove-a-phone-number"></a>Quitar un número de teléfono

1. [Abra Mi personal](https://aka.ms/mystaff).
1. Abra el perfil de un miembro del equipo.
1. Seleccione **Quitar número de teléfono**.
1. Seleccione **Eliminar** cuando haya terminado.

    ![Quitar el número de teléfono de un miembro del personal en Mi personal](media/my-staff-team-manager/deletephone1.png)
