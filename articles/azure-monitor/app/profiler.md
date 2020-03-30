---
title: Generación de perfiles de aplicaciones activas de Azure App Service con Application Insights | Microsoft Docs
description: Cree perfiles de aplicaciones activas de Azure App Service con Application Insights Profiler.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: ba9a2aca73dbdb8de298b68670fd6ab16f810a4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79234640"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Generación de perfiles de aplicaciones activas de Azure App Service con Application Insights

Puede ejecutar Profiler en aplicaciones ASP.NET y ASP.NET Core que se ejecutan en Azure App Service con el nivel de servicio Básico o superior. Actualmente, la habilitación de Profiler en Linux solo es posible a través de [este método](profiler-aspnetcore-linux.md).

## <a name="enable-profiler-for-your-app"></a><a id="installation"></a> Habilitación de Profiler en la aplicación
Para habilitar Profiler en una aplicación, siga las instrucciones que se indican a continuación. Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Profiler en otras plataformas compatibles:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler viene preinstalado en el entorno de ejecución de App Services. Los pasos siguientes le mostrarán cómo habilitarlo para App Service. Siga estos pasos incluso si ha incluido el SDK de App Insights en la aplicación en el momento de la compilación.

1. Habilite a la opción "Siempre disponible" para el servicio de aplicación. Puede actualizar el parámetro en la página de configuración de App Service, en Configuración general.
1. Vaya al panel **App Services** en Azure Portal.
1. Vaya al panel **Configuración > Application Insights**.

   ![Habilitación de App Insights en el portal de App Services](./media/profiler/AppInsights-AppServices.png)

1. Siga las instrucciones que aparecen en el panel para crear un nuevo recurso o seleccione un recurso de App Insights existente para supervisar la aplicación. También, asegúrese de que Profiler esté **activado**. Si el recurso de Application Insights está en otra suscripción de App Service, no puede usar esta página para configurar Application­ Insights. Sin embargo, puede hacerlo manualmente creando la configuración de la aplicación necesaria por usted mismo. [La siguiente sección contiene instrucciones para habilitar Profiler de forma manual.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

1. Profiler está habilitado con una configuración de aplicación de App Services.

    ![Configuración de aplicación de Profiler][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Habilitación de Profiler manualmente o con Azure Resource Manager
Application Insights Profiler puede habilitarse mediante la creación de la configuración de la aplicación para Azure App Service. La página con las opciones mostradas anteriormente crea esta configuración de aplicación de forma automática. Sin embargo, es posible automatizar la creación de estas opciones mediante una plantilla u otros medios. Esta configuración también funciona si el recurso de Application Insights está en otra suscripción de Azure App Service.
Estos son los valores necesarios para habilitar el generador de perfiles:

|Configuración de aplicación    | Value    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | iKey para el recurso de Application Insights    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


Puede establecer estos valores mediante las [plantillas de Azure Resource Manager](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest).

### <a name="enabling-profiler-for-other-clouds-manually"></a>Habilitación de Profiler para otras nubes manualmente

Si quiere habilitar Profiler para otras nubes, puede usar la siguiente configuración de aplicación.

|Configuración de aplicación    | Valores de US Government| Nube de China |   
|---------------|---------------------|-------------|
|ApplicationInsightsProfilerEndpoint         | https://agent.serviceprofiler.azure.us    | https://profiler.applicationinsights.azure.cn |
|ApplicationInsightsEndpoint | https://dc.applicationinsights.us | https://dc.applicationinsights.azure.cn |

## <a name="disable-profiler"></a>Deshabilitación de Profiler

Para detener o reiniciar el generador de perfiles en una determina instancia de la aplicación, en **Trabajos web**, vaya al recurso de la aplicación. Para eliminar Profiler, vaya a **Extensiones**.

![Deshabilitación de Profiler para un trabajo web][disable-profiler-webjob]

Se recomienda tener habilitado el generador de perfiles en todas las aplicaciones para detectar cualquier problema de rendimiento lo antes posible.

Los archivos de Profiler pueden eliminarse cuando se usa WebDeploy para implementar los cambios en la aplicación web. Puede evitar la eliminación si se excluye la carpeta App_Data de la eliminación durante la implementación. 


## <a name="next-steps"></a>Pasos siguientes

* [Trabajo con Application Insights en Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
