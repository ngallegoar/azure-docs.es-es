---
title: Identificadores de contexto de usuario para realizar un seguimiento de la actividad - Azure Application Insights
description: Realice un seguimiento de cómo los usuarios navegan por el servicio mediante la asignación a cada uno de ellos de una cadena de identificador única y persistente en Application Insights.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/03/2019
ms.reviewer: abgreg;mbullwin
ms.openlocfilehash: ecdcc8a84cdccb05ec514003d63f808583d719c9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "83797682"
---
# <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Enviar identificadores contextuales de usuario para habilitar las experiencias de uso en Azure Application Insights

## <a name="tracking-users"></a>Seguimiento de usuarios

Application Insights le permite supervisar y realizar un seguimiento de los usuarios a través de un conjunto de herramientas de uso del producto:

- [Usuarios, sesiones, eventos](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
- [Embudos](https://docs.microsoft.com/azure/application-insights/usage-funnels)
- Cohortes de [retención](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
- [Libros](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview)

Para realizar un seguimiento de lo que hace un usuario a lo largo del tiempo, Application Insights necesita un identificador para cada usuario o sesión. Incluya los siguientes identificadores en cada evento personalizado o vista de página.

- Usuarios, embudos, retención y cohortes: incluya el identificador de usuario.
- Sesiones: incluya el identificador de sesión.

> [!NOTE]
> Este es un artículo avanzado que describe los pasos manuales para el seguimiento de la actividad del usuario con Application Insights. Con muchas aplicaciones web **es posible que estos pasos no sean necesarios**, ya que los SDK del servidor predeterminados junto con el [SDK de JavaScript del lado cliente o explorador](../../azure-monitor/app/website-monitoring.md ) suelen ser suficientes para realizar un seguimiento automático de la actividad del usuario. Si no ha configurado la [supervisión de cliente](../../azure-monitor/app/website-monitoring.md ) además del SDK del lado servidor, hágalo primero y pruébelo para ver si las herramientas de análisis de comportamiento de usuario se ejecutan según lo esperado.

## <a name="choosing-user-ids"></a>Elección de los identificadores de usuario

Los identificadores de usuario se deben conservar entre las sesiones de usuario para realizar un seguimiento de cómo se comportan los usuarios con el tiempo. Hay varios enfoques para conservar el identificador.

- Una definición de un usuario que ya tiene en su servicio.
- Si el servicio tiene acceso a un explorador, puede pasar a este una cookie con un identificador en ella. El identificador se conservará siempre que la cookie permanezca en el explorador del usuario.
- Si es necesario, puede usar un nuevo identificador en cada sesión, pero los resultados sobre los usuarios serán limitados. Por ejemplo, no podrá ver cómo cambia el comportamiento de un usuario con el tiempo.

El identificador debe ser un GUID u otra cadena lo suficientemente compleja como para identificar de forma única a cada usuario. Por ejemplo, podría ser un número aleatorio largo.

Si el identificador contiene información de identificación personal sobre el usuario, no es un valor adecuado para enviarlo a Application Insights como identificador de usuario. Puede enviar un identificador de ese tipo como [identificador de usuario autenticado](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), pero no cumplirá el requisito de identificador de usuario para los escenarios de uso.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>Aplicaciones ASP.NET: Establecimiento del contexto de usuario en un inicializador de telemetría

Cree un inicializador de telemetría, tal como se describe [aquí](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#addmodify-properties-itelemetryinitializer) en detalle. Pase el id. de sesión a través de la telemetría de solicitudes y establezca Context.User.Id y Context.Session.Id.

Este ejemplo establece el identificador de usuario en un identificador que expira después de la sesión. Si es posible, use un identificador de usuario que se conserve entre sesiones.

### <a name="telemetry-initializer"></a>Inicializador de telemetría

```csharp
using System;
using System.Web;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace MvcWebRole.Telemetry
{
  /*
   * Custom TelemetryInitializer that sets the user ID.
   *
   */
  public class MyTelemetryInitializer : ITelemetryInitializer
  {
    public void Initialize(ITelemetry telemetry)
    {
        var ctx = HttpContext.Current;

        // If telemetry initializer is called as part of request execution and not from some async thread
        if (ctx != null)
        {
            var requestTelemetry = ctx.GetRequestTelemetry();
 
            // Set the user and session ids from requestTelemetry.Context.User.Id, which is populated in Application_PostAcquireRequestState in Global.asax.cs.
            if (requestTelemetry != null && !string.IsNullOrEmpty(requestTelemetry.Context.User.Id) &&
                (string.IsNullOrEmpty(telemetry.Context.User.Id) || string.IsNullOrEmpty(telemetry.Context.Session.Id)))
            {
                // Set the user id on the Application Insights telemetry item.
                telemetry.Context.User.Id = requestTelemetry.Context.User.Id;
 
                // Set the session id on the Application Insights telemetry item.
                telemetry.Context.Session.Id = requestTelemetry.Context.User.Id;
            }
        }
    }
  }
}
```

### <a name="globalasaxcs"></a>Global.asax.cs

```csharp
using System.Web;
using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

namespace MvcWebRole.Telemetry
{
    public class MvcApplication : HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            GlobalConfiguration.Configure(WebApiConfig.Register);
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
 
        protected void Application_PostAcquireRequestState()
        {
            var requestTelemetry = Context.GetRequestTelemetry();
 
            if (HttpContext.Current.Session != null && requestTelemetry != null && string.IsNullOrEmpty(requestTelemetry.Context.User.Id))
            {
                requestTelemetry.Context.User.Id = Session.SessionID;
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para habilitar las experiencias de uso, empiece por enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) o [vistas de páginas](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Si ya ha enviado eventos personalizados o vistas de página, explore las herramientas de uso para obtener información sobre cómo los usuarios utilizan el servicio.
    - [Información general del uso](usage-overview.md)
    - [Usuarios, sesiones y eventos](usage-segmentation.md)
    - [Embudos](usage-funnels.md)
    - [Retención](usage-retention.md)
    - [Libros](../../azure-monitor/platform/workbooks-overview.md)
