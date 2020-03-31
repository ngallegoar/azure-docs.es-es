---
title: 'Detección de recursos de Azure para su administración en PIM: Azure AD | Microsoft Docs'
description: Aprenda a detectar recursos de Azure que se administran en Azure AD Privileged Identity Management (PIM).
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87da43100c7494937ddc842e0f903ba3a360959e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022885"
---
# <a name="discover-azure-resources-to-manage-in-privileged-identity-management"></a>Detección de recursos de Azure para su administración en Privileged Identity Management

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), puede mejorar la protección de los recursos de Azure. Esto resulta útil para las organizaciones que ya usan Privileged Identity Management para proteger los roles de Azure AD, así como al grupo de administración y los propietarios de la suscripción que quieren proteger los recursos de producción.

La primera vez que se configura Privileged Identity Management para recursos de Azure, es preciso detectar y seleccionar los recursos que se van a proteger con Privileged Identity Management. No hay ningún límite en cuanto al número de recursos que se pueden administrar con Privileged Identity Management. Pero se recomienda empezar por los recursos más críticos (producción).

## <a name="discover-resources"></a>Detección de recursos

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Azure resources** (Recursos de Azure).

    Si esta es la primera vez que usa Privileged Identity Management para recursos de Azure, verá la página **Detectar recursos**.

    ![Panel de detección de recursos sin recursos mostrados para la experiencia inicial](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Si otro administrador de la organización ya administra recursos de Azure en Privileged Identity Management, verá una lista de los recursos que se están administrando actualmente.

    ![Panel de detección de recursos con los recursos que se administran actualmente](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Seleccione **Detectar recursos** para iniciar la experiencia de detección.

    ![Panel de detección con los recursos que se pueden administrar, como suscripciones y grupos de administración](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. En la página **Detección**, utilice **Filtro de estado de recursos** y **Seleccionar tipo de recurso** para filtrar los grupos de administración o las suscripciones en las que tiene permiso de escritura. Empezar con **Todo** es, probablemente, más fácil.

    Solo se pueden buscar y seleccionar recursos de la suscripción o de un grupo de administración para administrarlos mediante Privileged Identity Management. Al administrar un grupo de administración o una suscripción en Privileged Identity Management, también se pueden administrar sus recursos secundarios.

1. Seleccione la casilla de verificación junto a los recursos no administrados que quiera administrar.

1. Seleccione **Administrar recurso** para empezar a administrar los recursos seleccionados.

    > [!NOTE]
    > Una vez definido un grupo de administración o suscripción como administrado, no puede ser no administrado. Esto impide que otro administrador de recursos pueda eliminar la configuración de Privileged Identity Management.

    ![Panel de detección con un recurso seleccionado y la opción Administrar recurso resaltada](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Si ve un mensaje para confirmar la incorporación del recurso seleccionado para su administración, seleccione **Sí**.

    ![Mensaje que confirma la incorporación de los recursos seleccionados para la administración](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de las opciones de rol de recursos de Azure en Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Asignación de roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-assign-roles.md)
