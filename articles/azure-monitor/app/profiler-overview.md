---
title: Generación de perfiles de aplicaciones de producción en Azure con Application Insights Profiler
description: Identifique la ruta de acceso activa en el código del servidor web con un generador de perfiles de bajo impacto.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 0d3074d58560df5cb5bd6bdc2c0437a4be828918
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86499399"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Generación de perfiles de aplicaciones de producción en Azure con Application Insights
## <a name="enable-application-insights-profiler-for-your-application"></a>Habilitación de Application Insights Profiler para la aplicación

Azure Application Insights Profiler proporciona un seguimiento del rendimiento de las aplicaciones que se ejecutan en producción en Azure. Profiler captura los datos automáticamente a escala sin que afecte de forma negativa a los usuarios. Profiler le ayuda a identificar la ruta de acceso "activa" al código que tarda más tiempo cuando se atiende una solicitud web determinada. 

Profiler funciona con aplicaciones .NET implementadas en los siguientes servicios de Azure. En los vínculos siguientes se pueden encontrar instrucciones específicas para habilitar Profiler para cada tipo de servicio.

* [Azure App Service](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines y Virtual Machine Scale Sets](profiler-vm.md?toc=/azure/azure-monitor/toc.json)
* [**VERSIÓN PRELIMINAR** Aplicaciones web con Linux en Azure para ASP.NET Core](profiler-aspnetcore-linux.md?toc=/azure/azure-monitor/toc.json) 

Si ha habilitado Profiler, pero no ve los seguimientos, consulte nuestra [Guía de solución de problemas.](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)

## <a name="view-profiler-data"></a>Visualización de datos de Profiler

Para que Profiler cargue los seguimientos, la aplicación debe administrar las solicitudes de forma activa. Si está realizando un experimento, puede generar solicitudes dirigidas a la aplicación web mediante [Pruebas de rendimiento de Application Insights](/vsts/load-test/app-service-web-app-performance-test). Si acaba de habilitar Profiler, puede ejecutar una breve prueba de carga. Mientras se ejecuta la prueba de carga, presione el botón **Generar perfiles ahora** en el panel [**Profiler Settings**](profiler-settings.md) (Configuración de Profiler). Una vez que Profiler está en ejecución, genera perfiles aleatoriamente una vez cada hora aproximadamente y durante dos minutos. Si la aplicación está controlando un flujo constante de solicitudes, Profiler carga los seguimientos cada hora.

Después de que la aplicación reciba tráfico y Profiler haya tenido tiempo de cargar los seguimientos, podrá verlos. Este proceso puede tardar entre 5 y 10 minutos. Para ver los seguimientos, vaya al panel **Performance** (Rendimiento), seleccione **Take Actions** (Realizar acciones) y, luego, seleccione el botón **Seguimientos de Profiler**.

![Seguimientos de Profiler de vista previa del panel Rendimiento de Application Insights][performance-blade]

Seleccione una muestra para mostrar un desglose de nivel de código del tiempo dedicado a ejecutar la solicitud.

![Explorador de seguimiento de Application Insights][trace-explorer]

El explorador de seguimiento muestra la siguiente información:

* **Mostrar ruta de acceso activa**: abre el nodo hoja más grande o al menos algo que se aproxime. En la mayoría de los casos, este nodo está cerca de un cuello de botella de rendimiento.
* **Etiqueta**: nombre de la función o el evento. El árbol muestra una combinación de código y eventos que se han producido (como, por ejemplo eventos HTTP y SQL). El evento superior representa la duración total de la solicitud.
* **Transcurrido**: el intervalo de tiempo entre el inicio y el final de la operación.
* **Cuándo**: el momento en que se ejecutó la función o el evento con relación a otras funciones.

## <a name="how-to-read-performance-data"></a>Cómo leer datos de rendimiento

Profiler de servicio de Microsoft usa una combinación de método de muestreo e instrumentación para analizar el rendimiento de la aplicación. Cuando la colección detallada está en curso, el generador de perfiles del servicio muestrea cada milisegundo el puntero de instrucción de las CPU de todas las máquinas. Cada una de las muestras captura toda la pila de llamadas del subproceso que se está ejecutando actualmente. Esto proporciona información detallada sobre lo que dicho subproceso estaba haciendo, con un grado de abstracción tanto de nivel alto como bajo. Profiler de servicio también recopila otros eventos para realizar el seguimiento de la causalidad y la correlación de actividades, como eventos de cambio de contexto, eventos de biblioteca TPL y eventos de grupo de subprocesos.

La pila de llamadas que se muestra en la vista de escala de tiempo es el resultado del muestreo y la instrumentación. Como cada muestra captura la pila de llamadas completa del subproceso, incluye código de Microsoft .NET Framework y de otros marcos a los que se haga referencia.

### <a name="object-allocation-clrjit_new-or-clrjit_newarr1"></a><a id="jitnewobj"></a>Asignación de objetos (clr!JIT\_New o clr!JIT\_Newarr1)

**clr!JIT\_New** y **clr!JIT\_Newarr1** son funciones auxiliares de .NET Framework que asignan memoria desde un montón administrado. **clr!JIT\_New** se invoca cuando se asigna un objeto. **clr!JIT\_Newarr1** se invoca cuando se asigna una matriz de objetos. Estas dos funciones suelen ser rápidas y tardan relativamente poco tiempo. Si **clr!JIT\_New** o **clr!JIT\_Newarr1** tardan mucho tiempo en la escala de tiempo, significa que el código podría estar asignando muchos objetos y consumiendo una importante cantidad de memoria.

### <a name="loading-code-clrtheprestub"></a><a id="theprestub"></a>Código de carga (clr!ThePreStub)

**clr!ThePreStub** es una función auxiliar de .NET Framework que prepara el código para ejecutarse por primera vez. Esta ejecución suele incluir, pero sin limitarse a ello, la compilación JIT (Just-In-Time). Para cada método de C#, se debe invocar **clr!ThePreStub** una vez como máximo durante un proceso.

Si **clr!ThePreStub** tarda mucho tiempo con una solicitud, significa que la solicitud es la primera que ejecuta ese método. El entorno en tiempo de ejecución de .NET Framework tarda un tiempo considerable en cargar el primer método. Podría plantearse la posibilidad de usar un proceso de preparación que ejecute esa parte del código antes de que los usuarios accedan a él, o bien ejecutar el Generador de imágenes nativo (ngen.exe) en sus ensamblados.

### <a name="lock-contention-clrjitutil_moncontention-or-clrjitutil_monenterworker"></a><a id="lockcontention"></a>Contención de bloqueo (clr!JITutil\_MonContention o clr!JITutil\_MonEnterWorker)

**clr!JITutil\_MonContention** o **clr!JITutil\_MonEnterWorker** indican que el subproceso actual está a la espera de que se libere un bloqueo. Este texto se muestra normalmente al ejecutar una instrucción **LOCK** de C#, al invocar el método **Monitor.Enter** o al invocar un método con el atributo **MethodImplOptions.Synchronized**. La contención de bloqueo se produce normalmente cuando un subproceso _A_ adquiere un bloqueo y un subproceso _B_ intenta adquirir el mismo bloqueo antes de que el subproceso _A_ lo libere.

### <a name="loading-code-cold"></a><a id="ngencold"></a>Código de carga ([COLD])

Si el nombre del método contiene **[COLD]** , por ejemplo, **mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined**, significa que el entorno en tiempo de ejecución de .NET Framework es la primera vez que ejecuta código que no está optimizado mediante la [optimización guiada por perfiles](/cpp/build/profile-guided-optimizations). Para cada método, debe presentarse como máximo una vez durante el proceso.

Si el código de carga tarda una cantidad de tiempo considerable con una solicitud, significa que la solicitud es la primera en ejecutar la parte no optimizada del método. Considere la posibilidad de usar un proceso de preparación que ejecute esa parte del código antes de que los usuarios accedan e él.

### <a name="send-http-request"></a><a id="httpclientsend"></a>Envío de una solicitud HTTP

Métodos como **HttpClient.Send** indican que el código está esperando a que finalice una solicitud HTTP.

### <a name="database-operation"></a><a id="sqlcommand"></a>Operación de base de datos

Métodos como **SqlCommand.Execute** indican que el código está a la espera de que finalice una operación de base de datos.

### <a name="waiting-await_time"></a><a id="await"></a>Espera (AWAIT\_TIME)

**AWAIT\_TIME** indica que el código está a la espera de que finalice otra tarea. Este retraso sucede normalmente con la instrucción **AWAIT** de C#. Cuando el código efectúa una instrucción **AWAIT** de C#, el subproceso se desenreda y devuelve el control al grupo de subprocesos, y no hay ningún subproceso bloqueado a la espera de que finalice **AWAIT**. Pero, lógicamente, el subproceso que realizó **AWAIT** está "bloqueado" a la espera de que finalice la operación. La instrucción **AWAIT\_TIME** indica el tiempo de bloqueo a la espera de que finalice la tarea.

### <a name="blocked-time"></a><a id="block"></a>Tiempo de bloqueo

**BLOCKED_TIME** indica que el código está a la espera de que otro recurso esté disponible. Por ejemplo, podría estar esperando un objeto de sincronización, a que un subproceso esté disponible o a que termine una solicitud.

### <a name="unmanaged-async"></a>Async no administrada

.NET Framework emite eventos ETW y pasa identificadores de actividad entre subprocesos para que se pueda realizar un seguimiento de las llamadas asincrónicas entre subprocesos. Tanto al código no administrado (código nativo) como a algunos estilos anteriores les faltan estos eventos e identificadores de actividad, de modo que el generador de perfiles no puede informar sobre qué subproceso y qué funciones se ejecutan en el subproceso. Esto se etiqueta como "Async no administrada" en la pila de llamadas. Si descarga el archivo ETW, es posible que pueda usar [PerfView](https://github.com/Microsoft/perfview/blob/master/documentation/Downloading.md) para obtener más información sobre lo que sucede.

### <a name="cpu-time"></a><a id="cpu"></a>Tiempo de CPU

La CPU está ocupada ejecutando las instrucciones.

### <a name="disk-time"></a><a id="disk"></a>Tiempo de disco

La aplicación está ejecutando operaciones de disco.

### <a name="network-time"></a><a id="network"></a>Tiempo de red

La aplicación está ejecutando operaciones de red.

### <a name="when-column"></a><a id="when"></a>Columna Cuándo

La columna **Cuándo** es una visualización de cómo varían con el tiempo las muestras INCLUSIVAS recopiladas para un nodo. El intervalo total de la solicitud se divide en 32 depósitos de tiempo. Las muestras inclusivas para ese nodo se acumulan en esos 32 depósitos. Cada depósito se representa con una barra. El alto de la barra representa un valor escalado. En el caso de nodos marcados **CPU_TIME** o **BLOCKED_TIME**, o cuando existe una relación obvia de consumo de un recurso (por ejemplo, CPU, disco o subproceso), la barra representa el consumo de uno de esos recursos durante el depósito. Para estas métricas, puede obtener un valor superior al 100 % si consume varios recursos. Por ejemplo, si por término medio usa dos CPU a lo largo de un intervalo, consigue el 200 %.

## <a name="limitations"></a>Limitaciones

El período de retención de datos predeterminado es de cinco días. El número máximo de datos ingerido al día es de 10 GB.

No hay ningún cargo por el uso del servicio Profiler. Para poder usarlo, la aplicación web debe estar hospedada al menos en el nivel básico de la característica Web Apps de Azure App Service.

## <a name="overhead-and-sampling-algorithm"></a>Sobrecarga y algoritmo de muestreo

Profiler se ejecuta de manera aleatoria dos minutos cada hora en cada máquina virtual que hospede la aplicación que tiene habilitado Profiler para capturar seguimientos. Al ejecutarse Profiler, agrega una sobrecarga de la CPU del 5 al 15 % al servidor.

## <a name="next-steps"></a>Pasos siguientes
Habilite Application Insights Profiler para la aplicación de Azure. Consulte también:
* [App Services](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines y Virtual Machine Scale Sets](profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/profiler-overview/performance-blade-v2-examples.png
[trace-explorer]: ./media/profiler-overview/trace-explorer.png
