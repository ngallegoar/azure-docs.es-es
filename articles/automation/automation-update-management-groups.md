---
title: Uso de grupos dinámicos con Update Management de Azure Automation
description: En este artículo se describe cómo funcionan los grupos dinámicos con Update Management de Azure Automation.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: 29a72eb1fe7b8be18cd2160fc63160e408378585
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617440"
---
# <a name="use-dynamic-groups-with-update-management"></a>Uso de grupos dinámicos con Update Management

Update Management permite tener como destino un grupo dinámico de VM de Azure (o de VM que no sean de Azure) para implementaciones de actualizaciones. Estos grupos, definidos por consultas, se evalúan en tiempo de implementación, por lo que no tiene que editar la implementación para agregar máquinas.

## <a name="azure-machines"></a>Máquinas de Azure

Los grupos dinámicos no funcionan con máquinas virtuales clásicas. Al definir la consulta, se pueden usar los siguientes elementos juntos para rellenar un grupo dinámico:

* Subscription
* Grupos de recursos
* Ubicaciones
* Etiquetas

![Selección de grupos](./media/automation-update-management/select-groups.png)

Para obtener una versión preliminar de los resultados de un grupo dinámico, haga clic en **Versión preliminar**. La versión preliminar muestra la pertenencia al grupo en el momento actual. En el ejemplo, buscamos máquinas que tengan la etiqueta `Role` para el grupo **BackendServer**. Si se ha agregado esta etiqueta a varias máquinas, se agregan a las implementaciones futuras en ese grupo.

![vista previa de grupos](./media/automation-update-management/preview-groups.png)

## <a name="non-azure-machines"></a>Máquinas que no son de Azure

En el caso de las máquinas que no son de Azure, las búsquedas guardadas, a las que también se hace referencia como grupos de equipos, se usan para crear el grupo dinámico. Para aprender a crear una búsqueda guardada, consulte [Creación de un grupo de equipos](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Una vez creado el grupo, puede seleccionarlo en la lista de búsquedas guardadas. Haga clic en **Vista previa** para obtener una vista previa de los equipos de la búsqueda guardada en ese momento.

![Selección de grupos](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Pasos siguientes

Después de crear un grupo dinámico, puede [crear una implementación de actualizaciones](automation-tutorial-update-management.md).
