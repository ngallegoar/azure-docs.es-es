---
title: Solución de problemas de uso elevado de CPU en Windows Virtual Machines de Azure
description: .
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: mnanda
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: 9971cc26-916f-4e49-83cd-71eca74804f0
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 9/24/2020
ms.author: mnanda
ms.openlocfilehash: ffac5ac4d1a8143590e1d72aaafc8a02d6ab04ca
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977262"
---
# <a name="troubleshoot-high-cpu-issues-for-azure-windows-virtual-machines"></a>Solución de problemas de uso elevado de CPU en Windows Virtual Machines de Azure

## <a name="summary"></a>Resumen

Se producen problemas de rendimiento en diferentes sistemas operativos o aplicaciones, y cada problema requiere un enfoque único para solucionarlo. La mayoría de estos problemas están relacionados con la CPU, la memoria, la red y la entrada/salida (E/S) como los puntos clave donde se producen. Cada una de estas áreas genera síntomas diferentes (a veces simultáneamente) y requiere un diagnóstico y una solución diferentes.

En este artículo se describen los problemas de uso elevado de CPU que se producen en Azure Virtual Machines (VM) que se ejecutan con el sistema operativo Windows.

### <a name="high-cpu-issues-on-azure-windows-vms"></a>Problemas de uso elevado de CPU en Windows Virtual Machines de Azure

A excepción de los problemas de latencia de red y de E/S, para la solución de problemas de CPU y memoria se requieren las mismas herramientas y los mismos pasos que para los servidores locales. Una de las herramientas que Microsoft suele admitir es PerfInsights (disponible para Windows y Linux). PerfInsights puede proporcionar un diagnóstico de los procedimientos recomendados de Azure Virtual Machines en un informe descriptivo. PerfInsights también es una herramienta contenedor que puede ayudar a recopilar datos de Perfmon, Xperf y Netmon, en función de las marcas seleccionadas dentro de la herramienta.

La mayoría de las herramientas de solución de problemas de rendimiento existentes, como Perfmon o Procmon, que se usan para los servidores locales funcionarán en Windows Virtual Machines de Azure. Sin embargo, PerfInsights se ha diseñado explícitamente para que Azure Virtual Machines proporcionen más información, como procedimientos recomendados de Azure, procedimientos recomendados de SQL, grafos de latencia de E/S de alta resolución, pestañas de CPU y memoria, etc.

Tanto si se ejecuta en modo de usuario como en modo de kernel, cualquier subproceso de un proceso activo requiere ciclos de CPU para ejecutar el código desde el que se compila. Muchos problemas están directamente relacionados con la carga de trabajo. El tipo de carga de trabajo que existe en el servidor impulsa el consumo de recursos, incluida la CPU.

#### <a name="common-factors"></a>Factores comunes

Los siguientes factores son comunes en una situación de uso elevado de CPU:

- Cambio o implementación de código reciente que se aplica principalmente a aplicaciones como Internet Information Services (IIS), Microsoft SharePoint, Microsoft SQL Server o aplicaciones de terceros.

- Una actualización reciente que puede estar relacionada con una actualización de nivel de sistema operativo o con actualizaciones y correcciones acumulativas de nivel de la aplicación.

- Un cambio de consulta o índices obsoletos. Las aplicaciones de capa de datos SQL Server u Oracle también tienen una optimización del plan de consulta como otro factor. Los cambios en los datos o la falta de índices adecuados pueden hacer que algunas consultas necesiten gran cantidad de proceso.

- Específico de la máquina virtual de Azure. Hay ciertos procesos, como RDAgent, y procesos específicos de extensión, como el agente de supervisión, el agente MMA o el cliente de seguridad, que pueden provocar un uso elevado de CPU. Estos procesos se deben observar desde una perspectiva de configuración o de problemas conocidos.

## <a name="troubleshoot-the-issue"></a>Solución del problema

Este artículo se centra en aislar el proceso problemático. Un análisis más exhaustivo será específico del proceso que está impulsando el consumo elevado de CPU.

Por ejemplo, si el proceso es de SQL Server (sqlservr.exe), los pasos siguientes serán analizar qué consulta usó más ciclos de CPU en un período de tiempo específico.

### <a name="scope-the-issue"></a>Ámbito del problema

Estas son algunas preguntas que se deben formular al solucionar el problema:

- ¿Existe algún patrón del problema? Por ejemplo, ¿se produce el problema de uso elevado de CPU en un momento determinado cada día, semana o mes? Si es así, ¿puede correlacionarse este problema con un trabajo, un informe o un inicio de sesión de usuario?

- ¿Se inició el problema de uso elevado de CPU después de un cambio de código reciente? ¿Aplicó una actualización en Windows o en una aplicación?

- ¿Se inició el problema de uso elevado de CPU después de un cambio en la carga de trabajo, como un aumento del número de usuarios, un influjo de datos superior o un número mayor de informes?

- En el caso de Azure, ¿se inició el problema de uso elevado de CPU en alguna de las siguientes condiciones?

  - Después de una reimplementación o reinicio reciente
  - Cuando se cambió un tipo de máquina virtual o SKU
  - Cuando se agregó una nueva extensión
  - Después de realizar cambios en el equilibrador de carga

### <a name="azure-caveats"></a>Advertencias de Azure

Sepa cuál es su carga de trabajo. Al seleccionar una máquina virtual, puede subestimar los recuentos de CPU virtual (vCPU) si mira el costo general de hospedaje mensual. Si la carga de trabajo requiere gran cantidad de proceso, la selección de una SKU de máquina virtual menor, que tenga una o dos vCPU, puede producir problemas de carga de trabajo. Pruebe diferentes configuraciones para la carga de trabajo a fin de determinar la mejor capacidad de proceso necesaria.

Hay ciertas series de máquinas virtuales, como la serie B (modo de ráfaga), que se recomiendan para el control de calidad (QA) y las pruebas. El uso de estas series en el entorno de producción limita la capacidad de proceso una vez agotados los créditos de CPU.

En el caso de las aplicaciones conocidas, como SQL Server, Oracle, RDS (Servicios de Escritorio remoto), Windows Virtual Desktop, IIS o SharePoint, hay artículos sobre procedimientos recomendados de Azure que incluyen recomendaciones para la configuración mínima de estas cargas de trabajo.

### <a name="ongoing-high-cpu-issues"></a>Problemas continuos de uso elevado de CPU

Si el problema se produce en este momento, es la mejor oportunidad de capturar el seguimiento del proceso para determinar la causa del problema. Puede usar las herramientas existentes que ha estado usando en los servidores de Windows locales con el fin de localizar el proceso. El Soporte técnico de Azure recomienda las siguientes herramientas para Azure Virtual Machines.

### <a name="perfinsights"></a>PerfInsights

PerfInsights es la herramienta recomendada por el soporte técnico de Azure para problemas de rendimiento de las máquinas virtuales. Está diseñada para cubrir los procedimientos recomendados y las pestañas dedicadas de análisis para la CPU, la memoria y los grafos de E/S de alta resolución. Puede ejecutarla a petición, a través de Azure Portal o desde la máquina virtual. Puede compartir los datos con el equipo de soporte técnico de Azure.

#### <a name="run-perfinsights"></a>Ejecución de PerfInsights

PerfInsights está disponible para los sistemas operativos [Windows](./how-to-use-perfinsights.md) y [Linux](./how-to-use-perfinsights-linux.md). Para Windows, las opciones son las siguientes.

#### <a name="run-and-analyze-reports-through-azure-portal"></a>Ejecución y análisis de informes a través de Azure Portal

Cuando se [instala a través de Azure Portal](./performance-diagnostics.md), lo que realmente se instala es una extensión en la máquina virtual. Los usuarios también pueden instalar PerfInsights como una extensión; para ello, vaya directamente a la [hoja de extensiones de máquina virtual](./performance-diagnostics-vm-extension.md) y, después, elija una opción de diagnóstico de rendimiento.

#### <a name="azure-portal-option-1"></a>Opción 1 de Azure Portal

Vaya a la hoja de la máquina virtual y seleccione la opción **Diagnóstico de rendimiento**. Se le pedirá que instale la opción (usa extensiones) en la máquina virtual para la que la seleccionó.

  ![Instalación de los diagnósticos de rendimiento](./media/troubleshoot-high-cpu-issues-azure-windows-vm/1-install-performance-diagnostics.png)

#### <a name="azure-portal-option-2"></a>Opción 2 de Azure Portal

Vaya a **Diagnosticar y solucionar problemas** en la hoja de la máquina virtual y busque **Problemas de rendimiento de máquinas virtuales**.

  ![Solución de problemas de rendimiento de máquinas virtuales](./media/troubleshoot-high-cpu-issues-azure-windows-vm/2-troubleshoot-vm-performance-issues.png)

Si selecciona **Solucionar problemas**, se carga la pantalla de instalación de PerfInsights.

Si selecciona **Instalar**, la instalación proporciona distintas opciones de recopilación.

  ![Análisis de rendimiento](./media/troubleshoot-high-cpu-issues-azure-windows-vm/3-performance-analysis.png)

Las opciones numeradas de la captura de pantalla están relacionadas con los siguientes comentarios:

1. Para la opción de **High-CPU** (CPU elevada), seleccione **Análisis de rendimiento** u **Opciones avanzadas**.

2. Al agregar los síntomas aquí, se agregarán al informe, lo que le ayudará a compartir información con el Soporte técnico de Azure.

3. Seleccione la duración de la recopilación de datos. Para la opción de uso elevado de CPU, seleccione al menos 15 minutos o más. En el modo de Azure Portal, puede recopilar hasta 15 minutos de datos. Para períodos más largos de recopilación, debe ejecutar el programa como un ejecutable dentro de la máquina virtual.

4. Si el Soporte técnico de Azure le pregunta si desea recopilar estos datos, puede agregar el número de vale aquí. Este campo es opcional.

5. Seleccione este campo para aceptar el Contrato de licencia para el usuario final (CLUF).

6. Seleccione este campo si desea que este informe esté disponible para el equipo de Soporte técnico de Azure que le ayuda en este caso.

El informe se almacena en una de las cuentas de almacenamiento de su suscripción. Está disponible para su consulta y descarga posterior.

#### <a name="run-perfinsights-from-within-the-vm"></a>Ejecución de PerfInsights desde dentro de la máquina virtual

Este método se puede usar si pretende ejecutar PerfInsights durante períodos más largos. En el [artículo de PerfInsights](./how-to-use-perfinsights.md#how-do-i-run-perfinsights) se ofrece un tutorial detallado de los distintos comandos y marcas necesarios para ejecutar PerfInsights como archivo ejecutable. Para el uso elevado de CPU, necesitará uno de los siguientes modos:

- Escenario avanzado

  - `PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics`

- Escenario de máquinas virtuales lentas (rendimiento)

  - `PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>`

La salida del comando estará en la misma carpeta donde guardó el archivo ejecutable de PerfInsights.

#### <a name="what-to-look-for-in-the-report"></a>Lo que debe buscar en el informe

Después de ejecutar el informe, la ubicación del contenido depende de si se ejecutó mediante Azure Portal o como un ejecutable. Con cualquiera de las dos opciones, puede acceder a la carpeta de registro generada o descargarla localmente (si usa Azure Portal) para su análisis.

### <a name="run-through-the-azure-portal"></a>Ejecución mediante Azure Portal

  ![Diagnósticos de rendimiento de gran impacto](./media/troubleshoot-high-cpu-issues-azure-windows-vm/4-high-impact-performance-diagnostics.png)

  ![Descargar informe](./media/troubleshoot-high-cpu-issues-azure-windows-vm/5-download-report.png)

#### <a name="run-from-within-vm"></a>Ejecución desde dentro de la máquina virtual

La estructura de la carpeta debe ser similar a la de las siguientes imágenes:

  ![Seleccionar la salida](./media/troubleshoot-high-cpu-issues-azure-windows-vm/6-select-output.png)

  ![Estructura de carpetas](./media/troubleshoot-high-cpu-issues-azure-windows-vm/7-folder-structure.png)

1. Las recopilaciones adicionales, como Perfmon, Xperf, Netmon, registros SMB, registros de eventos, etc., se pueden encontrar en la carpeta de salida.

1. El informe real junto con el análisis y las recomendaciones.

1. Tanto para el rendimiento (VMslow) como para las opciones avanzadas, el informe recopila información de **perfmon** mientras dura la ejecución de PerfInsights.

1. Los registros de eventos muestran una vista rápida de los detalles útiles de los bloqueos de nivel de sistema o de proceso.

#### <a name="where-to-start"></a>Por dónde empezar

Abra el informe de PerfInsights. En la pestaña **Resultados** se registran los valores atípicos en términos de consumo de recursos. Si hay instancias de uso elevado de CPU, la pestaña **Resultados** las clasificará como de gran impacto o de impacto medio.

  ![Recursos de nivel de impacto](./media/troubleshoot-high-cpu-issues-azure-windows-vm/8-impact-level-resources.png)

De forma similar al ejemplo anterior, PerfInsights se ejecutó durante 30 minutos. Durante la mitad del tiempo, el proceso resaltado estuvo agotando la CPU en el lado superior. Si el mismo proceso se hubiese ejecutado durante el tiempo de recopilación, el nivel de impacto habría cambiado a **ALTO**.

Si expande el evento **Resultados**, verá varios detalles clave. En la pestaña se enumeran los procesos en orden descendente, por consumo de **Uso medio de CPU** y se muestra si el proceso estaba relacionado con el sistema, una aplicación propiedad de Microsoft (SQL, IIS) o un proceso de terceros.

#### <a name="more-details"></a>Más detalles

Hay una subpestaña dedicada en **CPU** que podría usarse para realizar el análisis detallado de patrones, por núcleo o por proceso.

La pestaña **Top CPU Consumers** (Principales consumidores de CPU) contiene dos secciones de interés independientes y se pueden ver las estadísticas por procesador. El diseño de la aplicación suele ser de un solo subproceso o se ancla a un solo procesador. En este escenario, uno o varios núcleos se ejecutan al 100 por ciento, mientras que otros núcleos se ejecutan en los niveles esperados. Estos escenarios son más complejos porque el uso medio de CPU en el servidor parece ejecutarse según lo esperado, pero los procesos que están anclados a núcleos que tienen un uso elevado serán más lentos de lo esperado.

  ![Uso elevado de CPU](./media/troubleshoot-high-cpu-issues-azure-windows-vm/9-high-cpu-usage.png)

La segunda sección (igualmente importante) es **Top Long Running CPU Consumers** (Principales consumidores de CPU de larga duración). En esta sección se muestran los detalles del proceso y su patrón de uso de CPU. La lista se ordena colocando en la parte superior los consumidores de CPU de promedio alto.

  ![Principales consumidores de CPU de larga duración](./media/troubleshoot-high-cpu-issues-azure-windows-vm/10-top-long-running-cpu-consumers.png)

Estas dos pestañas serán suficientes para establecer la ruta para los siguientes pasos de solución de problemas. En función del proceso que esté impulsando la condición de uso elevado de CPU, tendrá que responder a las preguntas que se formularon anteriormente. Los procesos como SQL Server (sqlservr. exe) o IIS (w3wp. exe) requieren una exploración en profundidad específica de los cambios de la consulta o del código que causan esta condición. Para los procesos del sistema como WMI o Lsass.exe, debe seguir una ruta diferente.

En el caso de los procesos relacionados con la máquina virtual de Azure, como RDAgent, OMS y los ejecutables de la extensión de supervisión, es posible que tenga que corregir una nueva compilación o versión con ayuda del equipo de Soporte técnico de Azure.

### <a name="perfmon"></a>Perfmon

**Perfmon** es una de las primeras herramientas para solucionar un problema de recursos en Windows Server. No proporciona un informe claro con recomendaciones o resultados. En su lugar, requiere que el usuario explore los datos recopilados y use un filtro específico en las distintas categorías de contador.

PerfInsights recopila Perfmon como registro adicional para los escenarios VMSlow y avanzado. Sin embargo, Perfmon se puede recopilar de forma independiente y tiene estas ventajas adicionales:

- Se puede recopilar de forma remota.

- Se puede programar a través de **Tareas**.

- Se puede recopilar durante períodos más largos o en modo continuo mediante el uso de la característica de sustitución.

Considere el mismo ejemplo que se muestra en PerfInsights para ver cómo Perfmon muestra estos datos. Las categorías de contadores necesarias son las siguientes:

- Información del procesador > %ProcessorTime > _Total

- Proceso > %ProcessorTime > Todas las instancias

#### <a name="where-to-start"></a>Por dónde empezar

Los nombres de los archivos de salida de Perfmon tienen la extensión `.blg`. Puede recopilar estos archivos de forma independiente o mediante PerfInsights. En este debate, usará `.blg` de Perfmon que se incluye en los datos de PerfInsights y que se recopilaron en el ejemplo anterior.

No hay informes predeterminados listos para el usuario disponibles en Perfmon. Hay diferentes vistas que cambian el tipo de grafo, pero el filtrado de procesos (o el trabajo necesario para identificar los procesos responsables) es manual.

> [!NOTE]
> La [herramienta PAL](https://github.com/clinthuffman/PAL) puede consumir archivos `.blg` y generar informes detallados.

Para empezar, seleccione la categoría **Agregar contadores**.

1. En **Contadores disponibles**, seleccione el contador **%ProcessorTime** en la categoría **Información del procesador**.

1. Seleccione **_Total**, que le proporciona las estadísticas de todos los núcleos combinados.

1. Seleccione **Agregar**. En la ventana se muestra **%ProcessorTime** en **Contadores agregados**.

  ![Adición de tiempo de procesador](./media/troubleshoot-high-cpu-issues-azure-windows-vm/11-add-processor-time.png)

Una vez cargados los contadores, verá gráficos de la línea de tendencia en el marco del tiempo de recopilación. Puede seleccionar o borrar los contadores. Hasta ahora, ha agregado un solo contador.

  ![Configuración del monitor de rendimiento](./media/troubleshoot-high-cpu-issues-azure-windows-vm/12-performance-monitor-1.png)

Cada contador tendrá los valores **Promedio**, **Mínimo** y **Máximo**. Céntrese en los valores **Promedio** y **Máximo**, ya que el valor promedio puede variar en función de la duración de la recopilación de datos. Si la actividad de uso elevado de CPU se ha detectado durante 10 minutos cuando la recopilación global fue de 40 minutos, los valores promedio serán mucho menores.

En el grafo de tendencia anterior se muestra que el **tiempo total de procesador** estuvo cerca del 80 por ciento durante aproximadamente 15 minutos.

#### <a name="identify-the-process"></a>Identificación del proceso

Hemos identificado que el servidor tuvo un consumo elevado de CPU durante un período de tiempo especificado, pero aún no hemos identificado el controlador. A diferencia de cuando se usa PerfInsights, en este caso debe buscar manualmente el proceso responsable.

Para esta tarea, debe borrar o quitar los contadores **%ProcessorTime** agregados anteriormente y, a continuación, agregar una nueva categoría:

- Proceso > %ProcessorTime > Todas las instancias

Esta categoría cargará los contadores de todos los procesos que se ejecutan en ese momento.

  ![Adición de contadores](./media/troubleshoot-high-cpu-issues-azure-windows-vm/13-add-counters.png)

En un equipo de producción típico, se pueden ejecutar cientos de procesos. Por lo tanto, puede que tarde un rato en borrar todos los contadores que parezcan tener un grafo de tendencia bajo o plano.

Para acelerar este proceso, utilice la vista **Histograma** y cambie el tipo de vista de **Línea** a **Histograma**, que le proporcionará un grafo de barras. Le resultará más fácil elegir los procesos que experimentan un uso elevado de CPU durante el tiempo de recopilación.

Dado que siempre habrá una barra para **Total**, céntrese en las barras que muestran una alta tasa de agotamiento. Puede eliminar las demás barras para limpiar la vista. Ahora, vuelva a la vista **Línea**.

  ![Indicadores del monitor de rendimiento](./media/troubleshoot-high-cpu-issues-azure-windows-vm/14-performance-monitor-2.png)

Ahora es más fácil detectar el proceso responsable. De forma predeterminada, los valores **Máx.** y **Mín.** son múltiplos del número de núcleos del servidor o de subprocesos del proceso.

  ![Resultados del monitor de rendimiento](./media/troubleshoot-high-cpu-issues-azure-windows-vm/15-performance-monitor-3.png)

La lista de herramientas disponibles no finaliza en PerfInsights para Perfmon. Tiene acceso a otras herramientas, como **ProcessMonitor** (ProcMon) o **Xperf**. Existen muchas herramientas de terceros que se pueden usar según sea necesario.

### <a name="azure-monitoring-tools"></a>Herramientas de supervisión de Azure

Azure Virtual Machines tiene métricas de confianza que incluyen información básica, como la CPU, la E/S de red y los bytes de E/S. En el caso de las métricas avanzadas, como Azure Monitor, tendrá que realizar solo algunas selecciones para configurar y usar la cuenta de almacenamiento que especifique.

#### <a name="basic-default-counters"></a>Contadores básicos (predeterminados)

  ![Métricas de gráfico](./media/troubleshoot-high-cpu-issues-azure-windows-vm/16-chart-metrics.png)

#### <a name="enabling-azure-monitor"></a>Habilitación de Azure Monitor

Después de habilitar las métricas de Azure Monitor, el software instala una extensión en la máquina virtual y, a continuación, comienza a recopilar métricas pormenorizadas, lo que incluye los contadores de Perfmon.

  ![Cuenta de almacenamiento de diagnóstico](./media/troubleshoot-high-cpu-issues-azure-windows-vm/17-diagnostics-storage-account.png)

La categoría de contador **Básico** se establece como **predeterminado**. Sin embargo, también puede establecer una recopilación **Personalizada**.

  ![Contadores de rendimiento](./media/troubleshoot-high-cpu-issues-azure-windows-vm/18-performance-counters.png)

Una vez habilitada la configuración, puede ver estos contadores **Invitado** en la sección **Métrica**. También puede establecer **Alertas** (incluidos los mensajes de correo electrónico) si las métricas alcanzan un umbral determinado.

  ![Espacio de nombres de métricas](./media/troubleshoot-high-cpu-issues-azure-windows-vm/19-metrics-namespace.png)

Para más información sobre cómo usar Azure Monitor para administrar las máquinas virtuales de Azure, consulte [Supervisión de máquinas virtuales de Azure con Azure Monitor](../../azure-monitor/insights/monitor-vm-azure.md).

### <a name="reactive-troubleshooting"></a>Solución de problemas reactiva

Si el problema ya se ha producido, en primer lugar debe descubrir qué causó el problema de uso elevado de CPU. La postura reactiva puede resultar complicada. El modo de recopilación de datos no será tan útil porque el problema ya se ha producido.

Si el problema se ha producido una sola vez, puede ser difícil determinar qué aplicación lo causó. Si la máquina virtual de Azure se configuró para usar OMS u otro seguimiento de diagnóstico, todavía puede obtener información sobre la causa del problema.

Si se trata de un patrón de repetición, recopile los datos durante el tiempo en que es probable que el problema se produzca de nuevo.

PerfInsights todavía no tiene una capacidad de **ejecución programada**. Sin embargo, Perfmon se puede ejecutar y programar a través de la línea de comandos.

### <a name="logman-command"></a>Comando logman

El comando **Logman Create Counter** se usa para ejecutar la recopilación de Perfmon a través de la línea de comandos, para programarlo a través del **Administrador de tareas** o para ejecutarlo de forma remota.

**Ejemplo** (incluye el modo de recopilación remota)

`Logman create counter LOGNAME -u DOMAIN\USERNAME * -f bincirc -v mmddhhmm -max 300 -c "\\SERVERNAME\LogicalDisk(*)\*" "\\SERVERNAME\Memory\*" "\\SERVERNAME\Network Interface(*)\*" "\\SERVERNAME\Paging File(*)\*" "\\SERVERNAME\PhysicalDisk(*)\*" "\\SERVERNAME\Process(*)\*" "\\SERVERNAME\Redirector\*" "\\SERVERNAME\Server\*" "\\SERVERNAME\System\*" "\\SERVERNAME\Terminal Services\*" "\\SERVERNAME\Processor(*)\*" "\\SERVERNAME\Cache\*" -si 00:01:00`

Logman.exe también se puede iniciar desde un equipo con una máquina virtual de Azure del mismo nivel en la misma red virtual.

Para más información sobre estos parámetros, vea [logman create counter](/windows-server/administration/windows-commands/logman-create-counter).

Una vez recopilados los datos de Perfmon mientras se produce el problema, los pasos restantes para analizar los datos son los mismos que los descritos anteriormente.

## <a name="conclusion"></a>Conclusión

Para cualquier problema de rendimiento, es fundamental comprender la carga de trabajo para resolver el problema. Las opciones de las diferentes SKU de máquina virtual y las distintas opciones de almacenamiento en disco deben evaluarse manteniendo el foco en la carga de trabajo de producción. El proceso de prueba de soluciones en diferentes máquinas virtuales puede ayudarle a tomar la mejor decisión.

Dado que las operaciones de usuario y la cantidad de datos varían, mantenga siempre un búfer en el proceso, redes y capacidades de E/S de la máquina virtual. Ahora, cualquier cambio repentino en la carga de trabajo no tiene un efecto importante.

Si prevé que la carga de trabajo va a aumentar próximamente, mueva a una SKU superior que tenga más potencia de proceso. Si la carga de trabajo va a requerir de gran cantidad de proceso, elija las SKU de máquina virtual de forma inteligente.