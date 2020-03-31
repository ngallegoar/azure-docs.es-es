---
title: 'Revisión del acceso a grupos y aplicaciones en las revisiones de acceso: Azure AD'
description: Obtenga información sobre cómo revisar su propio acceso a grupos o aplicaciones en las revisiones de acceso de Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d75d7ffe28fa126ee5e359cba19e66e3c0f36e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422397"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Revisión del acceso a los grupos o las aplicaciones en las revisiones de acceso de Azure AD

Azure Active Directory (Azure AD) simplifica el modo en que las empresas administran el acceso a los grupos o las aplicaciones en Azure AD y otras instancias de Microsoft Online Services, con una característica denominada revisiones de acceso de Azure AD.

En este artículo se describe cómo revisar su propio acceso a un grupo o una aplicación.

## <a name="open-the-access-review"></a>Abrir la revisión de acceso

El primer paso para realizar una revisión de acceso es buscar y abrir la revisión de acceso.

1. Busque un correo electrónico de Microsoft que le pide que revise el acceso. Este es un ejemplo de un correo electrónico para revisar su acceso a un grupo.

    ![Ejemplo de correo electrónico de Microsoft para revisar su acceso a un grupo](./media/review-your-access/access-review-email.png)

1. Haga clic en el vínculo **Review access** (Revisar acceso) para abrir la revisión de acceso.

Si no tiene el correo electrónico, puede encontrar las revisiones de acceso pendientes mediante estos pasos.

1. Inicie sesión en el portal de aplicaciones en [https://myapps.microsoft.com](https://myapps.microsoft.com).

    ![Portal de aplicaciones con las aplicaciones para las que tiene permiso](./media/review-your-access/myapps-access-panel.png)

1. En la esquina superior derecha de la página, haga clic en el símbolo de usuario que muestra la organización predeterminada y el nombre de usuario. Si aparece más de una organización, seleccione la que haya solicitado una revisión de acceso.

1. En el lado derecho de la página, haga clic en el icono **Access reviews** (Revisiones de acceso) para ver una lista de las revisiones de acceso pendientes.

    Si el icono no está visible, no hay ninguna revisión de acceso para esa organización y no se requiere ninguna acción en este momento.

    ![Lista de revisiones de acceso pendientes para aplicaciones y grupos](./media/review-your-access/access-reviews-list.png)

1. Haga clic en el vínculo **Begin review** (Comenzar revisión) correspondiente a la revisión de acceso que quiere realizar.

## <a name="perform-the-access-review"></a>Realización de la revisión de acceso

Una vez que ha abierto la revisión de acceso, puede ver los derechos de acceso.

1. Revise su acceso y decida si todavía lo necesita.

    Si la solicitud es revisar el acceso de otros usuarios, la página se verá diferente. Para obtener más información, consulte [Revisar el acceso a grupos o aplicaciones](perform-access-review.md).

    ![Abrir la revisión de acceso que le pregunta si aún necesita acceso a un grupo](./media/review-your-access/perform-access-review.png)

1. Seleccione **Yes** (Sí) para mantener el acceso o **No** para quitarlo.

1. Si selecciona **Yes** (Sí), es posible que necesite especificar una justificación en el cuadro **Reason** (Motivo).

    ![Revisión de acceso completada que pregunta si todavía necesita acceso a un grupo](./media/review-your-access/perform-access-review-submit.png)

1. Haga clic en **Enviar**.

    Su selección se envía y vuelve al portal de aplicaciones.

    Si quiere cambiar su respuesta, vuelva a abrir la página de revisiones de acceso y actualícela. Puede cambiar su respuesta en cualquier momento hasta que finalice la revisión de acceso.

    > [!NOTE]
    > Si indicó que ya no necesita acceso, este no se eliminará de inmediato. Se quitará cuando la revisión haya finalizado o cuando un administrador la detenga.

## <a name="next-steps"></a>Pasos siguientes

- [Completar una revisión de acceso de grupos o aplicaciones](complete-access-review.md)
