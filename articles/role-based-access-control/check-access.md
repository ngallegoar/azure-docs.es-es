---
title: 'Inicio rápido: Visualización del acceso de un usuario a los recursos de Azure (Azure RBAC)'
description: En este inicio rápido, aprenderá a ver el acceso que un usuario u otra entidad de seguridad tienen a los recursos de Azure mediante Azure Portal y el control de acceso basado en rol (RBAC).
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9be53aa964e75bab0b90495640537fe927a5af0e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "82734168"
---
# <a name="quickstart-view-the-access-a-user-has-to-azure-resources"></a>Inicio rápido: Visualización del acceso de un usuario a los recursos de Azure

Puede usar la hoja **Control de acceso (IAM)** en [Control de acceso basado en rol (RBAC)](overview.md) para ver el acceso que un usuario u otra entidad de seguridad tienen a los recursos de Azure. Sin embargo, a veces basta ver rápidamente el acceso de un único usuario o de otra entidad de seguridad. La manera más fácil de hacerlo es usar la característica **Comprobar acceso** en Azure Portal.

## <a name="view-role-assignments"></a>Visualización de asignaciones de roles

 La forma de ver el acceso de un usuario es para enumerar las asignaciones de roles. Siga estos pasos para ver las asignaciones de roles de un solo usuario, grupo, entidad de servicio o identidad administrada en el ámbito de la suscripción.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Comprobar acceso**.

    ![Control de acceso: pestaña Comprobar acceso](./media/check-access/access-control-check-access.png)

1. En la lista **Buscar**, seleccione el tipo de entidad de seguridad cuyo acceso quiere comprobar.

1. En el cuadro de búsqueda, escriba una cadena para buscar nombres para mostrar, direcciones de correo electrónico o identificadores de objeto en el directorio.

    ![Lista de selección de Comprobar acceso](./media/check-access/check-access-select.png)

1. Haga clic en la entidad de seguridad para abrir el panel **Asignaciones**.

    ![Panel de asignaciones](./media/check-access/check-access-assignments.png)

    En este panel puede ver los roles asignados a la entidad de seguridad seleccionada y el ámbito. Si hay asignaciones denegadas en este ámbito o heredadas en este ámbito, se mostrarán.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante Azure Portal](quickstart-assign-role-user-portal.md)
