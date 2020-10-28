---
title: Creación de alertas para Seguimiento de cambios e inventario de Azure Automation
description: En este artículo se explica cómo configurar alertas de Azure para notificar el estado de los cambios detectados por Seguimiento de cambios e inventario.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/15/2020
ms.topic: conceptual
ms.openlocfilehash: 275e57e5dcf173e8d5f30f262641b02698910422
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209314"
---
# <a name="how-to-create-alerts-for-change-tracking-and-inventory"></a>Creación de alertas para Seguimiento de cambios e inventario

Las alertas de Azure le notifican proactivamente los resultados de los trabajos de runbook, los problemas de estado del servicio u otros escenarios relacionados con su cuenta de Automation. Azure Automation no incluye las reglas de alerta preconfiguradas, pero puede crear las suyas propias en función de los datos que recopila. En este artículo se proporcionan instrucciones sobre cómo crear reglas de alerta basadas en los cambios identificados por Seguimiento de cambios e inventario.

Si no está familiarizado con las alertas en Azure Monitor, consulte [Información general sobre las alertas en Microsoft Azure](../../azure-monitor/platform/alerts-overview.md) antes de empezar. Para más información acerca de las alertas que usan consultas de registro, consulte [Alertas de registro en Azure Monitor](../../azure-monitor/platform/alerts-unified-log.md).

## <a name="create-alert"></a>Crear una alerta

En el ejemplo siguiente se muestra que el archivo **c:\windows\system32\drivers\etc\hosts** se ha modificado en un equipo. Este archivo es importante porque Windows lo usa para resolver los nombres de host en direcciones IP. Esta operación tiene prioridad sobre DNS y puede provocar problemas de conectividad. También puede provocar el redireccionamiento del tráfico a sitios web malintencionados o peligrosos.

![Gráfico en el que se muestra el cambio del archivo de hosts](./media/configure-alerts/changes.png)

Vamos a usar este ejemplo para describir los pasos para crear alertas sobre un cambio.

1. En la página **Seguimiento de cambios** de la cuenta de Automation, seleccione **Log Analytics** .

2. En la búsqueda de registros, busque los cambios de contenido del archivo **hosts** con la consulta `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Esta consulta busca cambios de contenido para los archivos con nombres de ruta de acceso completos que contengan la palabra `hosts`. También puede solicitar un archivo específico si cambia la parte de la ruta de acceso a su forma completa, por ejemplo con `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`.

3. Una vez que la consulta devuelva los resultados, seleccione **Nueva regla de alertas** en la búsqueda de registros para abrir la página **Alert creation** (Creación de alertas). También puede acceder a esta página mediante **Azure Monitor** en Azure Portal.

4. Vuelva a comprobar la consulta y modifique la lógica de la alerta. En este caso, desea que la alerta se desencadene incluso aunque se detecte un solo cambio en todas las máquinas del entorno.

    ![Cambio en la consulta de seguimiento de cambios en el archivo hosts](./media/configure-alerts/change-query.png)

5. Después de establecer la lógica de la alerta, asigne grupos de acciones para actuar en respuesta a cuando la alerta se desencadene. En este caso, vamos a configurar los correos electrónicos que se enviarán y el vale de Administración de servicios de TI (ITSM) que se va a crear.

Realice los pasos siguientes para configurar alertas que le permitan saber el estado de una implementación de actualizaciones. Si no está familiarizado con las alertas de Azure, consulte la [información general sobre alertas de Azure](../../azure-monitor/platform/alerts-overview.md).

## <a name="configure-action-groups-for-your-alerts"></a>Configuración de grupos de acciones para las alertas

Una vez que haya configurado las alertas, puede configurar un grupo de acciones para usarlas con varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks y mucho más. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../../azure-monitor/platform/action-groups.md).

1. Seleccione una alerta y, a continuación, **Crear nueva** en **Grupos de acciones** .

2. Escriba un nombre completo y un nombre corto para el grupo de acciones. Update Management usa el nombre corto en lugar del nombre completo para enviar notificaciones mediante el grupo especificado.

3. En **Acciones** , escriba un nombre para especificar la acción (por ejemplo, **Notificación por correo electrónico** ).

4. En **Tipo de acción** , seleccione el tipo adecuado (por ejemplo, **Correo electrónico, SMS, Push o Voz** ).

5. Seleccione **Editar detalles** .

6. Escriba el tipo de acción en el panel. Por ejemplo, si usa la opción **Correo electrónico, SMS, Push o Voz** , escriba un nombre de acción, seleccione la casilla **Correo electrónico** , escriba una dirección de correo electrónico válida y, por último, haga clic en **Aceptar** .

    ![Configuración de un grupo de acciones de correo electrónico](./media/configure-alerts/configure-email-action-group.png)

7. En el panel Agregar grupo de acciones, seleccione **Aceptar** .

8. Para un correo electrónico de alerta, puede personalizar el asunto. Seleccione **Personalizar acciones** en **Crear regla** y, a continuación, seleccione **Asunto del correo electrónico** .

9. Cuando finalice, seleccione **Crear regla de alertas** .

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [alertas en Azure Monitor](../../azure-monitor/platform/alerts-overview.md).

* Obtenga información sobre las [consultas de registros](../../azure-monitor/log-query/log-query-overview.md) para recuperar y analizar datos provenientes de un área de trabajo de Log Analytics.

* En [Administrar el uso y los costos con los registros de Azure Monitor](../../azure-monitor/platform/manage-cost-storage.md) se describe cómo controlar los costos cambiando el período de retención de datos, y cómo analizar y alertar sobre el uso de los datos.