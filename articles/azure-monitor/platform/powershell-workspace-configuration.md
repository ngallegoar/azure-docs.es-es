---
title: Creación y configuración de Log Analytics con PowerShell
description: Las áreas de trabajo de Log Analytics en Azure Monitor almacenan datos de servidores en su infraestructura local o en la nube. Puede recopilar datos de equipo del almacenamiento de Azure cuando son generados por Diagnósticos de Azure.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/19/2019
ms.openlocfilehash: 2584cedceab1386cbab9c72bb4b510eebe2122bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054706"
---
# <a name="manage-log-analytics-workspace-in-azure-monitor-using-powershell"></a>Administre el área de trabajo de Log Analytics en Azure Monitor con PowerShell

Puede usar los [cmdlets de PowerShell de Log Analytics](https://docs.microsoft.com/powershell/module/az.operationalinsights/) para realizar varias funciones en un área de trabajo de Log Analytics en Azure Monitor desde una línea de comandos o como parte de un script.  A continuación se indican algunos ejemplos de las tareas que puede realizar con PowerShell:

* Crear un área de trabajo
* Agregar o quitar una solución
* Importar y exportar búsquedas guardadas
* Crear un grupo de equipos
* Habilitar la recopilación de registros de IIS en equipos con el agente de Windows instalado
* Recopilar contadores de rendimiento en equipos Linux y Windows
* Recopilar eventos de Syslog en equipos Linux
* Recopilar eventos de registros de eventos de Windows
* Recopilar registros de eventos personalizados
* Adición del agente de Log Analytics a una máquina virtual de Azure
* Configurar Log Analytics para indizar los datos recopilados mediante Diagnósticos de Azure

Este artículo proporciona dos ejemplos de código que muestran algunas de las funciones que puede realizar desde PowerShell.  Puede consultar la [referencia de cmdlets de PowerShell de Log Analytics](https://docs.microsoft.com/powershell/module/az.operationalinsights/) para otras funciones.

> [!NOTE]
> Log Analytics se llamaba anteriormente Operational Insights, razón por la cual se utiliza este nombre en los cmdlets.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prerrequisitos
Estos ejemplos funcionan con la versión 1.0.0 o posterior del módulo Az.OperationalInsights.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Creación y configuración de un área de trabajo de Log Analytics
El siguiente ejemplo de script muestra cómo:

1. Crear un área de trabajo
2. Enumerar las soluciones disponibles
3. Agregar soluciones al área de trabajo
4. Importar búsquedas guardadas
5. Exportar búsquedas guardadas
6. Crear un grupo de equipos
7. Habilitar la recopilación de registros de IIS en equipos con el agente de Windows instalado
8. Recopilar contadores de rendimiento de discos lógicos de equipos Linux (% de inodos usados; megabytes libres; % de espacio usado; transferencias de disco/seg.; lecturas de disco/seg.; escrituras de disco/seg.)
9. Recopilar eventos de Syslog de equipos Linux
10. Recopilar eventos de error y advertencia del registro de eventos de aplicación de los equipos de Windows
11. Recopilar contadores de rendimiento de MB disponibles de equipos Windows
12. Recopilar registros personalizados

```powershell

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique across all Azure subscriptions - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Perf | where ObjectName == "LogicalDisk" and CounterName == "Current Disk Queue Length" and InstanceName == "C:"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1",
    "description": "Example custom log datasource",
    "inputs": [
        {
            "location": {
            "fileSystemLocations": {
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                "linuxFileTypeLogPaths": [ "/var/logs" ]
                }
            },
        "recordDelimiter": {
            "regexDelimiter": {
                "pattern": "\\n",
                "matchIndex": 0,
                "matchIndexSpecified": true,
                "numberedGroup": null
                }
            }
        }
    ],
    "extractions": [
        {
            "extractionName": "TimeGenerated",
            "extractionType": "DateTime",
            "extractionProperties": {
                "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                    }
                }
            }
        ]
    }
"@

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json

# Create Computer Group based on a query
New-AzOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzOperationalInsightsLinuxPerformanceCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernel syslog collection"
Enable-AzOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs

New-AzOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

> [!NOTE]
> El formato del parámetro **CustomLogRawJson** que define la configuración de un registro personalizado puede ser complejo. Utilice [Get-AzOperationalInsightsDataSource](https://docs.microsoft.com/powershell/module/az.operationalinsights/get-azoperationalinsightsdatasource?view=azps-3.2.0) para recuperar la configuración de un registro personalizado existente. La propiedad **Properties** es la configuración necesaria para el parámetro **CustomLogRawJson**.

En el ejemplo anterior, regexDelimiter se definió como "\\n" para la nueva línea. El delimitador de registro también puede ser una marca de tiempo.  Estos son los formatos admitidos:

| Formato | El formato JSON RegEx usa dos \\ por cada \ en una expresión regular estándar, por lo que si se prueba en una aplicación RegEx, \\ se reduce a \ | | |
| --- | --- | --- | --- |
| `YYYY-MM-DD HH:MM:SS` | `((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |
| `M/D/YYYY HH:MM:SS AM/PM` | `(([0-1]\\d)|[0-9])/(([0-3]\\d)|(\\d))/((\\d{2})|(\\d{4}))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]\\s(AM|PM|am|pm)` | | |
| `dd/MMM/yyyy HH:MM:SS` | `(([0-2][1-9]|[3][0-1])\\/(Jan|Feb|Mar|May|Apr|Jul|Jun|Aug|Oct|Sep|Nov|Dec|jan|feb|mar|may|apr|jul|jun|aug|oct|sep|nov|dec)\\/((19|20)[0-9][0-9]))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9])` |
| `MMM dd yyyy HH:MM:SS` | `(((?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Sept|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)).*?((?:(?:[0-2]?\\d{1})|(?:[3][01]{1})))(?![\\d]).*?((?:(?:[1]{1}\\d{1}\\d{1}\\d{1})|(?:[2]{1}\\d{3})))(?![\\d]).*?((?:(?:[0-1][0-9])|(?:[2][0-3])|(?:[0-9])):(?:[0-5][0-9])(?::[0-5][0-9])?(?:\\s?(?:am|AM|pm|PM))?))` | | |
| `yyMMdd HH:mm:ss` | `([0-9]{2}([0][1-9]|[1][0-2])([0-2][0-9]|[3][0-1])\\s\\s?([0-1]?[0-9]|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `ddMMyy HH:mm:ss` | `(([0-2][0-9]|[3][0-1])([0][1-9]|[1][0-2])[0-9]{2}\\s\\s?([0-1]?[0-9]|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s\\s?([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0-1]?[0-9]|[2][0-3]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM  d HH:mm:ss` <br> dos espacios después de MMM | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s\\s([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\s([0]?[1-9]|[1-2][0-9]|[3][0-1])\\s([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `dd/MMM/yyyy:HH:mm:ss +zzzz` <br> donde + es + o un - <br> donde zzzz es la compensación de tiempo | `(([0-2][1-9]|[3][0-1])\\/(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\\/((19|20)[0-9][0-9]):([0][0-9]|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\s[\\+|\\-][0-9]{4})` | | |
| `yyyy-MM-ddTHH:mm:ss` <br> La T es una letra T literal | `((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))T((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |

## <a name="configuring-log-analytics-to-send-azure-diagnostics"></a>Configuración de Log Analytics para enviar diagnósticos de Azure
Para la supervisión de recursos de Azure sin agente, los recursos necesitan tener Diagnósticos de Azure habilitado y configurado para escribir en un área de trabajo de Log Analytics. Este método envía los datos directamente al área de trabajo y no requiere que los datos se escriban en una cuenta de almacenamiento. Los recursos admitidos son los siguientes:

| Tipo de recurso | Registros | Métricas |
| --- | --- | --- |
| Puertas de enlace de aplicaciones    | Sí | Sí |
| Cuentas de Automation     | Sí | |
| Cuentas de Batch          | Sí | Sí |
| Data Lake Analytics     | Sí | |
| Data Lake Store         | Sí | |
| Elastic SQL Pool        |     | Sí |
| Espacio de nombres del Centro de eventos     |     | Sí |
| IoT Hubs                |     | Sí |
| Key Vault               | Sí | |
| Equilibradores de carga          | Sí | |
| Logic Apps              | Sí | Sí |
| Grupos de seguridad de red | Sí | |
| Azure Cache for Redis             |     | Sí |
| Servicios de búsqueda         | Sí | Sí |
| Espacio de nombres de Service Bus   |     | Sí |
| SQL (v12)               |     | Sí |
| Sitios web               |     | Sí |
| Granjas de servidores web        |     | Sí |

Para obtener información detallada sobre las métricas disponibles, consulte las [métricas admitidas con Azure Monitor](../../azure-monitor/platform/metrics-supported.md).

Para obtener información detallada sobre los registros disponibles, consulte [Servicios admitidos y esquema de los registros de recursos](../../azure-monitor/platform/diagnostic-logs-schema.md).

```powershell
$workspaceId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

También puede usar el cmdlet anterior para recopilar registros de recursos que se encuentran en distintas suscripciones. El cmdlet funciona en suscripciones distintas porque se proporciona tanto el identificador del recurso que crea los registros como del área de trabajo a la que se envían los registros.


## <a name="configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage"></a>Configuración del área de trabajo de Log Analytics para recopilar los diagnósticos de Azure desde el almacenamiento
Para recopilar datos de registro desde una instancia en ejecución de un servicio en la nube clásico o un clúster de Service Fabric, tiene que escribir primero los datos en Azure Storage. El área de trabajo de Log Analytics se puede configurar para recopilar los registros de la cuenta de almacenamiento. Los recursos admitidos son los siguientes:

* Servicios en la nube clásicos (roles web y de trabajo)
* Clústeres de Service Fabric

El ejemplo siguiente muestra cómo:

1. Enumerar las cuentas de almacenamiento existentes y las ubicaciones desde las que el área de trabajo indexará los datos
2. Crear una configuración para leer desde una cuenta de almacenamiento
3. Actualizar la configuración recién creada para indizar datos desde ubicaciones adicionales
4. Eliminar la configuración recién creada

```powershell
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable"
$workspace = (Get-AzOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want the workspace to index
$storageId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight"

```

También puede usar el script anterior para recopilar registros de cuentas de almacenamiento que se encuentran en distintas suscripciones. El script funciona en suscripciones distintas porque se proporciona tanto el identificador del recurso de cuenta de almacenamiento como la clave de acceso correspondiente. Cuando se cambia la clave de acceso, hay que actualizar los datos del almacenamiento para que tenga la nueva clave.


## <a name="next-steps"></a>Pasos siguientes
* [Revise los cmdlets de PowerShell de Log Analytics](https://docs.microsoft.com/powershell/module/az.operationalinsights/) para obtener información adicional sobre cómo usar PowerShell para la configuración de Log Analytics.

