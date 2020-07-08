---
title: 'Revisión del acceso a los roles de Azure AD en PIM: Azure AD | Microsoft Docs'
description: Aprenda a revisar el acceso a los roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 04/24/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4096c65fa4375152f44891d4a0a5aac4b9077cf2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743854"
---
# <a name="review-access-to-azure-ad-roles-in-privileged-identity-management"></a>Revisión de acceso a los roles de AD Azure en Privileged Identity Management

Privileged Identity Management (PIM) simplifica el modo en que las empresas administran el acceso con privilegios a los recursos de Azure Active Directory (AD) y a otros servicios en línea de Microsoft como Office 365 o Microsoft Intune. Siga los pasos de este artículo para realizar una correcta autorrevisión de los roles asignados.

Si se le ha asignado un rol administrativo, el administrador de roles con privilegios de su organización le puede pedir que revise y confirme periódicamente que aún necesita ese rol para su trabajo. Puede que reciba un correo electrónico con un vínculo o puede ir directamente a [Azure Portal](https://portal.azure.com).

Si es un administrador de roles con privilegios o un administrador global y está interesado en las revisiones de acceso, puede obtener más información en [How to start an access review](pim-how-to-start-security-review.md) (Cómo iniciar una revisión de acceso).

## <a name="add-a-pim-dashboard-tile"></a>Incorporación de un icono de panel de PIM

Si no tiene el servicio Privileged Identity Management anclado al panel en Azure Portal, siga estos pasos para comenzar.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione su nombre de usuario en la esquina superior derecha de Azure Portal y seleccione la organización de Azure AD directorio donde va a trabajar.
3. Seleccione **Todos los servicios** y use el cuadro de texto Filtro para buscar **Azure AD Privileged Identity Management**.
4. Active **Anclar al panel** y haga clic en **Crear**. Se abrirá la aplicación Privileged Identity Management.

## <a name="approve-or-deny-access"></a>Aprobación o denegación de acceso

Al aprobar o denegar el acceso, lo que hace es indicar al revisor si aún usa o no este rol. Elija **Aprobar** si desea permanecer en el rol o **Denegar** si no va a volver a necesitar el acceso. Su estado no cambiará inmediatamente, hay que esperar a que el revisor aplique los resultados.
Para buscar y completar la revisión de acceso, siga estos pasos:

1. En el servicio Privileged Identity Management, seleccione **Revisar acceso con privilegios**. Si tiene revisiones de acceso pendientes, aparecerán en la hoja **Revisiones de acceso** de Azure AD.
2. Seleccione la revisión que quiere completar.
3. A no ser que haya creado la revisión, aparecerá como el único usuario en ella. Seleccione la marca de verificación junto a su nombre.
4. Elija **Aprobar** o **Denegar**. Puede que necesite incluir un motivo de su decisión en el cuadro de texto **Proporcionar una razón** .  
5. Cierre la hoja **Revisar roles de Azure AD** .

## <a name="next-steps"></a>Pasos siguientes

- [Realización de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-perform-access-review.md)
