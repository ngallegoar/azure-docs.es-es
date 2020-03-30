---
title: Análisis de eventos de Azure Service Fabric con Application Insights
description: Obtenga información sobre cómo visualizar y analizar eventos con Application Insights para la supervisión y el diagnóstico de clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 5c80dc9f350fec30469a9bce2ed836c276ef3a7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464756"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Análisis y visualización de eventos con Application Insights

Application Insights, que forma parte de Azure Monitor, es una plataforma extensible para la supervisión y el diagnóstico de aplicaciones. Incluye una sólida herramienta de análisis y consulta, visualizaciones y paneles personalizables y otras opciones, como las alertas automáticas. La integración de Application Insights con Service Fabric incluye herramientas para Visual Studio y Azure Portal, así como métricas específicas de Service Fabric, lo que proporciona una completa experiencia de registro de forma estándar. Aunque muchos registros se crean y recopilan automáticamente con Application Insights, es aconsejable agregar un mayor registro personalizado a las aplicaciones para mejorar el diagnóstico.

Este artículo ayuda a analizar las siguientes preguntas comunes:

* ¿Cómo puedo saber qué está sucediendo dentro de la aplicación y los servicios, y recopilar telemetría?
* ¿Cómo puedo solucionar los problemas de la aplicación, en particular de los servicios que se comunican entre sí?
* ¿Cómo se obtienen las métricas de rendimiento de los servicios, por ejemplo, el tiempo de carga de las páginas o las solicitudes http?

El propósito de este artículo es mostrar cómo obtener información y solucionar problemas desde Application Insights. Si desea información acerca de cómo instalar y configurar Application Insights con Service Fabric, consulte este [tutorial](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-application-insights"></a>Supervisión en Application Insights

Application Insights se integra directamente con Service Fabric. En la página información general, Application Insights proporciona información esencial acerca del servicio, como el tiempo de respuesta y el número de solicitudes procesadas. Al hacer clic en el botón "Buscar" de la parte superior, verá una lista de las solicitudes recientes en la aplicación. Además, podrá ver aquí las solicitudes con error y determinar a qué se deben estos errores.

![Información general de Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

En el panel derecho de la imagen anterior hay dos tipos principales de entradas en la lista: solicitudes y eventos. Las solicitudes son llamadas realizadas a API de la aplicación mediante solicitudes HTTP (en este caso), y los eventos son personalizados que actúan como telemetría que puede agregar en cualquier lugar del código. Puede explorar aún más la instrumentación de las aplicaciones en [API de Application Insights para eventos y métricas personalizados](../azure-monitor/app/api-custom-events-metrics.md). Al hacer clic en una solicitud se mostrarán más detalles, como se muestra en la siguiente imagen, entre los que se incluyen datos específicos de Service Fabric, que se recopilan en el paquete NuGet Service Fabric de Application Insights. Esta información es útil para solucionar problemas y conocer el estado de la aplicación; toda la información se puede buscar en Application Insights

![Detalles de las solicitudes de Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights tiene una vista designada para realizar consultas en todos los datos entrantes. Haga clic en "Explorador de métricas" en la parte superior de la página de información general para ir al portal de Application Insights. Aquí puede ejecutar consultas en los eventos personalizados que se mencionaron antes, solicitudes, excepciones, contadores de rendimiento y otras métricas mediante el lenguaje de consulta Kusto. El ejemplo siguiente muestra todas las solicitudes de la última hora.

![Detalles de las solicitudes de Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Para explorar aún más las funcionalidades del portal de Application Insights, diríjase a la [documentación del portal de Application Insights](../azure-monitor/app/app-insights-dashboards.md).

### <a name="configuring-application-insights-with-eventflow"></a>Configuración de Application Insights con EventFlow

Si va a usar EventFlow para agregar eventos, asegúrese de importar el paquete NuGet `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights`. Debe incluirse el código siguiente en la sección *outputs* de *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Asegúrese de realizar los cambios necesarios en los filtros, así como de incluir cualquier otra entrada (junto con sus respectivos paquetes NuGet).

## <a name="application-insights-sdk"></a>SDK de Application Insights

Se recomienda utilizar EventFlow y WAD como soluciones de agregación, porque permiten adoptar un enfoque más modular para el diagnóstico y la supervisión, es decir, si desea cambiar las salidas de EventFlow, no es necesario realizar ningún cambio en la instrumentación real, sino simplemente una modificación sencilla en el archivo de configuración. Sin embargo, si decide invertir en la utilización de Application Insights y no existe ninguna probabilidad de cambiar a otra plataforma, debería considerar la posibilidad de usar de un SDK nuevo de Application Insights para agregar eventos y enviarlos a Application Insights. Esto significa que ya no tendrá que configurar EventFlow para enviar datos a Application Insights, sino que tendrá que instalar el paquete NuGet Service Fabric de Application Insights. Puede encontrar información detallada sobre el paquete [aquí](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

En [Application Insights support for Microservices and Containers](https://azure.microsoft.com/blog/app-insights-microservices/) (Application Insights admite microservicios y contenedores), se muestran algunas de las nuevas características en las que se está trabajando (todavía en versión beta), lo que permite disponer de opciones de supervisión de serie enriquecidas con Application Insights. Entre ellas se incluyen la supervisión de dependencias (se usan para compilar una instancia de AppMap de todos los servicios y aplicaciones de un clúster y la comunicación entre ellos) y una correlación mejorada de los seguimientos procedentes de los servicios (facilita la detección de un problema en el flujo de trabajo de una aplicación o de un servicio).

Si desarrolla en .NET, existe la probabilidad de que vaya a usar algunos de los modelos de programación de Service Fabric y desea usar Application Insights como la plataforma para visualizar y analizar datos de eventos y de registro, es aconsejable seguir la ruta del SDK de Application Insights como flujo de trabajo de supervisión y diagnóstico. Lea [esto](../azure-monitor/app/asp-net-more.md) y [esto](../azure-monitor/app/asp-net-trace-logs.md) para empezar a usar Application Insights para recopilar y visualizar los registros.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Ir al recurso de Application Insights en Azure Portal

Tras configurar Application Insights como salida de eventos y registros, la información debería empezar a mostrarse en el recurso de Application Insights al cabo de pocos minutos. Vaya al recurso de Application Insights, lo que le llevará al panel de recursos de Application Insights. Haga clic en **Buscar** en la barra de tareas de Application Insights para ver los últimos seguimientos que ha recibido y poder filtrarlos.

El *Explorador de métricas* es una herramienta útil para crear paneles personalizados basados en métricas sobre las que las aplicaciones, los servicios y el clúster pueden informar. Vea [Exploración de métricas en Application Insights](../azure-monitor/app/metrics-explorer.md) para configurar algunos gráficos por su cuenta en función de los datos que recopile.

Al hacer clic en **Analytics**, se le remitirá al portal de Analytics en Application Insights, donde puede consultar eventos y seguimientos con un ámbito más amplio y más opciones. Lea más información en [Analytics en Application Insights](../azure-monitor/app/analytics.md).

## <a name="next-steps"></a>Pasos siguientes

* [Definición de alertas en Application Insights](../azure-monitor/app/alerts.md) para recibir notificaciones sobre los cambios de rendimiento o de uso
* [Detección inteligente en Application Insights](../azure-monitor/app/proactive-diagnostics.md) realiza un análisis proactivo de la telemetría que se envía a Application Insights para avisar de posibles problemas de rendimiento
