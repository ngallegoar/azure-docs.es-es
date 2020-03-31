---
title: Reenvío de datos de informes de Azure Automation State Configuration a registros de Azure Monitor
description: En este artículo se muestra cómo enviar datos de los informes de Desired State Configuration (DSC) desde Azure Automation State Configuration a registros de Azure Monitor para ofrecer mayor información y administración.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 578fcf4cd03a2d4fc8400b9e84f53206750a588c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77430727"
---
# <a name="forward-azure-automation-state-configuration-reporting-data-to-azure-monitor-logs"></a>Reenvío de datos de informes de Azure Automation State Configuration a registros de Azure Monitor

Azure Automation State Configuration conserva los datos de estado del nodo durante 30 días.
Puede enviar los datos de estado del nodo al área de trabajo de Log Analytics si prefiere conservar esos datos durante un período más largo.
El estado de cumplimiento se puede ver en Azure Portal, o con PowerShell, tanto para los nodos como para los recursos de DSC individuales en las configuraciones de los nodos.
Con los registros de Azure Monitor, puede:

- Obtener información de cumplimiento de nodos administrados y recursos individuales
- Desencadenar un correo electrónico o una alerta basados en el estado de cumplimiento
- Escribir consultas avanzadas en los nodos administrados
- Correlacionar el estado de cumplimiento en las cuentas de Automation
- Visualizar el historial de cumplimiento de los nodo con el paso del tiempo

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Prerequisites

Para empezar a enviar los informes de Automation State Configuration a los registros de Azure Monitor, necesita:

- La versión de noviembre de 2016, o una posterior, de [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
- Una cuenta de Azure Automation Para más información, consulte [Introducción a Azure Automation](automation-intro.md).
- Un área de trabajo de Log Analytics con una oferta de servicio de Automation & Control. Para más información, consulte [Introducción a los análisis de registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).
- Al menos un nodo de Azure Automation State Configuration. Para más información, consulte [Incorporación de máquinas para administrarlas con State Configuration de Azure Automation](automation-dsc-onboarding.md).
- Módulo [xDscDiagnostics](https://www.powershellgallery.com/packages/xDscDiagnostics/2.7.0.0), versión 2.7.0.0 o posterior. Para conocer los pasos necesarios para la instalación, consulte [Solución de problemas de Desired State Configuration (DSC) de Azure Automation](./troubleshoot/desired-state-configuration.md).

## <a name="set-up-integration-with-azure-monitor-logs"></a>Configuración de la integración con registros de Azure Monitor

Para comenzar a importar datos de DSC de Azure Automation en los registros de Azure Monitor, haga lo siguiente:

1. Inicie sesión en su cuenta de Azure en PowerShell. Consulte [Inicio de sesión con Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
1. Para obtener el identificador de recurso de la cuenta de Automation, ejecute el siguiente cmdlet de PowerShell. Si tiene más de una cuenta de Automation, elija el identificador de recurso de la cuenta que desea configurar.

   ```powershell
   # Find the ResourceId for the Automation account
   Get-AzResource -ResourceType 'Microsoft.Automation/automationAccounts'
   ```

1. Para obtener el identificador de recurso del área de trabajo de Log Analytics, ejecute el siguiente cmdlet de PowerShell. Si tiene más de un área de trabajo, elija el identificador de recurso del área de trabajo que desea configurar.

   ```powershell
   # Find the ResourceId for the Log Analytics workspace
   Get-AzResource -ResourceType 'Microsoft.OperationalInsights/workspaces'
   ```

1. Ejecute el siguiente cmdlet de PowerShell, pero reemplace `<AutomationResourceId>` y `<WorkspaceResourceId>` por los valores de *ResourceId* valores de cada uno de los pasos anteriores.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Category 'DscNodeStatus'
   ```

1. Si desea detener la importación de datos de State Configuration de Azure Automation en los registros de Azure Monitor, ejecute el siguiente cmdlet de PowerShell.

   ```powershell
   Set-AzDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Category 'DscNodeStatus'
   ```

## <a name="view-the-state-configuration-logs"></a>Visualización de los registros de State Configuration

Después de configurar la integración con los registros de Azure Monitor de sus datos de State Configuration de Automation, seleccione **Registros** en la sección **Supervisión** del panel izquierdo de la página State Configuration (DSC) para consultarlos.

![Registros](media/automation-dsc-diagnostics/automation-dsc-logs-toc-item.png)

Se abre el panel **Búsqueda de registros** con una región de consulta cuyo ámbito es el recurso de la cuenta de Automation. Puede buscar las operaciones de DSC en los registros de State Configuration buscando en los registros de Azure Monitor. Los registros de las operaciones de DSC se almacenan en la tabla AzureDiagnostics. Por ejemplo, para buscar los nodos que no son compatibles, escriba la siguiente consulta.

```AzureDiagnostics
| where Category == 'DscNodeStatus' 
| where OperationName contains 'DSCNodeStatusData'
| where ResultType != 'Compliant'
```
Detalles de filtrado:

* Filtre por *DscNodeStatusData* para devolver las operaciones de cada nodo de State Configuration.
* Filtre por *DscResourceStatusData* para devolver las operaciones de cada recurso de DSC llamado en la configuración de nodo aplicada a ese recurso. 
* Filtre por *DscResourceStatusData* para devolver información de errores de los recursos de DSC con errores.

Para más información sobre la creación de consultas de registros para buscar datos, consulte [Introducción a las consultas de registro en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

### <a name="send-an-email-when-a-state-configuration-compliance-check-fails"></a>Envío de un correo electrónico cuando se produce un error en cualquier comprobación de cumplimiento de State Configuration

Uno de nuestros principales clientes solicita la capacidad para enviar un correo electrónico o un mensaje de texto cuando aparece algún problema en una configuración de DSC.

Para crear una regla de alertas, primero debe crear una búsqueda de registros para los registros del informe de State Configuration que deben invocar la alerta. Haga clic en el botón **+ Nueva regla de alertas** para crear y configurar la regla de alerta.

1. En la página de información general del área de trabajo de Log Analytics, haga clic en **Registros**.
1. Cree una consulta de búsqueda de registros para la alerta; para ello, escriba la siguiente búsqueda en el campo de la consulta: `Type=AzureDiagnostics Category='DscNodeStatus' NodeName_s='DSCTEST1' OperationName='DscNodeStatusData' ResultType='Failed'`

   Si ha configurado registros de más de una cuenta de Automation o suscripción a su área de trabajo, puede agrupar las alertas por suscripción o cuenta de Automation. El nombre de la cuenta de Automation se deriva del campo Resource (Recurso) en la búsqueda de DscNodeStatusData.
1. Para abrir la pantalla **Crear regla**, haga clic en **+ Nueva regla de alertas** en la parte superior de la página. 

Para obtener más información acerca de las opciones para configurar la alerta, consulte [Crear una regla de alerta](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Búsqueda de recursos de DSC con errores en todos los nodos

Una ventaja de usar los registros de Azure Monitor es que puede buscar comprobaciones con errores en todos los nodos.
Para buscar todas las instancias de recursos de DSC con errores:

1. En la página de información general del área de trabajo de Log Analytics, haga clic en **Registros**.
1. Cree una consulta de búsqueda de registros para la alerta; para ello, escriba la siguiente búsqueda en el campo de la consulta: `Type=AzureDiagnostics Category='DscNodeStatus' OperationName='DscResourceStatusData' ResultType='Failed'`

### <a name="view-historical-dsc-node-status"></a>Visualización del historial de estado del nodo de DSC

Para visualizar el historial de estado del nodo de DSC con el paso del tiempo, puede usar esta consulta:

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`

Esta consulta muestra un gráfico del estado del nodo con el paso del tiempo.

## <a name="azure-monitor-logs-records"></a>Registros de Azure Monitor

Diagnósticos de Azure Automation crea dos categorías de registros en los registros de Azure Monitor:

* Datos de estado del nodo (DscNodeStatusData)
* Datos de estado del recurso (DscResourceStatusData)

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Propiedad | Descripción |
| --- | --- |
| TimeGenerated |Fecha y hora en que se ejecutó la comprobación de cumplimiento. |
| OperationName |DscNodeStatusData. |
| ResultType |Si el nodo es compatible. |
| NodeName_s |El nombre del nodo administrado. |
| NodeComplianceStatus_s |Si el nodo es compatible. |
| DscReportStatus |Si la comprobación de cumplimiento se ejecutó correctamente. |
| ConfigurationMode | Cómo se aplica la configuración al nodo. Los valores posibles son: <ul><li>*ApplyOnly*: DSC se aplica la configuración y es lo único que hace, salvo que se inserte una nueva configuración en el nodo de destino o cuando se extrae una nueva configuración de un servidor. Después de la aplicación inicial de una nueva configuración, DSC no comprueba si se ha producido una desviación desde un estado configurado previamente. DSC intenta aplicar la configuración hasta que sea la correcta antes de que el valor de *ApplyOnly* surta efecto. </li><li>*ObjectID*: es el valor predeterminado. El LCM aplica las nuevas configuraciones. Después de la aplicación inicial de una nueva configuración, si el nodo de destino se desvía del estado deseado, DSC notifica la discrepancia en los registros. DSC intenta aplicar la configuración hasta que sea la correcta antes de que el valor de *ApplyAndMonitor* surta efecto.</li><li>*ApplyAndAutoCorrect*: DSC aplica las configuraciones nuevas. Después de la aplicación inicial de una nueva configuración, si el nodo de destino se desvía del estado deseado, DSC notifica la discrepancia en los registros y vuelve a aplicar la configuración actual.</li></ul> |
| HostName_s | El nombre del nodo administrado. |
| IPAddress | La dirección IPv4 del nodo administrado. |
| Category | DscNodeStatus. |
| Resource | El nombre de la cuenta de Azure Automation. |
| Tenant_g | GUID que identifica al inquilino para el llamador. |
| NodeId_g |GUID que identifica el nodo administrado. |
| DscReportId_g |GUID que identifica el informe. |
| LastSeenTime_t |Fecha y hora en que el informe se vio por última vez. |
| ReportStartTime_t |Fecha y hora en que el informe se inició. |
| ReportEndTime_t |Fecha y hora en que el informe se completó. |
| NumberOfResources_d |El número de recursos de DSC que se llama en la configuración que se aplica al nodo. |
| SourceSystem | Cómo los registros de Azure Monitor recopilan los datos. Siempre "Azure" para Diagnósticos de Azure. |
| ResourceId |Identificador de la cuenta de Azure Automation. |
| ResultDescription | La descripción de esta operación. |
| SubscriptionId | Identificador de la suscripción de Azure (GUID) para la cuenta de Automation. |
| ResourceGroup | Nombre del grupo de recursos de la cuenta de Automation. |
| ResourceProvider | MICROSOFT.AUTOMATION. |
| ResourceType | AUTOMATIONACCOUNTS. |
| CorrelationId |GUID identificador de correlación del informe de cumplimiento. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Propiedad | Descripción |
| --- | --- |
| TimeGenerated |Fecha y hora en que se ejecutó la comprobación de cumplimiento. |
| OperationName |DscResourceStatusData|
| ResultType |Si el recurso es compatible. |
| NodeName_s |El nombre del nodo administrado. |
| Category | DscNodeStatus. |
| Resource | El nombre de la cuenta de Azure Automation. |
| Tenant_g | GUID que identifica al inquilino para el llamador. |
| NodeId_g |GUID que identifica el nodo administrado. |
| DscReportId_g |GUID que identifica el informe. |
| DscResourceId_s |El nombre de la instancia del recurso de DSC. |
| DscResourceName_s |El nombre del recurso de DSC. |
| DscResourceStatus_s |Si el recurso de DSC es compatible. |
| DscModuleName_s |El nombre del módulo de PowerShell que contiene el recurso de DSC. |
| DscModuleVersion_s |La versión del módulo de PowerShell que contiene el recurso de DSC. |
| DscConfigurationName_s |El nombre de la configuración aplicada al nodo. |
| ErrorCode_s | El código de error si se produjo un error en el recurso. |
| ErrorMessage_s |El mensaje de error si se produjo un error en el recurso. |
| DscResourceDuration_d |El tiempo, en segundos, durante el que se ejecutó el recurso de DSC. |
| SourceSystem | Cómo los registros de Azure Monitor recopilan los datos. Siempre *Azure* para Diagnósticos de Azure. |
| ResourceId |Especifica la cuenta de Azure Automation. |
| ResultDescription | La descripción de esta operación. |
| SubscriptionId | Identificador de la suscripción de Azure (GUID) para la cuenta de Automation. |
| ResourceGroup | Nombre del grupo de recursos de la cuenta de Automation. |
| ResourceProvider | MICROSOFT.AUTOMATION. |
| ResourceType | AUTOMATIONACCOUNTS. |
| CorrelationId |GUID identificador de correlación del informe de cumplimiento. |

## <a name="summary"></a>Resumen

Al enviar los datos de Automation State Configuration a los registros de Azure Monitor, puede obtener más información del estado de los nodos de Azure Automation State Configuration mediante las siguientes acciones:

- Establecimiento de alertas que le notificarán cuando haya un problema
- Uso de vistas personalizadas y de consultas de búsqueda para visualizar los resultados de runbook, el estado del trabajo de runbook y otras métricas o indicadores clave relacionados.

Los registros de Azure Monitor proporcionan mayor visibilidad operativa para los datos de Automation State Configuration y pueden ayudar a resolver incidentes con mayor rapidez.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general, consulte [Azure Automation State Configuration](automation-dsc-overview.md).
- Para empezar a usar Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md)
- Para obtener información acerca de la compilación de configuraciones de DSC para que poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones en Azure Automation State Configuration](automation-dsc-compile.md)
- Para la referencia de cmdlets de PowerShell, consulte [cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- Para obtener información de precios, consulte [Precios de Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/)
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua mediante Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md)
- Para aprender a crear diferentes consultas de búsqueda y a revisar los registros de Automation State Configuration con los registros de Azure Monitor, consulte [Búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md).
- Para obtener más información sobre los registros de Azure Monitor y los orígenes de recopilación de datos, consulte [Introducción a la recopilación de datos de almacenamiento en los registros de Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md).
