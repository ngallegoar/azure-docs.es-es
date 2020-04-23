---
title: Supervisión del uso y el rendimiento en las aplicaciones de escritorio de Windows
description: Analice el uso y el rendimiento de la aplicación de escritorio de Windows con Application Insights.
ms.topic: conceptual
ms.date: 10/29/2019
ms.openlocfilehash: eb9e0fc480098478a3a68265ac85e0d5450e27fe
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537396"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Supervisión del uso y el rendimiento en las aplicaciones de escritorio de Windows clásicas

Las aplicaciones hospedadas en el entorno local, en Azure y en otras nubes pueden aprovechar las ventajas de Application Insights. La única limitación es la necesidad de [permitir la comunicación](../../azure-monitor/app/ip-addresses.md) al servicio de Application Insights. Para supervisar las aplicaciones de la Plataforma universal de Windows (UWP), se recomienda [Visual Studio App Center](../../azure-monitor/learn/mobile-center-quickstart.md).

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>Para enviar datos de telemetría a Application Insights desde una aplicación Windows clásica
1. En [Azure Portal](https://portal.azure.com), [cree un recurso de Application Insights](../../azure-monitor/app/create-new-resource.md ). Para el tipo de aplicación, elija la aplicación ASP.NET.
2. Realice una copia de la clave de instrumentación. Busque la clave en la lista desplegable Essentials del recurso que acaba de crear. 
3. En Visual Studio, edite los paquetes NuGet de su proyecto de aplicación y agregue Microsoft.ApplicationInsights.WindowsServer. (O elija Microsoft.ApplicationInsights si únicamente le interesa la API sola, sin los módulos de recopilación de telemetría estándar).
4. Establezca la clave de instrumentación en el código:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *su clave* `";`
   
    o en ApplicationInsights.config (si tiene instalado uno de los paquetes de telemetría estándar):
   
    `<InstrumentationKey>`*su clave*`</InstrumentationKey>` 
   
    Si usa ApplicationInsights.config, asegúrese de que sus propiedades en el Explorador de soluciones se establecen en **Acción de compilación = Contenido, Copiar en el directorio de salida = Copiar**.
5. [Use la API](../../azure-monitor/app/api-custom-events-metrics.md) para enviar telemetría.
6. Ejecute la aplicación y vea la telemetría en el recurso que creó en Azure Portal.

## <a name="example-code"></a><a name="telemetry"></a>Ejemplo de código
```csharp
using Microsoft.ApplicationInsights;

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
        {
            e.Cancel = true;

            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="override-storage-of-computer-name"></a>Invalidación del almacenamiento del nombre de equipo

De forma predeterminada, este SDK recopilará y almacenará el nombre de equipo del sistema que emite la telemetría. Para invalidar la recopilación, debe usar un inicializador de telemetría:

**Escriba un elemento TelemetryInitializer personalizado como el siguiente.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here, you can pass an empty string if needed.
                  telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```
Cree una instancia del inicializador en el método `Main()` de `Program.cs` que aparece a continuación al establecer la clave de instrumentación:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

   static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
        }
```

## <a name="next-steps"></a>Pasos siguientes
* [Creación de un panel](../../azure-monitor/app/overview-dashboard.md)
* [Búsqueda de diagnóstico](../../azure-monitor/app/diagnostic-search.md)
* [Exploración de métricas](../../azure-monitor/platform/metrics-charts.md)
* [Escribir consultas de Analytics](../../azure-monitor/app/analytics.md)

