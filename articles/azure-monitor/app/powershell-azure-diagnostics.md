---
title: Uso de PowerShell para configurar Application Insights en Azure | Microsoft Docs
description: Configuración automática de Azure Diagnostics para la canalización de datos a Application Insights
ms.topic: conceptual
ms.date: 08/06/2019
ms.openlocfilehash: da1796c8af5b9463d8223615f4b0629ba65eb3e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669810"
---
# <a name="using-powershell-to-set-up-application-insights-for-azure-cloud-services"></a>Uso de PowerShell para configurar Application Insights para Azure Cloud Services

[Microsoft Azure](https://azure.com) puede [configurarse para que envíe diagnósticos de Azure](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md) a [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Los diagnósticos están relacionados con Azure Cloud Services y Azure Virtual Machines. Complementan la telemetría que se envía desde la aplicación mediante el SDK de Application Insights. Como parte de la automatización del proceso de creación de nuevos recursos en Azure, puede configurar diagnósticos mediante PowerShell.

## <a name="azure-template"></a>Plantilla de Azure
Si la aplicación web está en Azure y crea los recursos mediante una plantilla de Azure Resource Manager, puede configurar Application Insights agregando lo siguiente al nodo de recursos:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` : nombre para el recurso de Application Insights
* `myWebAppName` : identificador de la aplicación web

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Habilitar la extensión de diagnósticos como parte de la implementación de un servicio en la nube
El cmdlet `New-AzureDeployment` tiene un parámetro `ExtensionConfiguration`, que toma una matriz de configuraciones de diagnósticos. Estas pueden crearse mediante el cmdlet `New-AzureServiceDiagnosticsExtensionConfig` . Por ejemplo:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Habilitar la extensión de diagnóstico en un servicio en la nube existente
En un servicio existente, use `Set-AzureServiceDiagnosticsExtension`.

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Obtener la configuración actual de la extensión de diagnósticos
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Eliminar la extensión de diagnósticos
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Si ha habilitado la extensión de diagnósticos mediante `Set-AzureServiceDiagnosticsExtension` o `New-AzureServiceDiagnosticsExtensionConfig` sin el parámetro Role, puede quitar la extensión mediante `Remove-AzureServiceDiagnosticsExtension` sin el parámetro Role. Si ha usado el parámetro Role al habilitar la extensión, debe utilizarlo también al quitar la extensión.

Para quitar la extensión de diagnóstico de cada rol individual:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Consulte también
* [Supervisión de aplicaciones de Azure Cloud Service con Application Insights](../../azure-monitor/app/cloudservices.md)
* [Envío de Azure Diagnostics a Application Insights](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Automatización de la configuración de alertas](powershell-alerts.md)

