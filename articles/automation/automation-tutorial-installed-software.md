---
title: Detección del software instalado en sus máquinas virtuales con Azure Automation | Microsoft Docs
description: Este artículo describe el software instalado en las máquinas virtuales de su entorno.
services: automation
keywords: inventario, automatización, seguimiento de cambios
ms.date: 04/11/2018
ms.topic: tutorial
ms.subservice: change-inventory-management
ms.custom: mvc
ms.openlocfilehash: d4acecbc6d1a1d7f617b0da95da1b97dc5a3dd75
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743663"
---
# <a name="discover-what-software-is-installed-on-your-vms"></a>Detección del software instalado en sus máquinas virtuales

En este tutorial, aprenderá a usar la característica Change Tracking e Inventario de Azure Automation para identificar el software instalado en su entorno. Puede recopilar y ver el inventario de software, archivos, demonios de Linux, servicios de Windows y claves del Registro de Windows en los equipos. Realizar un seguimiento de las configuraciones de sus máquinas puede ayudarle a identificar problemas de funcionamiento en el entorno y comprender mejor el estado de las máquinas.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Habilitación de Change Tracking e Inventario
> * Habilitación de una máquina virtual de Azure
> * Habilitación de una máquina que no es de Azure
> * Visualizar el software instalado
> * Buscar software instalado en los registros del inventario

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

* Suscripción a Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para que contenga los runbooks de monitor y de acción, y la tarea de monitor.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para habilitar la característica.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

En primer lugar, debe habilitar Change Tracking e Inventario para este tutorial. Si ya habilitó la característica, este paso no es necesario.

>[!NOTE]
>Si los campos aparecen atenuados, significa que otra característica de Automation está habilitada para la máquina virtual, y debe utilizar la misma área de trabajo y cuenta de Automation.

1. Vaya a la cuenta de Automation y seleccione **Inventario** o **Change Tracking** en **Administración de configuración**.

2. Elija el área de trabajo de [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json). Esta área de trabajo recopila datos generados por características como Change Tracking e Inventario. El área de trabajo proporciona una única ubicación para revisar y analizar datos desde varios orígenes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

3. Seleccione la cuenta de Automation que desea utilizar.

4. Configure la ubicación para la implementación.

5. Haga clic en **Habilitar** para implementar la característica en la máquina virtual. 

    ![Banner de la configuración de inventario](./media/automation-tutorial-installed-software/enableinventory.png)

Durante la configuración, la máquina virtual se aprovisiona con el agente de Log Analytics para Windows y un [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md). La habilitación de Change Tracking e Inventario puede tardar hasta 15 minutos. Durante este tiempo, no debería cerrar la ventana del explorador.

Después de habilitar la característica, la información sobre el software instalado y los cambios en la máquina virtual se transfiere a los registros de Azure Monitor. Los datos pueden tardar entre 30 minutos y 6 horas en estar disponibles para el análisis.

## <a name="add-an-azure-vm-to-change-tracking-and-inventory"></a>Incorporación de una máquina virtual de Azure a Change Tracking e Inventario

1. En la cuenta de Automation, vaya a **Inventario** o **Change Tracking**, en **Administración de configuración**.

2. Seleccione **Agregar máquina virtual de Azure**.

3. Seleccione su máquina virtual en la lista. 

4. Haga clic en **Habilitar** para habilitar la característica Change Tracking e Inventario en la máquina virtual. El agente de Log Analytics para Windows se implementa en la máquina virtual y la configura para que se comunique con el área de trabajo de Log Analytics. La operación de configuración puede tardar unos minutos. 

5. En este punto, si lo desea, puede seleccionar una nueva máquina virtual de la lista para habilitar la característica.

## <a name="add-a-non-azure-machine-to-change-tracking-and-inventory"></a>Incorporación de una máquina que no es de Azure a la característica Change Tracking e Inventario

Para habilitar la característica en máquinas que no son de Azure:

1. Instale el [agente de Log Analytics para Windows](../azure-monitor/platform/agent-windows.md) o el [agente de Log Analytics para Linux](automation-linux-hrw-install.md) en función de su sistema operativo. 

2. Vaya a la cuenta de Automation y seleccione **Inventario** o **Change Tracking** en **Administración de configuración**. 

3. Haga clic en **Administrar máquinas**. Se mostrará una lista de las máquinas que informan al área de trabajo de Log Analytics y que no tienen habilitada la característica Change Tracking e Inventario. Seleccione la opción adecuada para su entorno:

    * **Habilitar en todas las máquinas disponibles**: esta opción habilita la característica en todas las máquinas que informan al área de trabajo de Log Analytics en la actualidad.
    * **Habilitar en todas las máquinas disponibles y futuras**: esta opción habilita la característica en todas las máquinas que informan al área de trabajo de Log Analytics y, posteriormente, a todas las máquinas futuras que se agreguen al área de trabajo.
    * **Habilitar en las máquinas seleccionadas**: esta opción habilita la característica solo en las máquinas que haya seleccionado.

    ![Administrar máquinas](./media/automation-tutorial-installed-software/manage-machines.png)

## <a name="view-installed-software"></a>Visualizar el software instalado

Una vez habilitada la característica Change Tracking e Inventario, puede ver los resultados en la página Inventario.

1. Desde la cuenta de Automation, seleccione **Inventario** en **Administración de configuración**.

2. En la página Inventario, haga clic en la pestaña **Software**.

3. Observe que el software detectado se enumera en una tabla. El software se agrupa por nombre y versión. En la tabla puede verse cada registro de software con gran detalle. Estos detalles incluyen el nombre del software, la versión, el editor, la última hora de actualización (la hora de actualización más reciente que una máquina virtual notificó al grupo) y las máquinas (el recuento de las máquinas con ese software).

    ![Inventario de software](./media/automation-tutorial-installed-software/inventory-software.png)

4. Haga clic en una fila para ver las propiedades del registro de software y los nombres de las máquinas con dicho software.

5. Para buscar un software específico o un grupo de software, vaya al cuadro de texto directamente encima de la lista de software.
El filtro le permite buscar en función del nombre del software, la versión o el editor. Por ejemplo, la búsqueda de **Contoso** devuelve todo el software con un nombre, una versión o un editor que contenga **Contoso**.

## <a name="search-inventory-logs-for-installed-software"></a>Buscar software instalado en los registros del inventario

Change Tracking e Inventario generan datos de registro que se envían a los registros de Azure Monitor. Para buscar los registros mediante la ejecución de consultas, seleccione **Log Analytics** en la parte superior de la ventana Inventario. Los datos de inventario se almacenan en el tipo `ConfigurationData`.

La siguiente consulta de ejemplo Log Analytics devuelve los resultados de inventario relativos al publicador Microsoft Corporation.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| where Publisher == "Microsoft Corporation"
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
```

Para más información sobre la ejecución y la búsqueda de archivos de registro en los registros de Azure Monitor, consulte el artículo sobre los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="see-the-software-inventory-for-a-single-machine"></a>Visualización del inventario de software para una única máquina

Para ver el inventario de software de una única máquina, puede acceder a Inventario desde la página de recursos de la máquina virtual de Azure o usar los registros de Azure Monitor para filtrar hasta encontrar la máquina correspondiente. La siguiente consulta de Log Analytics de ejemplo devuelve la lista de software para una máquina denominada **ContosoVM**.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
| summarize by Publisher, SoftwareName
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a visualizar el inventario de software:

> [!div class="checklist"]
> * Habilitación de Change Tracking e Inventario
> * Habilitación de una máquina virtual de Azure
> * Habilitación de una máquina que no es de Azure
> * Visualizar el software instalado
> * Buscar software instalado en los registros del inventario

Continúe con la información general de la característica Change Tracking e Inventario para más información.

> [!div class="nextstepaction"]
> [Información general de Change Tracking e Inventario](change-tracking.md)
