---
title: Finalizar el acceso a la revisión de los roles de recursos de Azure en PIM-Azure AD | Microsoft Docs
description: Aprenda a completar una revisión de acceso de los roles de recursos de Azure Privileged Identity Management en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e45249245aaab97070b7e774d4b6bab6827bdc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74021996"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Complete una revisión de acceso de los roles de recursos de Azure en Privileged Identity Management

Los administradores de roles con privilegios pueden revisar el acceso con privilegios después de [iniciar una revisión de acceso](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) en Azure Active Directory (Azure AD) envía automáticamente un correo que solicita a los usuarios que revisen su acceso. Si un usuario no recibe un correo electrónico, puede enviarle las instrucciones necesarias para [realizar una revisión del acceso](pim-resource-roles-perform-access-review.md).

Cuando acabe el período de la revisión del acceso o una vez que todos los usuarios hayan finalizado su autorrevisión, siga los pasos de este artículo para administrar la revisión y ver los resultados.

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso

1. Vaya a [Azure Portal](https://portal.azure.com/). En el panel, seleccione el servicio de **recursos de Azure**.

2. Seleccione el recurso.

3. Seleccione la sección **Revisiones de acceso** del panel.

    ![Recursos de Azure: lista de revisiones de acceso que muestra el rol, el propietario, la fecha de inicio, la fecha de finalización y el estado](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Seleccione la revisión de acceso que quiere administrar.

En la página de detalles de la revisión de acceso, hay una serie de opciones para administrar dicha revisión. Las opciones son las siguientes:

![Opciones para administrar una revisión: Detener, Restablecer, Aplicar, Eliminar](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Stop

Todas las revisiones de acceso tienen una fecha de finalización. Seleccione **Detener** para finalizarla pronto. Los usuarios que no hayan finalizado su revisión en este momento no podrán finalizarla después de detener la revisión. No se puede reiniciar una revisión después de detenerla.

### <a name="reset"></a>Reset

Puede restablecer una revisión de acceso para quitar todas las decisiones que se tomaron en ella. Después de restablecer una revisión de acceso, todos los usuarios se marcan como no revisados de nuevo.

### <a name="apply"></a>Aplicar

Una vez finalizada la revisión de acceso, seleccione **Aplicar** para implementar el resultado de la revisión. Si se denegó el acceso de un usuario en la revisión, este paso quitará su asignación de rol.  

### <a name="delete"></a>Eliminar

Si ya no le interesa más la revisión, elimínela. Seleccione **Eliminar** para quitar la revisión del servicio de Privileged Identity Management.

## <a name="results"></a>Results

En la página **Resultados**, consulte y descargue una lista de los resultados de la revisión.

![Página de resultados que muestra los usuarios, el resultado, quién realizó la revisión, quién la aplicó y la aplicación del resultado](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisores

Vea y agregue revisores a la revisión de acceso existente. Recuerde a los revisores que realicen sus revisiones.

![Página de revisores que muestra el nombre y el nombre principal de usuario](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Pasos siguientes

- [Inicio de una revisión de acceso para los roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-start-access-review.md)
- [Realice una revisión de acceso de mis roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-perform-access-review.md)
