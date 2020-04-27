---
title: Migración de las implementaciones de actualizaciones de OMS a Azure
description: En este artículo se describe cómo migrar las implementaciones de actualizaciones de OMS a Azure
services: automation
ms.subservice: update-management
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 910f284eedbf50be5b58b6c18f02e50adda35e9a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680000"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>Migración de las implementaciones de actualizaciones de OMS a Azure

El portal de Operations Management Suite (OMS) está quedando [en desuso](../azure-monitor/platform/oms-portal-transition.md). Toda la funcionalidad que estaba disponible en el portal de OMS para Update Management está disponible en Azure Portal, a través de los registros de Azure Monitor. En este artículo se proporciona la información necesaria para la migración a Azure Portal.

## <a name="key-information"></a>Información importante

* Las implementaciones existentes seguirán funcionando. Una vez que haya vuelto a crear la implementación en Azure, podrá eliminar la implementación anterior de OMS.
* Todas las características existentes que tenía en OMS están disponibles en Azure; para más información sobre Update Management, consulte [Introducción a Update Management](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Acceso a Azure Portal

En el área de trabajo de OMS, haga clic en **Abrir en Azure**. Esta selección permite navegar al área de trabajo de Log Analytics usada en OMS.

![Abrir en Azure: portal de OMS](media/migrate-oms-update-deployments/link-to-azure-portal.png)

En Azure Portal, haga clic en **Cuenta de Automation**

![Registros de Azure Monitor](media/migrate-oms-update-deployments/log-analytics.png)

En su cuenta de Azure Automation, haga clic en **Update Management**.

![Administración de actualizaciones](media/migrate-oms-update-deployments/azure-automation.png)

En Azure Portal, seleccione **Cuentas de Automation** en **Todos los servicios**. 

En **Herramientas de administración**, seleccione la cuenta de Automation adecuada y haga clic en **Update Management**.

## <a name="recreate-existing-deployments"></a>Nueva creación de las implementaciones existentes

Todas las implementaciones de actualizaciones creadas en el portal de OMS tienen una [búsqueda guardada](../azure-monitor/platform/computer-groups.md) que también se conoce como un grupo de equipos, con el mismo nombre que la implementación de actualizaciones existente. La búsqueda guardada incluye la lista de las máquinas que se programaron en la implementación de actualizaciones.

![Administración de actualizaciones](media/migrate-oms-update-deployments/oms-deployment.png)

Para usar esta búsqueda guardada existente, siga estos pasos:

Para crear una implementación de actualizaciones, vaya a Azure Portal, seleccione la cuenta de Automation que se usa y haga clic en **Update Management**. Haga clic en **Programar implementación de actualizaciones**.

![Programación de la implementación de actualizaciones](media/migrate-oms-update-deployments/schedule-update-deployment.png)

Se abre el panel Nueva implementación de actualización. Escriba valores para las propiedades descritas en la tabla siguiente y haga clic en **Crear**:

En **Máquinas para actualizar**, seleccione la búsqueda guardada que se usó en la implementación de OMS existente.

| Propiedad | Descripción |
| --- | --- |
|Nombre |Nombre único para identificar la implementación de actualizaciones. |
|Sistema operativo| Seleccione **Linux** o **Windows**.|
|Máquinas para actualizar |Seleccione una búsqueda guardada, un grupo importado o elija la máquina en la lista desplegable y seleccione equipos individuales. Si elige **Máquinas**, la preparación de la máquina se muestra en la columna **PREPARACIÓN DE ACTUALIZACIONES DEL AGENTE**.</br> Para información sobre los distintos métodos de creación de grupos de equipos en los registros de Azure Monitor, consulte el artículo sobre los [Grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md) |
|Clasificaciones de actualizaciones|Seleccione todas las clasificaciones de actualizaciones que necesite. CentOS no admite estas clasificaciones de forma nativa.|
|Actualizaciones para excluir|Escriba las actualizaciones que desea excluir. En Windows, escriba el artículo de la KB sin el prefijo **KB**. En Linux, escriba el nombre del paquete o use un carácter comodín.  |
|Configuración de programación|Seleccione la hora de inicio y seleccione **Una vez** o **Periódico** para la periodicidad. | 
| Ventana de mantenimiento |Número de minutos establecido para las actualizaciones. El valor no puede ser inferior a 30 minutos ni superior a 6 horas. |
| Control de reinicio| Determina cómo se deben controlar los reinicios.</br>Las opciones disponibles son la siguientes:</br>Reboot if required (Default) [Reiniciar si es necesario (predeterminada)]</br>Always reboot (Reiniciar siempre)</br>Never reboot (No reiniciar nunca)</br>Only reboot (solo reiniciar), no se instalarán las actualizaciones|

Haga clic en **Implementaciones de actualizaciones programadas** para ver el estado de la implementación de actualizaciones recién creada.

![nueva implementación de actualizaciones](media/migrate-oms-update-deployments/new-update-deployment.png)

Como se mencionó anteriormente, una vez configuradas las implementaciones nuevas a través de Azure Portal, las implementaciones existentes se pueden quitar del portal de OMS.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Update Management en Azure, consulte [Update Management](automation-update-management.md).
