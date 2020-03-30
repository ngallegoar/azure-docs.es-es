---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: e971af5d415ea788f97ab11b58541e629c7eb0ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76694575"
---
## <a name="lifecycle"></a>Ciclo de vida

En la sección **Ciclo de vida**, especificará cuándo expira la asignación de un usuario para el paquete de acceso. También puede especificar si los usuarios pueden extender sus asignaciones.

1. En la sección **Expiración**, establezca **Access package assignments expires** (La asignación de paquetes de acceso expira) en **En la fecha**, **Número de días** o **Nunca**.

    Para la opción **En la fecha**, seleccione una fecha de expiración en el futuro.

    Para la opción **Número de días**, especifique un número entre 0 y 3660 días.

    En función de lo que seleccione, la asignación de un usuario para el paquete de acceso expirará en una fecha concreta, un número determinado de días después de la aprobación o nunca.

1. Haga clic en **Mostrar configuración de expiración avanzada** para mostrar la configuración adicional.

    ![Paquete de acceso - Configuración de la expiración de la directiva](./media/active-directory-entitlement-management-lifecycle-policy/expiration.png)

1. Para permitir que el usuario amplíe sus asignaciones, establezca **Permitir que los usuarios extiendan el acceso** en **Sí**.

    Si se permiten las extensiones en la directiva, el usuario recibirá un correo electrónico 14 días y 1 día antes de que se establezca la expiración de su asignación de paquete de acceso en el que se le solicitará que amplíe la asignación. Si el usuario envía una solicitud para extender el acceso, la fecha de la extensión debe ser igual o anterior a la expiración de las asignaciones, tal y como se define en la drectiva que se usó para conceder al usuario acceso al paquete de acceso. Por ejemplo, si la directiva indica que las asignaciones están configuradas para expirar el 30 de junio, la extensión máxima que un usuario puede solicitar es el 30 de junio.

    Si se extiende el acceso de un usuario, no podrán solicitar el paquete de acceso después de la fecha de extensión especificada (la fecha está establecida en la zona horaria del usuario que ha creado la directiva).

1. Para requerir la aprobación para conceder una extensión, establezca **Requerir aprobación para conceder extensión** en **Sí**.

    Se utilizará la misma configuración de aprobación que se especificó en la pestaña **Solicitudes**.

1. Haga clic en **Siguiente** o en **Actualizar**.
