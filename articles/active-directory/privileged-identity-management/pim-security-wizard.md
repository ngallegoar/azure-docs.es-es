---
title: 'Asistente para la seguridad de los roles de Azure AD en PIM: Azure Active Directory | Microsoft Docs'
description: Describe el asistente para la seguridad que puede usar con la finalidad de convertir las asignaciones de roles de Azure AD con privilegios permanentes en aptos mediante Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/21/2020
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9f12b2b31da4c7fe67eef9674d96b517d4e2bfa
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867752"
---
# <a name="azure-ad-roles-security-wizard-in-privileged-identity-management"></a>Asistente para seguridad de roles de Azure AD en Privileged Identity Management

Si es la primera persona que usa Privileged Identity Management (PIM) en la organización de Azure Active Directory (Azure AD), se le presentará un asistente para comenzar. El asistente le ayuda a comprender los riesgos de seguridad de las identidades con privilegios y a usar Privileged Identity Management para reducir dichos riesgos. No tiene que realizar cambios en las asignaciones de roles existentes en el asistente; si lo prefiere, puede hacerlo más adelante.

> [!Important]
> El asistente de seguridad no está disponible temporalmente. Gracias por su paciencia.

## <a name="wizard-overview"></a>Información general sobre el asistente

Antes de que la organización comience a usar Privileged Identity Management, todas las asignaciones de roles son permanentes: los usuarios siempre tienen estos roles aunque en el momento actual no necesiten sus privilegios. El primer paso del asistente muestra una lista de roles con privilegios elevados y cuántos usuarios tienen actualmente esos roles. Puede explorar en profundidad un rol determinado para conocer mejor a los usuarios en caso de que alguno de ellos no le resulte familiar.

El segundo paso del asistente le ofrece la oportunidad de cambiar las asignaciones de roles del administrador.  

> [!WARNING]
> Es importante que tenga al menos un administrador global y más de un administrador de roles con privilegios con una cuenta profesional o educativa (no una cuenta de Microsoft). Si solo hay un administrador de roles con privilegios, la organización no podrá administrar Privileged Identity Management si esa cuenta se elimina.
> Además, mantenga las asignaciones de roles permanentes si un usuario tiene una cuenta de Microsoft (es decir, una cuenta que usan para iniciar sesión en servicios de Microsoft como Skype y Outlook.com). Si tiene pensado exigir la autenticación multifactor para la activación de ese rol, ese usuario se bloqueará.

## <a name="run-the-wizard"></a>Ejecutar el asistente

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD** y, luego, seleccione **Asistente**.

    ![Roles de Azure AD: página del asistente que muestra los 3 pasos para ejecutar el asistente](./media/pim-security-wizard/wizard-start.png)

1. Seleccione **1 Detectar roles con privilegios**.

1. Revise la lista de roles con privilegios para ver qué usuarios son permanentes o aptos.

    ![Detección de roles con privilegios: panel de roles que muestra los miembros permanentes y aptos](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Seleccione **Siguiente** para seleccionar los usuarios o grupos que quiere convertir en válidos.

    ![Página de conversión de los miembros en aptos con opciones para seleccionar los miembros que quiere que sean aptos para los roles](./media/pim-security-wizard/convert-members-eligible.png)

1. Una vez que haya seleccionado los usuarios o grupos, seleccione **Siguiente**.

    ![Página de revisión de cambios que muestra a los miembros con asignaciones de roles permanentes que se convertirán](./media/pim-security-wizard/review-changes.png)

1. Seleccione **Aceptar** para convertir las asignaciones permanentes en válidas.

    Cuando se complete la conversión, verá una notificación.

    ![Notificación que muestra el estado de una conversión](./media/pim-security-wizard/notification-completion.png)

Si necesita convertir otras asignaciones de roles con privilegios en aptas, puede volver a ejecutar el asistente. Si desea usar la interfaz de Privileged Identity Management en lugar del asistente, consulte [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md)
- [Concesión de acceso a otros administradores para administrar Privileged Identity Management](pim-how-to-give-access-to-pim.md)
