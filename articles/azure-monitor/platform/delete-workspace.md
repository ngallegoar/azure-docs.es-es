---
title: Eliminación y recuperación de un área de trabajo de Azure Log Analytics | Microsoft Docs
description: Obtenga información para eliminar el área de trabajo de Log Analytics si creó uno en una suscripción personal o reestructure el modelo de área de trabajo.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/26/2020
ms.openlocfilehash: 3784eda2db5f375f04cdde84108a78ae277baf60
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860671"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Eliminación y recuperación de un área de trabajo de Azure Log Analytics

En este artículo se explica el concepto de eliminación temporal de áreas de trabajo de Azure Log Analytics y cómo recuperarlas.

## <a name="considerations-when-deleting-a-workspace"></a>Consideraciones al eliminar áreas de trabajo

Al eliminar un área de trabajo de Log Analytics, se realiza una operación de eliminación temporal para que se pueda recuperar el área de trabajo, incluidos los datos y los agentes conectados en un plazo de 14 días, tanto si la eliminación fue accidental como intencionada. Después del período de eliminación temporal, el recurso del área de trabajo y sus datos no podrán recuperarse: los datos se pondrán en cola para su eliminación permanente en un plazo de 30 días. El nombre del área de trabajo es "released" y puede usarlo para crear una nueva área de trabajo.

> [!NOTE]
> Si desea invalidar el comportamiento de eliminación temporal y eliminar el área de trabajo de forma permanente, siga los pasos descritos en [Eliminación permanente del área de trabajo](#permanent-workspace-delete).

Debe tener cuidado al eliminar un área de trabajo porque puede haber datos importantes y una configuración que puede afectar negativamente a las operaciones de servicio. Revise qué agentes, soluciones y otros servicios y orígenes de Azure almacenan sus datos en Log Analytics. Por ejemplo:

* Soluciones de administración
* Azure Automation
* Agentes que se ejecutan en máquinas virtuales Windows y Linux
* Agentes que se ejecutan en equipos Windows y Linux en su entorno
* System Center Operations Manager

La operación de eliminación temporal quita el recurso del área de trabajo y los permisos de los usuarios asociados. Si los usuarios están asociados a otras áreas de trabajo, podrán seguir utilizando Log Analytics con esas otras áreas de trabajo.

## <a name="soft-delete-behavior"></a>Comportamiento de eliminación temporal

La operación de eliminación del área de trabajo quita el recurso de Resource Manager del área de trabajo, pero su configuración y sus datos se conservan durante 14 días (y da la sensación de que el área de trabajo se ha eliminado). Los agentes y grupos de administración de System Center Operations Manager configurados para enviar notificaciones al área de trabajo continúan en un estado huérfano durante el período de eliminación temporal. El servicio proporciona además un mecanismo para recuperar el área de trabajo eliminada (incluidos sus datos y recursos conectados), básicamente, deshaciendo la eliminación.

> [!NOTE] 
> Tanto las soluciones instaladas como los servicios vinculados, como la cuenta de Azure Automation, se quitan permanentemente del área de trabajo en el momento de la eliminación y no se pueden recuperar. Deben volver a configurarse después de la operación de recuperación para que el área de trabajo vuelva al estado en que estaba configurado anteriormente.

Las áreas de trabajo se pueden eliminar un mediante [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [API REST](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) o en [Azure Portal](https://portal.azure.com).

### <a name="azure-portal"></a>Azure portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Áreas de trabajo de Log Analytics**.
3. En la lista de áreas de trabajo de Log Analytics, seleccione un área de trabajo y, a continuación, haga clic en **Eliminar** en la parte superior del panel intermedio.
4. Aparece una página de confirmación que muestra la ingesta de datos en el área de trabajo durante la semana pasada. Escriba el nombre del área de trabajo que desea confirmar y, a continuación, haga clic en **Eliminar**.

   ![Confirmación de la eliminación del área de trabajo](media/delete-workspace/workspace-delete.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

## <a name="permanent-workspace-delete"></a>Eliminación permanente del área de trabajo
Es posible que el método de eliminación temporal no encaje en algunos escenarios como el desarrollo y las pruebas, donde es necesario repetir una implementación con la misma configuración y el mismo nombre de área de trabajo. En tales casos, puede eliminar el área de trabajo de forma permanente e "invalidar" el período de eliminación temporal. La operación de eliminación permanente del área de trabajo libera el nombre del área de trabajo y puede crear una nueva área de trabajo con el mismo nombre.


> [!IMPORTANT]
> Use la operación de eliminación permanente de áreas de trabajo con precaución porque es irreversible y no podrá recuperar el área de trabajo ni sus datos.

Para eliminar el área de trabajo de forma permanente, use la solicitud REST [Áreas de trabajo: eliminar](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) con una etiqueta de forzado:

```rst
DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview&force=true
Authorization: Bearer <token>
```

Como alternativa, puede ejecutar la operación desde el sitio de documentación de REST de Azure:
1.  Vaya a la API de REST [Áreas de trabajo: eliminar](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) y haga clic en **Probar**. 
2.  Escriba los detalles del área de trabajo que desea eliminar permanentemente
3.  Finalice un parámetro nuevo *force* con valor *true*
4.  Haga clic en el icono "+" situado a la derecha del valor. Esto agregará *force = true* al URI de la solicitud
5.  Haga clic en el botón *Ejecutar*

La respuesta debe ser 200 OK

## <a name="recover-workspace"></a>Recuperación de áreas de trabajo
Al eliminar un área de trabajo de Log Analytics accidental o intencionadamente, el servicio coloca el área de trabajo en un estado de eliminación temporal, lo que hace que sea inaccesible para cualquier operación. El nombre del área de trabajo eliminada se conserva durante el periodo de eliminación temporal y no se puede para crear un área de trabajo nueva. Después del período de eliminación temporal, el área de trabajo no es recuperable, está programada para su eliminación permanente y su nombre se ha liberado y se puede usar para crear una nueva área de trabajo.

Puede recuperar el área de trabajo durante el período de eliminación temporal, incluidos los datos, la configuración y los agentes conectados. Necesita tener permisos de colaborador en la suscripción y el grupo de recursos donde se localizó el área de trabajo antes de la operación de eliminación temporal. La recuperación del área de trabajo se realiza mediante la creación de un área de trabajo de Log Analytics con los detalles del área de trabajo eliminada, incluidos los siguientes:

- Id. de suscripción
- Nombre del grupo de recursos
- Nombre del área de trabajo
- Region

### <a name="azure-portal"></a>Azure portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Áreas de trabajo de Log Analytics**. Verá la lista de áreas de trabajo que tiene en el ámbito seleccionado.
3. Haga clic en **Recuperar** en el menú superior izquierdo para abrir una página con áreas de trabajo en estado de eliminación temporal que se puedan recuperar.

   ![Recuperación de áreas de trabajo](media/delete-workspace/recover-menu.png)

4. Seleccione el área de trabajo y haga clic en **Recuperar** para recuperar dicha área de trabajo.

   ![Recuperación de áreas de trabajo](media/delete-workspace/recover-workspace.png)


### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

El área de trabajo y todos sus datos se restauran después de la operación de recuperación. Las soluciones y los servicios vinculados se quitaron permanentemente del área de trabajo cuando se eliminó y se deben volver a configurar para restaurarla a su estado configurado anterior. Puede que algunos de los datos no estén disponibles para la consulta después de la recuperación del área de trabajo hasta que se vuelvan a instalar las soluciones asociadas y se agreguen sus esquemas al área de trabajo.

> [!NOTE]
> * Al volver a crear un área de trabajo durante el período de eliminación temporal, se indica que este nombre de área de trabajo ya está en uso. 
 
### <a name="troubleshooting"></a>Solución de problemas
Debe tener al menos permisos de *Colaborador de Log Analytics* para eliminar un área de trabajo.<br>
Si recibe el mensaje de error *El nombre del área de trabajo ya está en uso* o se produce un *conflicto* al crear un área de trabajo, puede deberse a:
* el nombre del área de trabajo no está disponible y lo está usando alguien de su organización u otro cliente.
* El área de trabajo se ha eliminado en los últimos 14 días y su nombre se mantiene reservado durante el período de eliminación temporal. Para invalidar la eliminación temporal y eliminar inmediatamente el área de trabajo y crear una con el mismo nombre, siga estos pasos para recuperar el área de trabajo primero y realizar una eliminación permanente:<br>
   1. [Recuperar](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#recover-workspace) el área de trabajo.
   2. [Eliminar permanentemente](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#permanent-workspace-delete) el área de trabajo.
   3. Crear una nueva área de trabajo con el mismo nombre.
