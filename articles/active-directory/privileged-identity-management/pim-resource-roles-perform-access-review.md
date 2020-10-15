---
title: 'Revisión del acceso a los roles de recursos de Azure en PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a revisar el acceso a los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8091890d174ae68f012a6ec24685a0e705100f8b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84743684"
---
# <a name="review-access-to-azure-resource-roles-in-privileged-identity-management"></a>Revisión de acceso a los roles de recursos de Azure en Privileged Identity Management

Las revisiones de acceso de Privileged Identity Management (PIM) pueden ayudar a proteger el acceso a roles con privilegios en Azure Active Directory (Azure AD). En este artículo se describen los pasos necesarios para completar una revisión de las asignaciones de roles con privilegios en una revisión de acceso de Azure AD.

Si se le asigna una función administrativa, es posible que se le pida que su administrador complete una revisión de acceso para confirmar su necesidad de un rol. La solicitud de confirmación puede llegar en un correo electrónico que incluye un vínculo, o puede confirmarse en [Azure Portal](https://portal.azure.com).

Si es un administrador de roles con privilegios interesado en las revisiones de acceso, puede obtener más información en [Inicio de una revisión de acceso](pim-resource-roles-start-access-review.md).

## <a name="approve-or-deny-access"></a>Aprobación o denegación de acceso

Puede aprobar o denegar el acceso en función de si sigue usando este rol o no. Elija **Aprobar** si desea permanecer en el rol o **Denegar** si no va a volver a necesitar el acceso. El estado solo cambia después de que el revisor aplica los resultados.

Para buscar y completar la revisión de acceso, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Seleccione **Azure Active Directory** y abra **Privileged Identity Management**.
1. Seleccione **Revisar acceso**.

   ![Captura de pantalla de la aplicación Privileged Identity Management, con la hoja Revisar acceso seleccionada](media/pim-resource-roles-perform-access-review/rbac-access-review-complete.png)

1. Seleccione la revisión que quiere completar.
1. Elija **Aprobar** o **Denegar**. En el cuadro **Proporcionar un motivo**, escriba si es necesario una justificación comercial para su decisión.

   ![Captura de pantalla de la página de detalles de la revisión](media/pim-resource-roles-perform-access-review/rbac-access-review-choice.png)

## <a name="next-steps"></a>Pasos siguientes

- [Realización de una revisión de acceso de mis roles de Azure AD en Privileged Identity Management](pim-how-to-perform-security-review.md)
