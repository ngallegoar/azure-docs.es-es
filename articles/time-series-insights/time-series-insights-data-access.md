---
title: 'Configuración de la seguridad para conceder acceso a los datos: versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Aprenda a configurar la seguridad y los permisos, y a administrar las directivas de acceso a datos en el entorno de la versión preliminar de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/15/2020
ms.custom: seodec18
ms.openlocfilehash: 1735daf66483df496141a642ac6633973aa0abf0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81407545"
---
# <a name="grant-data-access-to-an-environment"></a>Concesión de acceso a datos en un entorno

En este artículo se tratan los dos tipos de directivas de acceso de la versión preliminar de Azure Time Series Insights.

> [!TIP]
> Lea [Autenticación y autorización](time-series-insights-authentication-and-authorization.md) para más información sobre los pasos de registro de aplicaciones de Azure Active Directory.

## <a name="sign-in-to-time-series-insights"></a>Iniciar sesión en Time Series Insights

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Busque el entorno de Time Series Insights. Especifique `Time Series` en el cuadro **Búsqueda**. Seleccione **Entornos de Time Series** en los resultados de búsqueda.
1. Seleccione el entorno de Time Series Insights de la lista.

## <a name="grant-data-access"></a>Concesión de acceso a datos

Siga estos pasos para conceder acceso a datos a una entidad de seguridad de usuario.

1. Seleccione **Directivas de acceso a datos** y, luego, **+ Agregar**.

    [![Seleccionar y agregar una directiva de acceso a datos](media/data-access/data-access-select-add-button.png)](media/data-access/data-access-select-add-button.png#lightbox)

1. Elija **Seleccionar usuario**. Busque el nombre de usuario o la dirección de correo para encontrar al usuario que quiere agregar. Elija **Seleccionar** para confirmar la selección.

    [![Seleccionar un usuario para agregarlo](media/data-access/data-access-select-user-to-confirm.png)](media/data-access/data-access-select-user-to-confirm.png#lightbox)

1. Elija **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario:

    * Seleccione **Colaborador** si quiere permitir al usuario cambiar los datos de referencia y compartir las consultas y las perspectivas guardadas con otros usuarios del entorno.

    * En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

   Seleccione **Aceptar** para confirmar la elección del rol.

    [![Confirmar el rol seleccionado](media/data-access/data-access-select-a-role.png)](media/data-access/data-access-select-a-role.png#lightbox)

1. Seleccione **Aceptar** en la página **Seleccionar rol de usuario**.

    [![Seleccionar Aceptar en la página Seleccionar rol de usuario](media/data-access/data-access-confirm-user-and-role.png)](media/data-access/data-access-confirm-user-and-role.png#lightbox)

1. Confirme que la página **Directivas de acceso a datos** enumera los usuarios y los roles de cada uno.

    [![Comprobar los usuarios y roles correctos](media/data-access/data-access-verify-and-confirm-assignments.png)](media/data-access/data-access-verify-and-confirm-assignments.png#lightbox)

## <a name="provide-guest-access-from-another-azure-ad-tenant"></a>Proporcionar acceso de invitado desde otro inquilino de Azure AD

El rol `Guest` no es un rol de administración. Se trata de un término que se usa para una cuenta a la que se ha invitado desde un inquilino a otro. Después de que la cuenta de invitado se haya invitado al directorio del inquilino, se le puede aplicar el mismo control de acceso que a cualquier otra cuenta. Puede conceder acceso de administración a un entorno de Time Series Insights mediante la hoja de Control de acceso (IAM). O puede conceder acceso a los datos del entorno utilizando la hoja Directivas de acceso a datos. Para más información sobre el acceso de invitado de inquilino de Azure Active Directory (Azure AD), consulte [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Siga estos pasos para conceder acceso de invitado a un entorno de Time Series Insights a un usuario de Azure AD desde otro inquilino.

1. Seleccione **Directivas de acceso a datos** y, luego, **+ Invitar**.

    [![Seleccionar Directivas de acceso a datos y, luego, + Invitar](media/data-access/data-access-invite-another-aad-tenant.png)](media/data-access/data-access-invite-another-aad-tenant.png#lightbox)

1. Escriba la dirección de correo electrónico del usuario que desea invitar. Esta dirección de correo electrónico tiene que estar asociada con Azure AD. Opcionalmente puede incluir un mensaje personal con la invitación.

    [![Escribir la dirección de correo electrónico para buscar al usuario seleccionado](media/data-access/data-access-invite-guest-by-email.png)](media/data-access/data-access-invite-guest-by-email.png#lightbox)

1. Busque la burbuja de confirmación que aparece en la pantalla.

    [![Buscar la burbuja de confirmación que aparece](media/data-access/data-access-confirmation-bubble.png)](media/data-access/data-access-confirmation-bubble.png#lightbox)

1. Elija **Seleccionar usuario**. Busque la dirección de correo electrónico del usuario que ha invitado para ubicar al usuario que quiere agregar. Haga clic en **Seleccionar** para confirmar la selección.

    [![Seleccionar el usuario y confirmar la selección](media/data-access/data-access-select-invited-person-confirmation.png)](media/data-access/data-access-select-invited-person-confirmation.png#lightbox)

1. Elija **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario invitado:

    * Seleccione **Colaborador** si quiere permitir al usuario cambiar los datos de referencia y compartir las consultas y las perspectivas guardadas con otros usuarios del entorno.

    * En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

   Seleccione **Aceptar** para confirmar la elección del rol.

    [![Confirmar la elección del rol](media/data-access/data-access-select-ok-and-confirm.png)](media/data-access/data-access-select-ok-and-confirm.png#lightbox)

1. Seleccione **Aceptar** en la página **Seleccionar rol de usuario**.

1. Confirme que la página **Directivas de acceso a datos** aparece el usuario invitado y los roles de cada usuario invitado.

    [![Comprobar que usuarios y roles están correctamente asignados](media/data-access/data-access-confirm-invited-users-and-roles.png)](media/data-access/data-access-confirm-invited-users-and-roles.png#lightbox)

1. Ahora, el usuario invitado recibirá un correo electrónico de invitación en la dirección especificada anteriormente. El usuario invitado seleccionará **Comenzar** para confirmar su aceptación y conectarse a la nube de Azure.

    [![El invitado selecciona Comenzar para aceptar](media/data-access/data-access-email-invitation.png)](media/data-access/data-access-email-invitation.png#lightbox)

1. Después de seleccionar **Comenzar**, se presentará al usuario invitado un cuadro de permisos asociado a la organización del administrador. Tras conceder el permiso seleccionando **Aceptar**, se iniciará la sesión.

    [![El invitado revisa los permisos y los acepta](media/data-access/data-access-grant-permission-sign-in.png)](media/data-access/data-access-grant-permission-sign-in.png#lightbox)

1. El administrador [comparte la dirección URL del entorno](time-series-insights-parameterized-urls.md) con su invitado.

1. Cuando el usuario invitado inicia sesión en la dirección de correo electrónico que utilizó para invitarle y acepta la invitación, se le redirige a Azure Portal. 

1. Ahora el invitado puede acceder al entorno compartido mediante la dirección URL del entorno que ha proporcionado el administrador. El invitado puede especificar esa dirección URL en su explorador web para obtener acceso inmediato.

1. El inquilino del administrador se mostrará al usuario invitado después de seleccionar su icono de perfil en la esquina superior derecha del explorador de Time Series.

    [![Selección del avatar en insights.azure.com](media/data-access/data-access-select-tenant-and-instance.png)](media/data-access/data-access-select-tenant-and-instance.png#lightbox)


    Una vez que el usuario invitado selecciona el inquilino del administrador, tiene la posibilidad de seleccionar el entorno compartido de Time Series Insights. 
    
    Ahora tienen todas las funcionalidades asociadas al rol que le ha proporcionado en el **paso 5**.

    [![El usuario invitado selecciona el inquilino de Azure en la lista desplegable](media/data-access/data-access-all-capabilities.png)](media/data-access/data-access-all-capabilities.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

* Aprenda [cómo agregar un origen del evento de Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) al entorno de Time Series Insights.

* Envíe [eventos al origen del evento](./time-series-insights-send-events.md).

* Visualice [el entorno en el explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).
