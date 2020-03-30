---
title: 'Revisión del acceso a los grupos y las aplicaciones en las revisiones de acceso: Azure AD'
description: Aprenda a revisar el acceso de los miembros del grupo o de las aplicaciones en las revisiones de acceso de Azure Active Directory.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee4125e82dd5176f01de294011e22a1d66005094
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128448"
---
# <a name="review-access-to-groups-and-applications-in-azure-ad-access-reviews"></a>Revisión del acceso a los grupos y las aplicaciones en las revisiones de acceso de Azure AD

Azure Active Directory (Azure AD) simplifica el modo en que las empresas administran el acceso a los grupos y las aplicaciones en Azure AD y otras instancias de Microsoft Online Services, gracias a una característica denominada revisiones de acceso de Azure AD.

En este artículo se describe cómo un revisor designado lleva a cabo una revisión de acceso para los miembros de un grupo o los usuarios con acceso a una aplicación.

## <a name="open-the-access-review"></a>Abrir la revisión de acceso

El primer paso para realizar una revisión de acceso es buscar y abrir la revisión de acceso.

1. Busque un correo electrónico de Microsoft que le pide que revise el acceso. Este es un ejemplo de un correo electrónico para revisar el acceso de un grupo.

    ![Ejemplo de correo electrónico de Microsoft para revisar el acceso a un grupo](./media/perform-access-review/access-review-email.png)

1. Haga clic en el vínculo **Start review** (Iniciar revisión) para abrir la revisión de acceso.

Si no tiene el correo electrónico, puede encontrar las revisiones de acceso pendientes mediante estos pasos.

1. Inicie sesión en el portal de aplicaciones en [https://myapps.microsoft.com](https://myapps.microsoft.com).

    ![Portal de aplicaciones con las aplicaciones para las que tiene permiso](./media/perform-access-review/myapps-access-panel.png)

1. En la esquina superior derecha de la página, haga clic en el símbolo de usuario que muestra la organización predeterminada y el nombre de usuario. Si aparece más de una organización, seleccione la que haya solicitado una revisión de acceso.

1. Haga clic en el icono **Revisiones de acceso** para ver una lista de las revisiones de acceso pendientes.

    Si el icono no está visible, no hay ninguna revisión de acceso para esa organización y no se requiere ninguna acción en este momento.

    ![Lista de revisiones de acceso pendientes para aplicaciones y grupos](./media/perform-access-review/access-reviews-list.png)

1. Haga clic en el vínculo **Begin review** (Comenzar revisión) correspondiente a la revisión de acceso que quiere realizar.

## <a name="perform-the-access-review"></a>Realización de la revisión de acceso

Cuando haya abierto la revisión de acceso, verá los nombres de los usuarios que necesitan revisarse.

Si la solicitud es para revisar su propio acceso, la página tendrá un aspecto diferente. Para más información, consulte [Revisar el acceso por sí mismo para grupos o aplicaciones](review-your-access.md).

![Revisión de acceso abierta con los usuarios que es necesario revisar](./media/perform-access-review/perform-access-review.png)

Hay dos maneras de aprobar o denegar el acceso:

- Puede aprobar o denegar el acceso de uno o más usuarios.
- Puede aceptar las recomendaciones del sistema, que es la manera más rápida y fácil.

### <a name="approve-or-deny-access-for-one-or-more-users"></a>Aprobación o denegación del acceso de uno o varios usuarios

1. Revise la lista de usuarios para decidir si aprobar o denegar su acceso continuado.

1. Para aprobar o denegar el acceso de un solo usuario, haga clic en la fila para abrir una ventana y especificar la acción que se realizará. Para aprobar o denegar el acceso de varios usuarios, agregue marcas de verificación junto a los usuarios y, luego, haga clic en el botón **Review X user(s)** (Revisar usuarios: X) para abrir una ventana y especificar la acción que se realizará.

1. Haga clic en **Aprobar** o **Denegar**. Si no está seguro, puede hacer clic en **Desconocido**. En este caso, el usuario mantiene el acceso, pero la selección se reflejará en los registros de auditoría.

    ![Ventana de acción que incluye las opciones Aprobar, Denegar y Desconocido](./media/perform-access-review/approve-deny.png)

1. Si es necesario, escriba un motivo en el cuadro **Motivo**.

    El administrador de la revisión de acceso puede pedirle que proporcione un motivo para aprobar el acceso continuo o la pertenencia a grupos.

1. Una vez que ha especificado la acción para realizar, haga clic en **Guardar**.

    Si quiere cambiar la respuesta, seleccione la fila y actualice la respuesta. Por ejemplo, puede aprobar el acceso de un usuario denegado anteriormente o denegar el de un usuario aprobado antes. Puede cambiar su respuesta en cualquier momento hasta que finalice la revisión de acceso.

    Si hay varios revisores, se registra la última respuesta enviada. Considere un ejemplo donde un administrador designa dos revisores: Alice y Bob. Alice abre primero la revisión de acceso y aprueba el acceso. Antes de que finalice la revisión, Bob abre la revisión de acceso y deniega el acceso. La última respuesta de denegación es la que se registra.

    > [!NOTE]
    > Si un usuario tiene el acceso denegado, no se quita inmediatamente. Se quita cuando la revisión ha finalizado o cuando un administrador la detiene.

### <a name="approve-or-deny-access-based-on-recommendations"></a>Aprobación o denegación del acceso según las recomendaciones

Para facilitar las revisiones de acceso y que sean más rápidas, también se proporcionan recomendaciones que se pueden aceptar con un solo clic. Las recomendaciones se generan en función de actividad de inicio de sesión del usuario.

1. En la barra azul en la parte inferior de la página, haga clic en **Aceptar recomendaciones**.

    ![Revisión de acceso abierta que muestra el botón Aceptar recomendaciones](./media/perform-access-review/accept-recommendations.png)

    Verá un resumen de las acciones recomendadas.

    ![Ventana que muestra un resumen de las acciones recomendadas](./media/perform-access-review/accept-recommendations-summary.png)

1. Haga clic en **Aceptar** para aceptar las recomendaciones.

## <a name="next-steps"></a>Pasos siguientes

- [Completar una revisión de acceso de grupos o aplicaciones](complete-access-review.md)
