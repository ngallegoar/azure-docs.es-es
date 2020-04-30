---
title: Uso del panel de configuración de Azure Application Insights Profiler | Microsoft Docs
description: Consulte el estado de Profiler e inicie sesiones de generación de perfiles
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 52391ffc8eb6b6879e5fd90df26d9ccacbfacebb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188933"
---
# <a name="configure-application-insights-profiler"></a>Configuración de Application Insights Profiler

## <a name="updated-profiler-agent"></a>Agente de Profiler actualizado
Las características del desencadenador solo funcionan con la versión 2.6 o posterior del agente de Profiler. Si está ejecutando una instancia de Azure App Service, el agente se actualizará automáticamente. Puede ver qué versión del agente está ejecutando si va a la dirección URL de KUDU para el sitio web y agrega \DiagnosticServices al final, de la siguiente manera: `https://yourwebsite.scm.azurewebsites.net/diagnosticservices`. El trabajo web de Application Insights Profiler debe tener la versión 2.6 o posterior. Puede forzar una actualización reiniciando la aplicación web. 

Si está ejecutando Profiler en una VM o un servicio en la nube, debe tener instalada la extensión de Windows Azure Diagnostics (WAD) 16.0.4 o una versión posterior. Puede comprobar la versión de WAD iniciando sesión en su VM y mirando este directorio: C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.16.0.4. El nombre del directorio es la versión de WAD que está instalada. El agente de Azure VM actualizará WAD automáticamente cuando haya nuevas versiones disponibles.

## <a name="profiler-settings-page"></a>Página de configuración de Profiler

Para abrir el panel de configuración de Azure Application Insights Profiler, vaya al panel de rendimiento de Application Insights y seleccione el botón **Configurar Profiler**.

![Vínculo para abrir la página de configuración de Profiler][configure-profiler-entry]

Esta opción abre una página que tiene este aspecto:

![Página de configuración de Profiler][configure-profiler-page]

La página **Configurar Application Insights Profiler** tiene estas características:

| | |
|-|-|
Generar perfiles ahora | inicia sesiones de generación en todas las aplicaciones vinculadas a esta instancia de Application Insights.
Desencadenadores | Esto le permite configurar desencadenadores que hacen que se ejecute la instancia de Profiler. 
Sesiones de generación de perfiles recientes | muestra información acerca de las anteriores sesiones de generación de perfiles.

## <a name="profile-now"></a>Generar perfiles ahora
Esta opción le permite iniciar una sesión de creación de perfiles bajo petición. Cuando haga clic en este vínculo, todos los agentes de Profiler que envían datos a esta instancia de Application Insights comenzarán a capturar un perfil. Después de 5 a 10 minutos, la sesión de creación del perfil se mostrará en la lista que tiene a continuación.

Para que un usuario desencadene manualmente una sesión de Profiler, requiere como mínimo tener "acceso de escritura" en su rol para el componente de Application Insights. En la mayoría de los casos, este acceso se obtiene automáticamente y no es necesario realizar ninguna tarea adicional. Si tiene problemas, el rol del ámbito de suscripción que debe agregar es el rol de "Colaborador de componentes de Application Insights". [Obtenga más información sobre el control de acceso de rol con Azure Monitoring](https://docs.microsoft.com/azure/azure-monitor/app/resources-roles-access-control).

## <a name="trigger-settings"></a>Configuración del desencadenador
![Control flotante de los ajustes del desencadenador][trigger-settings-flyout]

Al hacer clic en el botón Desencadenadores en la barra de menú, se abre el cuadro de configuración del desencadenador. Puede configurar el desencadenador para comenzar a generar perfiles cuando el porcentaje de uso de la CPU o la memoria alcanza el nivel establecido.

| | |
|-|-|
Botón de activado o desactivado | Activado: el desencadenador puede iniciar Profiler. Desactivado: el desencadenador no iniciará Profiler.
Umbral de memoria | Cuando este porcentaje de memoria está en uso, se iniciará Profiler.
Duration | Se establece el período de tiempo durante el que se ejecutará Profiler cuando se active.
Recuperación | Se establece el período de tiempo durante el cual Profiler esperará antes de comprobar nuevamente el uso de la memoria o la CPU después de que se desencadene.

## <a name="recent-profiling-sessions"></a>Sesiones de generación de perfiles recientes
En esta sección de la página se muestra información sobre las sesiones recientes de creación de perfiles. Una sesión de creación de perfiles representa el período de tiempo durante el que el agente de Profiler toma un perfil de una de las máquinas que hospedan su aplicación. Puede abrir los perfiles de una sesión haciendo clic en una de las filas. Para cada sesión, mostramos lo siguiente:

| | |
|-|-|
Desencadenado en función de | Cómo se inició la sesión, ya sea mediante un desencadenador, la opción Crear perfil o un muestreo predeterminado. 
Nombre de la aplicación | Nombre de la aplicación de la que se creó el perfil.
Instancia de máquina | Nombre de la máquina en la que se ejecutó el agente de Profiler.
Timestamp | Hora en la que se capturó el perfil.
Seguimiento | Número de seguimientos que se adjuntaron a solicitudes individuales.
% de CPU | Porcentaje de la CPU que se estaba usando mientras se ejecutaba Profiler.
% de memoria | Porcentaje de memoria que se estaba usando mientras se ejecutaba Profiler.

## <a name="use-web-performance-tests-to-generate-traffic-to-your-application"></a><a id="profileondemand"></a> Usar pruebas de rendimiento web para generar tráfico en la aplicación

Profiler se puede desencadenar manualmente con un solo clic. Imagine que está ejecutando una prueba de rendimiento web. Necesitará realizar seguimientos que le ayuden a saber si la aplicación web funciona correctamente en condiciones de carga. Poder control el momento en que se capturan los seguimientos es fundamental, ya que sabe cuándo se va a ejecutar la prueba de carga. Sin embargo, con el intervalo de muestreo aleatorio es posible que se lo pierda.

En las secciones siguientes se ilustra cómo funciona este escenario:

### <a name="step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>Paso 1: Generación de tráfico a la aplicación web mediante el inicio de una prueba de rendimiento web

Si la aplicación web tiene ya tráfico entrante o si simplemente desea generar tráfico manualmente, omita esta sección y continúe con el paso 2.

1. En el portal de Application Insights, seleccione **Configure** > **Performance Testing** (Configurar > Prueba de rendimiento). 

1. Para iniciar una nueva prueba de rendimiento, seleccione el botón **New** (Nuevo).

   ![creación de una prueba de rendimiento][create-performance-test]

1. En el panel **New performance test** (Nueva prueba de rendimiento), configure la dirección URL de destino de la prueba. Acepte la configuración predeterminada y seleccione **Run test** (Ejecutar prueba) para empezar a ejecutar la prueba de carga.

    ![Configuración de la prueba de carga][configure-performance-test]

    En primer lugar, la nueva prueba se pone en cola y,después, pasa al estado *en curso*.

    ![La prueba de carga se ha enviado y está en cola][load-test-queued]

    ![Se está ejecutando la prueba de carga y está en curso][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>Paso 2: Inicio de una sesión a petición de Profiler

1. Cuando la prueba de carga esté en ejecución, inicie Profiler para capturar los seguimientos de la aplicación web mientras se recibe la carga.

1. Vaya al panel de **configuración de Profiler**.


### <a name="step-3-view-traces"></a>Paso 3: Visualización de seguimientos

Cuando Profiler deje de ejecutarse, siga las instrucciones de la notificación para ir al panel de rendimiento y ver los seguimientos.

## <a name="next-steps"></a>Pasos siguientes
[Habilitar Profiler y ver seguimientos](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[configure-profiler-page]: ./media/profiler-settings/configureBlade.png
[trigger-settings-flyout]: ./media/profiler-settings/CPUTrigger.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png
