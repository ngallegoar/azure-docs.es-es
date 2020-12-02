---
title: Guía para la solución de problemas de Azure SignalR Service
description: Información sobre cómo solucionar problemas comunes
author: YanJin
ms.service: signalr
ms.topic: conceptual
ms.date: 11/06/2020
ms.author: yajin1
ms.openlocfilehash: cc17dcef7a554bee2715c79ba7d0c2356db2c6b3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185664"
---
# <a name="troubleshooting-guide-for-azure-signalr-service-common-issues"></a>Guía para la solución de problemas comunes de Azure SignalR Service

En esta guía se proporcionan instrucciones útiles para la solución de los problemas comunes que los clientes tuvieron y resolvieron en los últimos años.

## <a name="access-token-too-long"></a>El token de acceso es demasiado largo

### <a name="possible-errors"></a>Errores posibles:

* `ERR_CONNECTION_` en el lado cliente
* 414 - URI de solicitud demasiado largo
* 413 Carga demasiado grande
* El token de acceso no debe tener más de 4 K. 413 Entidad de solicitud demasiado larga

### <a name="root-cause"></a>Causa principal:

Para HTTP/2, la longitud máxima de un solo encabezado es **4 K**, por lo que, si usa el explorador para acceder al servicio de Azure, se producirá un error `ERR_CONNECTION_` para esta limitación.

En el caso de los clientes HTTP/1.1 o C#, la longitud máxima del URI es **12 k** y la longitud máxima del encabezado es **16 K**.

Con la versión del SDK **1.0.6** o posterior, `/negotiate` producirá `413 Payload Too Large` cuando el token de acceso generado sea mayor que **4 K**.

### <a name="solution"></a>Solución:

De forma predeterminada, las notificaciones de `context.User.Claims` se incluyen al generar el token de acceso JWT a **ASRS**(**A** zure **S** ignal **R** **S** ervice), para que las notificaciones se conserven y se puedan pasar de **ASRS** a `Hub` cuando el cliente se conecta a `Hub`.

En algunos casos, los objetos `context.User.Claims` se usan para almacenar gran cantidad de información del servidor de aplicaciones, que en su mayoría no se usa en `Hub`s, sino en otros componentes.

El token de acceso generado se pasa a través de la red, y para las conexiones WebSocket/SSE, los tokens de acceso se pasan a través de las cadenas de consulta. Por lo tanto, como procedimiento recomendado, solo se sugiere pasar a través de **ASRS** las notificaciones **necesarias** del cliente al servidor de aplicaciones cuando Hub lo necesite.

Hay un objeto `ClaimsProvider` para personalizar las notificaciones que se pasan a **ASRS** dentro del token de acceso.

Para ASP.NET Core:
```cs
services.AddSignalR()
        .AddAzureSignalR(options =>
            {
                // pick up necessary claims
                options.ClaimsProvider = context => context.User.Claims.Where(...);
            });
```

Para ASP.NET:
```cs
services.MapAzureSignalR(GetType().FullName, options =>
            {
                // pick up necessary claims
                options.ClaimsProvider = context.Authentication?.User.Claims.Where(...);
            });
```

## <a name="tls-12-required"></a>TLS 1.2 obligatorio

### <a name="possible-errors"></a>Errores posibles:

* Error [279](https://github.com/Azure/azure-signalr/issues/279) de ASP.NET que indica que no hay ningún servidor disponible.
* Error [324](https://github.com/Azure/azure-signalr/issues/324) de ASP.NET que indica que la conexión no está activa y que los datos no se pueden enviar al servicio.
* Error al realizar la solicitud HTTP a https://<API endpoint>. Este error puede deberse a que el certificado del servidor no está configurado correctamente con HTTP.SYS en el caso de HTTPS. La causa puede ser también una falta de coincidencia del enlace de seguridad entre el cliente y el servidor.

### <a name="root-cause"></a>Causa principal:

El servicio de Azure solo admite TLS 1.2 por cuestiones de seguridad. Con .NET Framework, es posible que TLS 1.2 no sea el protocolo predeterminado. Como resultado, las conexiones del servidor a ASRS no se pueden establecer correctamente.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

1. Si este error se puede reproducir localmente, desactive *Solo mi código*, genere todas las excepciones de CLR y depure el servidor de aplicaciones de forma local para ver qué excepción se produce.
    * Desactive *Solo mi código*.

        :::image type="content" source="./media/signalr-howto-troubleshoot-guide/uncheck-just-my-code.png" alt-text="Desactivar Solo mi código":::

    * Genere excepciones de CLR.

        :::image type="content" source="./media/signalr-howto-troubleshoot-guide/throw-clr-exceptions.png" alt-text="Generar excepciones de CLR":::

    * Vea las excepciones que se generan al depurar el código del servidor de aplicaciones:
        
        :::image type="content" source="./media/signalr-howto-troubleshoot-guide/tls-throws.png" alt-text="Excepción generada":::

2. En el caso de ASP.NET, también puede agregar el código siguiente a `Startup.cs` para habilitar el seguimiento detallado y ver los errores del registro.
```cs
app.MapAzureSignalR(this.GetType().FullName);
// Make sure this switch is called after MapAzureSignalR
GlobalHost.TraceManager.Switch.Level = SourceLevels.Information;
```

### <a name="solution"></a>Solución:

Agregue el siguiente código a su página de inicio:
```cs
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
```

## <a name="400-bad-request-returned-for-client-requests"></a>Error 400 Solicitud incorrecta devuelto para solicitudes de cliente

### <a name="root-cause"></a>Causa principal

Compruebe si la solicitud de cliente tiene varias cadenas de consulta `hub`. `hub` es un parámetro de consulta conservado, y el error 400 se generará si el servicio detecta más de un elemento `hub` en la consulta.

## <a name="401-unauthorized-returned-for-client-requests"></a>Se ha devuelto el error 401 No autorizado para las solicitudes de cliente.

### <a name="root-cause"></a>Causa principal

Actualmente, el valor predeterminado de la duración del token JWT es de 1 hora.

En el caso de ASP.NET Core SignalR, cuando se usa el tipo de transporte WebSocket, es correcto.

Para otro tipo de transporte de ASP.NET Core SignalR, SSE y sondeo prolongado, esto significa de forma predeterminada que la conexión puede persistir como máximo durante 1 hora.

En el caso de ASP.NET SignalR, el cliente envía una solicitud KeepAlive de `/ping` al servicio de vez en cuando, cuando se produce un error de `/ping`, el cliente **anula** la conexión y nunca se vuelve a conectar. Esto significa que, en el caso de ASP.NET SignalR, la duración del token predeterminada hace que la conexión dure **como máximo** 1 hora para todo el tipo de transporte.

### <a name="solution"></a>Solución

Por motivos de seguridad, no se recomienda ampliar TTL. Se recomienda agregar lógica de reconexión del cliente para reiniciar la conexión cuando se produce el error 401. Cuando el cliente reinicia la conexión, se negociará con el servidor de aplicaciones para obtener el token JWT de nuevo y obtener un token renovado.

Consulte [aquí](#restart_connection) cómo reiniciar las conexiones de cliente.

## <a name="404-returned-for-client-requests"></a>Error 404 devuelto para solicitudes de cliente

En el caso de una conexión persistente de SignalR, primero realizará un proceso `/negotiate` en Azure SignalR Service y, a continuación, establecerá la conexión real con Azure SignalR Service.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

* Consulte [Cómo ver las solicitudes salientes](#view_request) para obtener la solicitud del cliente al servicio.
* Compruebe la dirección URL de la solicitud cuando se produce el error 404. Si la dirección URL apunta a la aplicación web y es similar a `{your_web_app}/hubs/{hubName}`, compruebe si el cliente `SkipNegotiation` es `true`. Al usar Azure SignalR, el cliente recibe la dirección URL de redireccionamiento la primera vez que negocia con el servidor de aplicaciones. El cliente **NO** debe omitir la negociación al usar Azure SignalR.
* Se puede producir otro error 404 cuando la solicitud de conexión se controla durante más de **5** segundos después de llamar a `/negotiate`. Compruebe la marca de tiempo de la solicitud de cliente e infórmenos del problema si la solicitud al servicio tiene una respuesta lenta.

## <a name="404-returned-for-aspnet-signalrs-reconnect-request"></a>Error 404 devuelto para la solicitud de reconexión de ASP.NET SignalR

En el caso de ASP.NET SignalR, cuando la [conexión de cliente se interrumpe](#client_connection_drop), se vuelve a conectar con el mismo parámetro `connectionId` tres veces antes de detener la conexión. `/reconnect` puede ayudar si la conexión se interrumpe debido a problemas intermitentes de la red, ya que `/reconnect` puede restablecer la conexión persistente correctamente. En otras circunstancias, por ejemplo, si se interrumpe la conexión de cliente debido a la interrupción de la conexión del servidor enrutado, o SignalR Service tiene algunos errores internos, como el reinicio de la instancia, la conmutación por error o la implementación, la conexión ya no existe, entonces `/reconnect` devuelve `404`. Es el comportamiento esperado para `/reconnect` y, después de tres reintentos, la conexión se detiene. Se recomienda tener una lógica de [reinicio de conexión](#restart_connection) cuando se detiene la conexión.

## <a name="429-too-many-requests-returned-for-client-requests"></a>Error 429 Demasiadas solicitudes devueltas para solicitudes de cliente

El error 429 se devuelve si su recuento de conexiones **simultáneas** supera el límite.

En el caso de las instancias **Gratis**, el límite del número de conexiones **simultáneas** es 20 para instancias **Estándar** y el límite del número de conexiones **simultáneas** **por unidad** es 1000, lo que significa que 100 unidades permitirán 100 000 conexiones simultáneas.

Las conexiones incluyen conexiones de cliente y de servidor. Consulte [aquí](./signalr-concept-messages-and-connections.md#how-connections-are-counted) cómo se cuentan las conexiones.

## <a name="500-error-when-negotiate-azure-signalr-service-is-not-connected-yet-please-try-again-later"></a>Error 500 al negociar: Azure SignalR Service no está conectado todavía. Vuelva a intentarlo más tarde.

### <a name="root-cause"></a>Causa principal

Este error se genera cuando no hay ninguna conexión de servidor a la instancia conectada de Azure SignalR Service.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

Habilite el seguimiento del lado servidor para averiguar los detalles del error cuando el servidor intenta conectarse a Azure SignalR Service.

#### <a name="enable-server-side-logging-for-aspnet-core-signalr"></a>Habilitación del registro del lado servidor para ASP.NET Core SignalR

El registro del lado servidor para ASP.NET Core SignalR se integra con los [registros](/aspnet/core/fundamentals/logging/?tabs=aspnetcore2x&view=aspnetcore-2.1) basados en `ILogger` proporcionado en el marco de ASP.NET Core. Puede habilitar el registro del lado servidor mediante `ConfigureLogging`. A continuación se incluye un ejemplo de uso:
```cs
.ConfigureLogging((hostingContext, logging) =>
        {
            logging.AddConsole();
            logging.AddDebug();
        })
```
Las categorías de registrador de Azure SignalR Service siempre comienzan con `Microsoft.Azure.SignalR`. Para habilitar los registros detallados de Azure SignalR Service, configure los prefijos anteriores en el nivel `Debug` del archivo **appsettings.json** como se indica a continuación:
```JSON
{
    "Logging": {
        "LogLevel": {
            ...
            "Microsoft.Azure.SignalR": "Debug",
            ...
        }
    }
}
```

#### <a name="enable-server-side-traces-for-aspnet-signalr"></a>Habilitación del seguimiento del lado servidor para ASP.NET SignalR

Cuando use una versión del SDK posterior a la `1.0.0`, podrá habilitar los seguimientos al agregar el siguiente código a `web.config`: ([Detalles](https://github.com/Azure/azure-signalr/issues/452#issuecomment-478858102))
```xml
<system.diagnostics>
    <sources>
      <source name="Microsoft.Azure.SignalR" switchName="SignalRSwitch">
        <listeners>
          <add name="ASRS" />
        </listeners>
      </source>
    </sources>
    <!-- Sets the trace verbosity level -->
    <switches>
      <add name="SignalRSwitch" value="Information" />
    </switches>
    <!-- Specifies the trace writer for output -->
    <sharedListeners>
      <add name="ASRS" type="System.Diagnostics.TextWriterTraceListener" initializeData="asrs.log.txt" />
    </sharedListeners>
    <trace autoflush="true" />
  </system.diagnostics>
```

<a name="client_connection_drop"></a>

## <a name="client-connection-drops"></a>Interrupciones de la conexión de cliente

Cuando el cliente está conectado a Azure SignalR, la conexión persistente entre el cliente y Azure SignalR a veces puede interrumpirse por distintas razones. En esta sección se describen varias causas posibles de esta interrupción de la conexión y se proporcionan instrucciones sobre cómo identificar la causa principal.

### <a name="possible-errors-seen-from-the-client-side"></a>Posibles errores detectados en el lado cliente

* `The remote party closed the WebSocket connection without completing the close handshake`
* `Service timeout. 30.00ms elapsed without receiving a message from service.`
* `{"type":7,"error":"Connection closed with an error."}`
* `{"type":7,"error":"Internal server error."}`

### <a name="root-cause"></a>Causa principal:

Las conexiones de cliente pueden interrumpirse en varias circunstancias:
* Cuando `Hub` produce excepciones con la solicitud entrante.
* Cuando se interrumpe la conexión del servidor, a la que se dirige el cliente, vea la sección siguiente para obtener más información sobre las [interrupciones de las conexiones del servidor](#server_connection_drop).
* Cuando se produce un problema de conectividad de red entre el cliente y SignalR Service.
* Cuando SignalR Service tiene algunos errores internos, como el reinicio de la instancia, la conmutación por error, la implementación, etc.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

1. Abra el registro del lado servidor de la aplicación para ver si se ha producido alguna anomalía.
2. Compruebe el registro de eventos del servidor de aplicaciones para ver si se ha reiniciado el servidor de aplicaciones.
3. Cree un problema para nosotros en el que debe indicar el plazo de tiempo y envíenos el nombre del recurso por correo electrónico.


## <a name="client-connection-increases-constantly"></a>Aumento constante de la conexión de cliente

Puede deberse a un uso inadecuado de la conexión de cliente. Si alguien olvida detener o eliminar el cliente de SignalR, la conexión permanece abierta.

### <a name="possible-errors-seen-from-the-signalrs-metrics-that-is-in-monitoring-section-of-azure-portal-resource-menu"></a>Posibles errores detectados a partir de las métricas de SignalR que se encuentran en la sección de supervisión del menú de recursos de Azure Portal

Las conexiones de cliente aumentan constantemente durante mucho tiempo en las métricas de Azure SignalR.

:::image type="content" source="./media/signalr-howto-troubleshoot-guide/client-connection-increasing-constantly.jpg" alt-text="Aumento constante de la conexión de cliente":::

### <a name="root-cause"></a>Causa principal:

Nunca se llama a `DisposeAsync` de la conexión de cliente de SignalR, y la conexión se mantiene abierta.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

1. Compruebe si el cliente de SignalR nunca se **cierra**.

### <a name="solution"></a>Solución

Compruebe si se cierra la conexión. Llame a `HubConnection.DisposeAsync()` manualmente para detener la conexión después de usarla.

Por ejemplo:

```C#
var connection = new HubConnectionBuilder()
    .WithUrl(...)
    .Build();
try
{
    await connection.StartAsync();
    // Do your stuff
    await connection.StopAsync();
}
finally
{
    await connection.DisposeAsync();
}
```

### <a name="common-improper-client-connection-usage"></a>Uso inadecuado común de la conexión de cliente

#### <a name="azure-function-example"></a>Ejemplo de función de Azure 

Este problema suele producirse cuando alguien establece una conexión de cliente de SignalR en el método de función de Azure en lugar de convertirlo en un miembro estático de la clase Function. Podría esperar que solo se establezca una conexión de cliente, pero verá que el número de conexiones de cliente aumenta constantemente en las métricas que se encuentran en la sección de supervisión del menú de recursos de Azure Portal; todas estas conexiones solo se interrumpen después del reinicio de la función de Azure o de Azure SignalR Service. Esto se debe a que, para **cada** solicitud, la función de Azure crea **una** conexión de cliente; si no detiene la conexión de cliente en el método Function, el cliente mantiene las conexiones activas con Azure SignalR Service.

#### <a name="solution"></a>Solución

* Recuerde cerrar la conexión de cliente si usa los clientes de SignalR en la función de Azure o si usa el cliente de SignalR como singleton.
* En lugar de usar clientes de SignalR en la función de Azure, puede crear clientes de SignalR en cualquier lugar y usar [enlaces de Azure Functions para Azure SignalR Service](https://github.com/Azure/azure-functions-signalrservice-extension) para [negociar](https://github.com/Azure/azure-functions-signalrservice-extension/blob/dev/samples/simple-chat/csharp/FunctionApp/Functions.cs#L22) el cliente en Azure SignalR. Además, también puede emplear el enlace para [enviar mensajes](https://github.com/Azure/azure-functions-signalrservice-extension/blob/dev/samples/simple-chat/csharp/FunctionApp/Functions.cs#L40). Los ejemplos para negociar el cliente y enviar mensajes se pueden encontrar [aquí](https://github.com/Azure/azure-functions-signalrservice-extension/tree/dev/samples). Se puede encontrar más información [aquí](https://github.com/Azure/azure-functions-signalrservice-extension).
* Al usar clientes de SignalR en la función de Azure, puede haber una arquitectura mejor para su escenario. Compruebe si ha diseñado una arquitectura sin servidor adecuada. Puede consultar información sobre las [aplicaciones sin servidor en tiempo real con los enlaces de SignalR Service en Azure Functions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService).

<a name="server_connection_drop"></a>

## <a name="server-connection-drops"></a>Interrupción de la conexión del servidor

Cuando se inicia el servidor de aplicaciones, en segundo plano, el SDK de Azure empieza a iniciar conexiones del servidor con la instancia remota de Azure SignalR. Como se describe en el artículo sobre el [funcionamiento interno de Azure SignalR Service](https://github.com/Azure/azure-signalr/blob/dev/docs/internal.md), Azure SignalR enruta el tráfico de cliente entrante a estas conexiones de servidor. Una vez que se interrumpe una conexión de servidor, todas las conexiones de cliente que atiende se cerrarán también.

Como las conexiones entre el servidor de aplicaciones y SignalR Service son persistentes, pueden experimentar problemas de conectividad de red. En el SDK del servidor, tenemos la estrategia de **volver a conectar siempre** a las conexiones del servidor. Como procedimiento recomendado, también se recomienda a los usuarios que agreguen lógica de reconexión continua a los clientes con un tiempo de retraso aleatorio para evitar solicitudes masivas simultáneas al servidor.

Se publican nuevas versiones de Azure SignalR Service con regularidad y, a veces, se aplican revisiones o actualizaciones del sistema operativo de Azure o se produce una interrupción ocasional de los servicios dependientes. Estos escenarios pueden ocasionar una breve interrupción del servicio, pero, siempre que el lado del cliente tenga el mecanismo de desconexión/reconexión, el impacto será mínimo, como cualquier desconexión o reconexión causada por el lado cliente.

En esta sección se describen varias causas posibles de esta interrupción de la conexión del servidor y se proporcionan algunas instrucciones sobre cómo identificar la causa principal.

### <a name="possible-errors-seen-from-server-side"></a>Posibles errores detectados en el lado servidor:

* `[Error]Connection "..." to the service was dropped`
* `The remote party closed the WebSocket connection without completing the close handshake`
* `Service timeout. 30.00ms elapsed without receiving a message from service.`

### <a name="root-cause"></a>Causa principal:

**ASRS**(**A** zure **S** ignal **R** **S** ervice) cierra la conexión de servicio del servidor.

### <a name="troubleshooting-guide"></a>Guía de solución de problemas

1. Abra el registro del lado servidor de la aplicación para ver si se ha producido alguna anomalía.
2. Compruebe el registro de eventos del servidor de aplicaciones para ver si se ha reiniciado el servidor de aplicaciones.
3. Cree un problema para nosotros en el que debe indicar el plazo de tiempo y envíenos el nombre del recurso por correo electrónico.

## <a name="tips"></a>Sugerencias

<a name="view_request"></a>

* ¿Cómo se puede ver la solicitud saliente del cliente?
Considere ASP.NET Core como ejemplo (ASP.NET es similar):
    * Desde el explorador:

        Considere Chrome como ejemplo; puede usar **F12** para abrir la ventana de la consola y cambiar a la pestaña **Red**. Es posible que tenga que actualizar la página con **F5** para capturar la red desde el principio.

        :::image type="content" source="./media/signalr-howto-troubleshoot-guide/chrome-network.gif" alt-text="Red en la vista de Chrome":::

    * Desde el cliente de C#:

        Puede ver el tráfico web local con [Fiddler](https://www.telerik.com/fiddler). El tráfico de WebSocket es compatible a partir de Fiddler 4.5.

        :::image type="content" source="./media/signalr-howto-troubleshoot-guide/fiddler-view-network-inline.png" alt-text="Red en la vista de Fiddler" lightbox="./media/signalr-howto-troubleshoot-guide/fiddler-view-network.png":::

<a name="restart_connection"></a>

* ¿Cómo se reinicia la conexión de cliente?
    
    Estos son los [códigos de ejemplo](https://github.com/Azure/azure-signalr/tree/dev/samples) que contienen la lógica de reinicio de la conexión con la estrategia *ALWAYS RETRY*:

    * [Cliente de C# en ASP.NET Core](https://github.com/Azure/azure-signalr/tree/dev/samples/ChatSample/ChatSample.CSharpClient/Program.cs#L64)

    * [Cliente de JavaScript en ASP.NET Core](https://github.com/Azure/azure-signalr/tree/dev/samples/ChatSample/ChatSample/wwwroot/index.html#L164)

    * [Cliente de C# en ASP.NET](https://github.com/Azure/azure-signalr/tree/dev/samples/AspNet.ChatSample/AspNet.ChatSample.CSharpClient/Program.cs#L78)

    * [Cliente de JavaScript en ASP.NET](https://github.com/Azure/azure-signalr/tree/dev/samples/AspNet.ChatSample/AspNet.ChatSample.JavaScriptClient/wwwroot/index.html#L71)

## <a name="next-steps"></a>Pasos siguientes

En esta guía ha aprendido sobre cómo controlar los problemas comunes. También puede aprender otros métodos de solución de problemas genéricos. 

> [!div class="nextstepaction"]
> [Solución de problemas de conectividad y entrega de mensajes](./signalr-howto-troubleshoot-method.md)