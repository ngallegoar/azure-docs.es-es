---
title: Inicio rápido de directiva de expiración de grupo - Azure AD | Microsoft Docs
description: Expiración de grupos de Office 365 en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16b6e4f521568c89e415e6e9fa621175052cf34d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74026894"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Guía de inicio rápido: Establecimiento de la expiración de los grupos de Office 365 en Azure Active Directory

En esta guía de inicio rápido, puede establecer la directiva de expiración para los grupos de Office 365. Cuando los usuarios pueden configurar sus propios grupos, el número de grupos sin uso se puede multiplicar. Una manera de administrar los grupos sin uso es establecer la expiración de dichos grupos, para reducir el mantenimiento de la eliminación manual de grupos.

La directiva de expiración es sencilla:

- Los grupos con actividades de usuario se renuevan automáticamente a medida que se aproxima la expiración.
- Los propietarios del grupo reciben una notificación para renovar un grupo que va a expirar.
- Se eliminará cualquier grupo que no se renueve.
- El propietario de un grupo o un administrador de Azure AD pueden restaurar un grupo de Office 365 eliminado en 30 días.

> [!NOTE]
> Ahora, los grupos usan la inteligencia de Azure AD para que se renueven automáticamente en función de si se han usado recientemente. Esta decisión de renovación se basa en la actividad de los usuarios en grupos en los servicios de Office 365 como Outlook, SharePoint, Teams, Yammer y otros.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisite"></a>Requisito previo

 Para configurar la expiración de grupos, es necesario contar como mínimo con el rol de Usuario administrador en la organización.

## <a name="turn-on-user-creation-for-groups"></a>Activación de la creación de grupos por el usuario

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de Usuario administrador.

2. Seleccione **Grupos** y, a continuación, seleccione **General**.
  
   ![Página de configuración de autoservicio de grupos](./media/groups-quickstart-expiration/self-service-settings.png)

3. Establezca **Los usuarios pueden crear grupos de Office 365** en **Sí**.

4. Seleccione **Guardar** para guardar la configuración de grupos cuando haya terminado.

## <a name="set-group-expiration"></a>Establecimiento de la expiración de grupo

1. Inicie sesión en [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory** > **Grupos** > **Expiración** para abrir la configuración de expiración.
  
   ![Página de configuración de expiración del grupo](./media/groups-quickstart-expiration/expiration-settings.png)

2. Establezca el intervalo de expiración. Seleccione un valor preestablecido o escriba un valor personalizado superior a 31 días. 

3. Especifique una dirección de correo electrónico a la que deben enviarse las notificaciones de expiración cuando un grupo no tiene propietario.

4. Para esta guía de inicio rápido, establezca **Habilitar expiración de estos grupos de Office 365** en **Todos**.

5. Seleccione **Guardar** para guardar la configuración de expiración cuando haya terminado.

Eso es todo. En esta guía de inicio rápido, ha establecido correctamente la directiva de expiración para los grupos de Office 365 seleccionados.

## <a name="clean-up-resources"></a>Limpieza de recursos

### <a name="to-remove-the-expiration-policy"></a>Para eliminar la directiva de expiración, siga estos pasos:

1. Asegúrese de que ha iniciado sesión en [Azure Portal](https://portal.azure.com) con una cuenta que sea el administrador global del inquilino.
2. Seleccione **Azure Active Directory** > **Grupos** > **Expiración**.
3. Establezca **Habilitar expiración de estos grupos de Office 365** en **Ninguno**.

### <a name="to-turn-off-user-creation-for-groups"></a>Para desactivar la creación de grupos por el usuario, siga estos pasos:

1. Seleccione **Azure Active Directory** > **Grupos** > **General**. 
2. En Azure Portal, establezca **Los usuarios pueden crear grupos de Office 365** en **No**.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la expiración, incluidas las instrucciones de PowerShell y las limitaciones técnicas, consulte el artículo siguiente:

> [!div class="nextstepaction"]
> [Expiration policy PowerShell](groups-lifecycle.md) (Directiva de expiración de PowerShell)
