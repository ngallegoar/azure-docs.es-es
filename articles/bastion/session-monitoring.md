---
title: Supervisión y administración de sesiones de Azure Bastion | Microsoft Docs
description: En este artículo, aprenderá a seleccionar una sesión en curso y forzar su desconexión o eliminación.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: charwen
ms.openlocfilehash: 617ec30fc9b97e89b6ccd0de6255d65da94d7b63
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780444"
---
# <a name="session-monitoring-and-management-for-azure-bastion"></a>Supervisión y administración de sesiones de Azure Bastion

Cuando el servicio Bastion se aprovisiona e implementa en la red virtual, puede usarlo para conectarse sin problemas a cualquier máquina virtual de esta red virtual. A medida que los usuarios se conectan a las cargas de trabajo, se Azure Bastion se puede usar para supervisar las sesiones remotas y realizar acciones de administración rápidas. La supervisión de sesiones de Azure Bastion le permite ver qué usuarios están conectados a las máquinas virtuales. Muestra la dirección IP desde la que se conectó el usuario, cuánto tiempo ha estado conectado y cuándo se conectaron. La experiencia de administración de sesiones le permite seleccionar una sesión en curso y forzar su desconexión o eliminar una sesión para desconectar al usuario de la sesión en curso.

## <a name="monitor-remote-sessions"></a><a name="monitor"></a>Supervisión de sesiones remotas

1. En [Azure Portal](https://portal.azure.com), navegue hasta el recurso de Azure Bastion y seleccione **Sesiones** en la página de Azure Bastion.

   ![sesiones](./media/session-monitoring/sessions.png)
2. En el lado derecho de la página **Sesiones**, puede ver las sesiones remotas en curso.

   ![ver sesión](./media/session-monitoring/view-session.png)
3. Seleccione **Actualizar** para ver la lista actualizada de sesiones remotas. Al seleccionar Actualizar, Azure Bastion capturará la información de supervisión más reciente y la actualizará en el portal.

   ![actualizar](./media/session-monitoring/refresh.png)


## <a name="delete-or-force-disconnect-an-ongoing-remote-session"></a><a name="view"></a>Eliminar o forzar la desconexión de una sesión remota en curso

Puede seleccionar un conjunto de sesiones y forzar su desconexión. Los siguientes pasos le mostrarán cómo eliminar sesiones remotas:

1. Navegue hasta el recurso de Azure Bastion y seleccione **Sesiones** en la página de Azure Bastion.

   ![navegar](./media/session-monitoring/navigate.png)
2. Después de seleccionar Sesiones, verá una lista de sesiones remotas.

   ![lista de sesiones](./media/session-monitoring/list.png)
3. Seleccione una sesión remota específica, seleccione los tres puntos en el extremo derecho de la fila de sesión y, a continuación, seleccione **Eliminar**.

   ![delete](./media/session-monitoring/delete.png)
4. Al seleccionar Eliminar, la sesión remota se desconectará y el usuario recibirá un mensaje que diga "Ha sido desconectado" en la sesión remota.

   ![desconexión](./media/session-monitoring/disconnect.png)

## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
