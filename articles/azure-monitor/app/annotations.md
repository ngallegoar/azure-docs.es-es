---
title: Anotaciones de la versión de Application Insights | Microsoft Docs
description: Agregue marcadores de implementación o compilación a sus gráficos del Explorador de métricas en Application Insights.
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: e0e2a106b276110e13b3c68889e4d1d349ba73a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666520"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anotaciones sobre gráficos de métricas en Application Insights

En las anotaciones sobre gráficos del [Explorador de métricas](../../azure-monitor/app/metrics-explorer.md) se muestra donde ha implementado una nueva compilación u otros eventos importantes. Las anotaciones permiten ver fácilmente si los cambios tuvieron algún efecto en el rendimiento de la aplicación. Se pueden crear automáticamente con el sistema de compilación de [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/tasks/). También puede crear anotaciones para marcar los eventos que prefiera si los crea desde PowerShell.

> [!NOTE]
> En este artículo se refleja la **experiencia de métricas clásicas** en desuso. Las anotaciones solo están disponibles actualmente en la experiencia clásica y en los **[libros](../../azure-monitor/app/usage-workbooks.md)** . Para más información acerca de la experiencia de métricas actual, consulte [Características avanzadas del Explorador de métricas de Azure](../../azure-monitor/platform/metrics-charts.md).

![Ejemplo de anotaciones](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-pipelines-build"></a>Anotaciones de la versión con una compilación de Azure Pipelines

Las anotaciones de la versión son una característica del servicio Azure Pipelines basado en la nube de Azure DevOps.

### <a name="install-the-annotations-extension-one-time"></a>Instalación de la extensión Anotaciones (una vez)
Para poder crear anotaciones de la versión, debe instalar una de las muchas extensiones de Azure DevOps disponibles en Visual Studio Marketplace.

1. Inicie sesión en su proyecto de [Azure DevOps](https://azure.microsoft.com/services/devops/).
   
1. En la página de [extensión de anotaciones de la versión](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) de Visual Studio Marketplace, seleccione su organización de Azure DevOps y, a continuación, seleccione **Instalar** para agregar la extensión a la organización de Azure DevOps.
   
   ![Seleccione una organización de Azure DevOps y, a continuación, seleccione Instalar.](./media/annotations/1-install.png)
   
Solo deberá instalar la extensión una vez para su organización de Azure DevOps. Las anotaciones de la versión se pueden configurar ahora para cualquier proyecto de su organización.

### <a name="configure-release-annotations"></a>Configurar anotaciones de versión

Cree una clave de API independiente para cada una de las plantillas de versión de Azure Pipelines.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra el recurso de Application Insights que supervisa su aplicación. Si no tiene uno, [cree un nuevo recurso de Application Insights](../../azure-monitor/app/app-insights-overview.md).
   
1. Abra la pestaña **Acceso de API** y copie el **Id. de Application Insights**.
   
   ![En Acceso de API, copie el identificador de la aplicación.](./media/annotations/2-app-id.png)

1. En una ventana del explorador independiente, abra o cree la plantilla de versión que administra sus implementaciones de Azure Pipelines.
   
1. Seleccione **Agregar tarea** y, a continuación, seleccione la tarea **Anotación de versión de Application Insights** en el menú.
   
   ![Seleccione Agregar tarea y seleccione Anotación de versión de Application Insights.](./media/annotations/3-add-task.png)

   > [!NOTE]
   > Actualmente, la tarea Anotación de versión solo admite agentes basados en Windows; no se ejecuta en Linux, macOS ni otros tipos de agentes.
   
1. En **Id. de aplicación**, pegue el identificador de Application Insights que copió de la pestaña **Acceso de API**.
   
   ![Pegue el identificador de Application Insights.](./media/annotations/4-paste-app-id.png)
   
1. De vuelta en la ventana **Acceso de API** de Application Insights, seleccione **Crear clave de API**. 
   
   ![En la pestaña Acceso de API, seleccione Crear clave de API.](./media/annotations/5-create-api-key.png)
   
1. En la ventana **Crear clave de API**, escriba una descripción, seleccione **Escribir anotaciones** y, a continuación, seleccione **Generar clave**. Copie la clave nueva.
   
   ![En la ventana Crear clave de API, escriba una descripción, seleccione Escribir anotaciones y, a continuación, seleccione Generar clave.](./media/annotations/6-create-api-key.png)
   
1. En la ventana de la plantilla de versión, en la pestaña **Variables**, seleccione **Agregar** para crear una definición de variable para la nueva clave de API.

1. En **Nombre**, escriba `ApiKey` y, en **Valor**, pegue la clave de API que copió en la pestaña **Acceso de API**.
   
   ![En la pestaña Variables de Azure DevOps, seleccione Agregar, asigne a la variable el nombre ApiKey y pegue la clave de API en Valor.](./media/annotations/7-paste-api-key.png)
   
1. Seleccione **Guardar** en la ventana de la plantilla de versión principal para guardar la plantilla.

## <a name="view-annotations"></a>Ver anotaciones
Ahora, cuando utilice la plantilla de versión para implementar una nueva versión, se enviará una anotación a Application Insights. Las anotaciones se mostrarán en los gráficos del **Explorador de métricas**.

Seleccione cualquier marcador de anotación (flecha de color gris claro) para abrir los detalles de la versión, incluidos el solicitante, la rama de control de código fuente, la canalización de la versión y el entorno.

![Seleccione un marcador de anotación de la versión.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Crear anotaciones personalizadas desde PowerShell
Puede usar el script de PowerShell [CreateReleaseAnnotation](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1) de GitHub para crear anotaciones desde cualquier proceso que desee, sin usar Azure DevOps. 

1. Realice una copia local de [CreateReleaseAnnotation.ps1](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).
   
1. Use los pasos del procedimiento anterior para obtener el identificador de Application Insights y crear una clave de API desde la pestaña **Acceso de API** de Application Insights.
   
1. Llame al script de PowerShell con el código siguiente, reemplazando los marcadores de posición entre corchetes angulares por sus valores. Los valores de `-releaseProperties` son opcionales. 
   
   ```powershell
   
        .\CreateReleaseAnnotation.ps1 `
         -applicationId "<applicationId>" `
         -apiKey "<apiKey>" `
         -releaseName "<releaseName>" `
         -releaseProperties @{
             "ReleaseDescription"="<a description>";
             "TriggerBy"="<Your name>" }
   ```

El script se puede modificar (por ejemplo, para crear anotaciones para el pasado).

## <a name="next-steps"></a>Pasos siguientes

* [Crear elementos de trabajo](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automation con PowerShell](../../azure-monitor/app/powershell.md)
