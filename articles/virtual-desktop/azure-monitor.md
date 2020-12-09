---
title: Uso de Azure Monitor para Windows Virtual Desktop (versión preliminar) - Azure
description: Uso de Azure Monitor para Windows Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e0be6decf28fcbb2edacd5019f567d26403b1f31
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465879"
---
# <a name="use-azure-monitor-for-windows-virtual-desktop-to-monitor-your-deployment-preview"></a>Uso de Azure Monitor para Windows Virtual Desktop para supervisar implementaciones (versión preliminar)

>[!IMPORTANT]
>Azure Monitor para Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Monitor para Windows Virtual Desktop (versión preliminar) es un panel basado en libros de Azure Monitor que ayuda a que los profesionales de TI entiendan sus entornos de Windows Virtual Desktop. Este tema lo guiará a través del proceso de configuración de Azure Monitor para Windows Virtual Desktop con el objetivo de supervisar los entornos de Windows Virtual Desktop.

## <a name="requirements"></a>Requisitos

Antes de empezar a usar Azure Monitor para Windows Virtual Desktop, debe configurar lo siguiente:

- Todos los entornos de Windows Virtual Desktops que supervise deben estar basados en la versión más reciente de Windows Virtual Desktop que sea compatible con Azure Resource Manager.

- Al menos un área de trabajo de Log Analytics configurada.

- Habilite la recopilación de datos de los siguientes componentes del área de trabajo de Log Analytics:
    - Los contadores de rendimiento necesarios
    - Todos los eventos o contadores de rendimiento utilizados en Azure Monitor para Windows Virtual Desktop
    - Los datos de la herramienta de diagnóstico de todos los objetos del entorno que se va a supervisar.
    - Las máquinas virtuales del entorno que se va a supervisar.

Cualquier persona que supervise Azure Monitor para Windows Virtual Desktop en su entorno también necesitará los siguientes permisos de acceso de lectura:

- Acceso de lectura al grupo de recursos donde se encuentran los recursos del entorno

- Acceso de lectura a los grupos de recursos donde se encuentran los hosts de sesión del entorno

>[!NOTE]
> El acceso de lectura solo permite que los administradores vean los datos. Necesitarán permisos diferentes para administrar los recursos en el portal de Windows Virtual Desktop.

## <a name="open-azure-monitor-for-windows-virtual-desktop"></a>Apertura de Azure Monitor para Windows Virtual Desktop

Puede abrir Azure Monitor para Windows Virtual Desktop con uno de los métodos siguientes:

- Vaya a [aka.ms/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks).

- Busque y seleccione **Windows Virtual Desktop** en Azure Portal y, a continuación, seleccione **Conclusiones**.

- Busque y seleccione **Azure Monitor** en Azure Portal. Seleccione **Centro de conclusiones** en **Conclusiones** y, en **Otros**, seleccione **Windows Virtual Desktop** para abrir el panel en la página Azure Monitor.

Una vez que haya abierto Azure Monitor para Windows Virtual Desktop, seleccione una o varias de las casillas etiquetadas como **Suscripción**, **Grupo de recursos**, **Grupo de hosts** e **intervalo de tiempo**, en función del entorno que desee supervisar.

>[!NOTE]
>Actualmente, Windows Virtual Desktop solo permite supervisar una suscripción, un grupo de recursos y un grupo de hosts al mismo tiempo. Si no puede encontrar el entorno que desea supervisar, consulte [nuestra documentación de solución de problemas](troubleshoot-azure-monitor.md) para ver las solicitudes de características y problemas conocidos.

## <a name="set-up-with-the-configuration-workbook"></a>Proceso de configuración con el libro de configuración

Si esta es la primera vez que abre Azure Monitor para Windows Virtual Desktop, tendrá que configurar Azure Monitor para sus recursos de Windows Virtual Desktop. Para configurar los recursos:

1. Abra el libro en Azure Portal.
2. Seleccione **Open the configuration workbook** (Abrir el libro de configuración).

El libro de configuración configura el entorno de supervisión y permite comprobar la configuración después de finalizar el proceso de configuración. Es importante comprobar la configuración si los elementos del panel no se muestran correctamente o si el grupo de productos publica actualizaciones que requieren puntos de datos adicionales.

## <a name="host-pool-diagnostic-settings"></a>Configuración de diagnóstico del grupo de hosts

Deberá habilitar la configuración de diagnóstico de Azure Monitor en todos los objetos del entorno de Windows Virtual Desktop que admitan esta característica.

1. Abra Azure Monitor para Windows Virtual Desktop en [aka.ms/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks) y, a continuación, seleccione **Libro de configuración**.

2. Seleccione un entorno para supervisar en **Suscripción**, **Grupo de recursos** y **Grupo de hosts**.

3. En **Configuración de diagnóstico del grupo de hosts**, compruebe si los diagnósticos de Windows Virtual Desktop están habilitados para el grupo de hosts. Si no es así, aparecerá un mensaje de error que indica: "No se encontró ninguna configuración de diagnóstico del grupo de hosts seleccionado". 
   
   Las siguientes tablas deben estar habilitadas:

    - Punto de control
    - Error
    - Administración
    - Conexión
    - HostRegistration

    >[!NOTE]
    > Si no ve el mensaje de error, no es necesario que realice el paso 4.

4. Seleccione **Configure host pool** (Configurar grupo de hosts).

5. Seleccione **Implementar**.

6. Actualizar el libro.

Puede obtener más información sobre cómo habilitar el diagnóstico en todos los objetos del entorno de Windows Virtual Desktop o acceder al área de trabajo de Log Analytics en [Envío de diagnósticos de Windows Virtual Desktop a Log Analytics](diagnostics-log-analytics.md).

## <a name="configure-log-analytics"></a>Configuración de Log Analytics

Para empezar a usar Azure Monitor para Windows Virtual Desktop, también necesitará, al menos, un área de trabajo de Log Analytics para recopilar datos del entorno que planea supervisar y suministrarlos al libro. Si ya tiene una configuración, vaya a [Configuración de contadores de rendimiento](#set-up-performance-counters). Si desea configurar una nueva área de trabajo de Log Analytics para la suscripción de Azure que contiene el entorno de Windows Virtual Desktop, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace.md).

>[!NOTE]
>Se aplicarán los cargos de almacenamiento estándar de Log Analytics. Para empezar, se recomienda elegir el modelo de pago por uso y ajustarlo a medida que escale la implementación y recopile más datos. Para obtener más información, consulte la sección [Precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

### <a name="set-up-performance-counters"></a>Configuración de contadores de rendimiento

Debe habilitar los contadores de rendimiento específicos para la recopilación en el intervalo de ejemplo correspondiente, en el área de trabajo de Log Analytics. Estos contadores de rendimiento son los únicos contadores que necesitará para supervisar Windows Virtual Desktop. Puede deshabilitar todos los demás para ahorrar costos.

Si ya tiene los contadores de rendimiento habilitados y desea quitarlos, siga las instrucciones de [Configuración de contadores de rendimiento](../azure-monitor/platform/data-sources-performance-counters.md) para volver a configurarlos. Aunque en el artículo se describe cómo agregar contadores, también puede quitarlos en la misma ubicación.

Si aún no ha configurado los contadores de rendimiento, aquí se muestra cómo configurarlos para Azure Monitor para Windows Virtual Desktop:

1. Vaya a [aka.ms/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks) y, a continuación, seleccione el **libro de configuración** en la parte inferior de la ventana.

2. En **Configuración de Log Analytics**, seleccione el área de trabajo que ha configurado para la suscripción.

3. En **Contadores de rendimiento del área de trabajo**, verá la lista de contadores necesarios para la supervisión. En el lado derecho de la lista, active los elementos de la lista **Contadores que faltan** para habilitar los contadores que necesitará para iniciar la supervisión del área de trabajo.

4. Seleccione **Configuración de contadores de rendimiento**.

5. Seleccione **Aplicar configuración**.

6. Actualice el libro de configuración y continúe con la configuración del entorno.

También puede agregar nuevos contadores de rendimiento después de la configuración inicial, siempre que el servicio se actualice y requiera nuevas herramientas de supervisión. Puede comprobar que todos los contadores necesarios están habilitados seleccionándolos en la lista **Contadores que faltan**.

>[!NOTE]
>Los contadores de rendimiento de retraso de entrada solo son compatibles con Windows 10 RS5 y versiones posteriores o Windows Server 2019 y versiones posteriores.

Para obtener más información sobre cómo agregar manualmente contadores de rendimiento que aún no están habilitados para la recopilación, consulte [Configuración de contadores de rendimiento](../azure-monitor/platform/data-sources-performance-counters.md).

### <a name="set-up-windows-events"></a>Configuración de eventos de Windows

A continuación, deberá habilitar eventos específicos de Windows para la recopilación en el área de trabajo de Log Analytics. Los eventos descritos en esta sección son los únicos que necesita Azure Monitor para Windows Virtual Desktop. Puede deshabilitar todos los demás para ahorrar costos.

Para configurar eventos de Windows, siga estos pasos:

1. Si ya tiene eventos de Windows habilitados y desea quitarlos, quite los que no quiera antes de usar el libro de configuración con el fin de habilitar el conjunto necesario para la supervisión.

2. Vaya a Azure Monitor para Windows Virtual Desktop en [aka.ms/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks) y, a continuación, seleccione **Libro de configuración** en la parte inferior de la ventana.

3. En **Configuración de eventos de Windows**, hay una lista de eventos de Windows necesarios para la supervisión. En el lado derecho de esa lista está la lista **Eventos que faltan**, donde encontrará los nombres y tipos de eventos necesarios que actualmente no están habilitados para su área de trabajo. Registre cada uno de estos nombres para más adelante.

4. Seleccione **Open Workspaces Configuration** (Abrir configuración de áreas de trabajo).

5. Seleccione **Datos**.

6. Seleccione **Registros de eventos de Windows**.

7. Agregue los nombres de evento que faltan del paso 3 y el tipo de evento necesario para cada uno.

8. Actualice el libro de configuración y continúe con la configuración del entorno.

Puede agregar nuevos eventos de Windows después de la configuración inicial si las actualizaciones de la herramienta de supervisión requieren la habilitación de nuevos eventos. Para asegurarse de que tiene todos los eventos necesarios habilitados, vuelva a la lista **Eventos que faltan** y habilite los eventos que faltan.

## <a name="install-the-log-analytics-agent-on-all-hosts"></a>Instalación del agente de Log Analytics en todos los hosts

Por último, deberá instalar el agente de Log Analytics en todos los hosts del grupo de hosts para enviar datos desde los hosts al área de trabajo seleccionada.

Para instalar el agente de Log Analytics, haga lo siguiente:

1. Vaya a Azure Monitor para Windows Virtual Desktop en [aka.ms/azmonwvdi](https://portal.azure.com/#blade/Microsoft_Azure_WVD/WvdManagerMenuBlade/workbooks) y, a continuación, seleccione **Libro de configuración** en la parte inferior de la ventana.

2. Si Log Analytics no está configurado para todos los hosts del grupo de hosts, verá un error en la parte inferior de la sección de configuración de Log Analytics con el mensaje "Some hosts in the host pool are not sending data to the selected Log Analytics workspace" (Algunos hosts del grupo de hosts no están enviando datos al área de trabajo de Log Analytics seleccionada). Seleccione **Add hosts to workspace** (Agregar hosts al área de trabajo) para agregar los hosts seleccionados. Si no ve el mensaje de error, no continúe.

3. Actualice el libro de configuración.

>[!NOTE]
>La máquina host debe estar en ejecución para instalar la extensión de Log Analytics. Si se produce un error en la implementación automática en un host, siempre puede instalar manualmente la extensión en un host. Para obtener información sobre cómo instalar la extensión manualmente, consulte [Extensión de máquina virtual de Log Analytics para Windows](../virtual-machines/extensions/oms-windows.md).

## <a name="optional-configure-alerts"></a>Opcional: configuración de MPIO

Puede configurar Azure Monitor para Windows Virtual Desktop para que notifique si se produce alguna alerta de Azure Monitor grave en la suscripción seleccionada. Para ello, siga las instrucciones de [Respuesta a eventos con las alertas de Azure Monitor](../azure-monitor/learn/tutorial-response.md).

## <a name="diagnostic-and-usage-data"></a>Datos de diagnóstico y uso

Microsoft recopila automáticamente datos de uso y rendimiento a través del servicio Azure Monitor. Microsoft usa estos datos para mejorar la calidad, la seguridad y la integridad del servicio.

Para proporcionar funcionalidades de solución de problemas precisas y eficaces, los datos recopilados incluyen el identificador de sesión del portal, el identificador de usuario de Azure Active Directory y el nombre de la pestaña del portal donde se produjo el evento. Microsoft no recopila nombres, direcciones ni otra información de contacto.

Para más información sobre el uso y la recopilación de datos, vea la [Declaración de privacidad de Microsoft Online Services](https://privacy.microsoft.com/privacystatement).

>[!NOTE]
>Para obtener información sobre cómo ver o eliminar los datos personales recopilados por el servicio, vea [Solicitudes de titulares de los datos de Azure para el RGPD y la CCPA](/microsoft-365/compliance/gdpr-dsr-azure). Si quiere conocer más detalles del Reglamento general de protección de datos, consulte la [sección sobre RGPD del Portal de confianza de servicios](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado Windows Virtual Desktop desde Azure Portal, aquí tiene algunos recursos que pueden ayudarlo con lo siguiente:

- Consulte nuestro [glosario](azure-monitor-glossary.md) para obtener más información sobre los términos y conceptos relacionados con Azure Monitor para Windows Virtual Desktop.
- Si encuentra algún problema, consulte nuestra [guía de solución de problemas](troubleshoot-azure-monitor.md) para obtener ayuda.