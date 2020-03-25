---
title: Solución de problemas de cambios en una máquina virtual de Azure | Microsoft Docs
description: Use Change Tracking para solucionar problemas de cambios en una máquina virtual de Azure.
services: automation
ms.subservice: change-inventory-management
keywords: cambio, seguimiento, automatización
ms.date: 12/05/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 60ca1ef3d5c14a0f3dea5b662fc5c95184e6574d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75420633"
---
# <a name="troubleshoot-changes-in-your-environment"></a>Solución de problemas en el entorno

En este tutorial aprenderá a solucionar problemas de cambios en una máquina virtual de Azure. Si habilita Change Tracking, podrá hacer un seguimiento de los cambios de software, archivos, demonios de Linux, servicios de Windows y claves del Registro de Windows en las máquinas.
Identificar esos cambios de configuración puede ayudarle a localizar problemas operativos de su entorno.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una máquina virtual para Change Tracking e Inventario
> * Buscar servicios detenidos en los registros de cambios
> * Configurar el seguimiento de cambios
> * Habilitar la conexión del registro de actividad
> * Desencadenar un evento
> * Ver los cambios
> * Configurar alertas

## <a name="prerequisites"></a>Prerequisites

Para completar este tutorial, necesita:

* Suscripción a Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para contener los runbooks de monitor y de acción y la tarea de monitor.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para incorporar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

En primer lugar, debe habilitar Change Tracking e Inventario para la máquina virtual en este tutorial. Si habilitó previamente otra solución de automatización para una máquina virtual, este paso no es necesario.

1. En el menú de la izquierda, seleccione **Máquinas virtuales** y seleccione una máquina virtual de la lista.
1. En el menú de la izquierda, en la sección **OPERACIONES**, haga clic en **Inventario**. Se abre la página **Change Tracking**.

![Habilitar cambios](./media/automation-tutorial-troubleshoot-changes/enableinventory.png) Se abre la pantalla **Change Tracking**. Configure la ubicación, el área de trabajo de Log Analytics y la cuenta de Automation que use y haga clic en **Habilitar**. Si los campos aparecen atenuados, significa que otra solución de automatización está habilitada para la máquina virtual y que deben usarse la misma área de trabajo y cuenta de Automation.

Un área de trabajo de [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) se usa para recopilar datos que se generan mediante características y servicios, como, por ejemplo, Inventario.
El área de trabajo proporciona una única ubicación para revisar y analizar datos desde varios orígenes.

Durante la incorporación, la máquina virtual se aprovisiona con Microsoft Monitoring Agent (MMA) e Hybrid Worker.
Este agente se usa para comunicarse con la máquina virtual y obtener información sobre el software instalado.

La habilitación de la solución puede tardar hasta 15 minutos. Durante este tiempo, no debería cerrar la ventana del explorador.
Después de habilitar la solución, la información sobre el software instalado y los cambios en la máquina virtual se pasa a los registros de Azure Monitor.
Los datos pueden tardar entre 30 minutos y 6 horas en estar disponibles para el análisis.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-change-tracking-in-azure-monitor-logs"></a>Uso del seguimiento de cambios en los registros de Azure Monitor

El seguimiento de cambios genera datos de registro que se envían a los registros de Azure Monitor.
Para buscar los registros mediante la ejecución de consultas, seleccione **Log Analytics** en la parte superior de la ventana **Seguimiento de cambios**.
Los datos de Change Tracking se almacenan con el tipo **ConfigurationChange**.
La siguiente consulta de Log Analytics de ejemplo devuelve todos los servicios de Windows que se hayan detenido.

```loganalytics
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Para más información sobre la ejecución y la búsqueda de archivos de registro en los registros de Azure Monitor, consulte el artículo sobre los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="configure-change-tracking"></a>Configuración de Change Tracking

Change Tracking le ofrece la posibilidad de realizar un seguimiento de los cambios en la configuración de su máquina virtual. En los pasos siguientes se muestra cómo configurar el seguimiento de las claves del Registro y los archivos.

Para elegir de qué archivos y claves del Registro se realizará la recopilación y el seguimiento, seleccione **Editar configuración** en la parte superior de la página **Change Tracking**.

> [!NOTE]
> Change Tracking e Inventario usan la misma configuración de recopilación y se configuran en un nivel de área de trabajo.

En la ventana **Configuración del área de trabajo**, agregue las claves del Registro de Windows, los archivos de Windows o los archivos de Linux de los que desea realizar un seguimiento, tal como se describe en las tres secciones siguientes.

### <a name="add-a-windows-registry-key"></a>Agregar una clave del Registro de Windows

1. En la pestaña **Registro de Windows**, seleccione **Agregar**.
    Se abrirá la ventana **Agregar Registro de Windows para el seguimiento de cambios**.

1. En **Agregar Registro de Windows para el seguimiento de cambios**, introduzca la información de la clave cuyo seguimiento se va a realizar y haga clic en **Guardar**.

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos        |
|Clave del registro de Windows   | La ruta de acceso para buscar el archivo, por ejemplo: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="add-a-windows-file"></a>Agregar un archivo de Windows

1. En la pestaña **Archivos de Windows**, seleccione **Agregar**. Se abrirá la ventana **Agregar archivo de Windows para el seguimiento de cambios**.

1. En **Agregar archivo de Windows para el seguimiento de cambios**, introduzca la información del archivo o directorio cuyo seguimiento se va a realizar y haga clic en **Guardar**.

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos        |
|Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo, por ejemplo: "c:\temp\\\*.txt"<br>También puede usar variables de entorno, como "%winDir%\System32\\\*. *"         |
|Recursividad     | Determina si se usa recursividad al buscar el elemento cuyo seguimiento se va a realizar.        |
|Cargar archivo de contenido para toda la configuración| Activa o desactiva la carga de contenido de archivos en los cambios sometidos a seguimiento. Opciones disponibles: **True** o **False**.|

### <a name="add-a-linux-file"></a>Agregar un archivo de Linux

1. En la pestaña **Archivos de Linux**, seleccione **Agregar**. Se abrirá la ventana **Agregar archivo de Linux para el seguimiento de cambios**.

1. En **Agregar archivo de Linux para el seguimiento de cambios**, introduzca la información del archivo o directorio cuyo seguimiento se va a realizar y haga clic en **Guardar**.

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos        |
|Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo, por ejemplo: "/etc/*.conf"       |
|Tipo de ruta de acceso     | Tipo de elemento cuyo seguimiento se va a realizar; posibles valores son Archivo y Directorio        |
|Recursividad     | Determina si se usa recursividad al buscar el elemento cuyo seguimiento se va a realizar.        |
|Usar sudo     | Esta configuración determina si se va a utilizar sudo al buscar el elemento.         |
|Vínculos     | Esta configuración determina cómo se tratan los vínculos simbólicos cuando se recorren directorios.<br> **Omitir**: ignora los vínculos simbólicos y no incluye los archivos y directorios de referencia.<br>**Seguir**: sigue los vínculos simbólicos durante la recursión y también incluye los archivos y directorios de referencia.<br>**Administrar**: sigue los vínculos simbólicos y permite modificar el tratamiento del contenido devuelto.      |
|Cargar archivo de contenido para toda la configuración| Activa o desactiva la carga de contenido de archivos en los cambios sometidos a seguimiento. Opciones disponibles: **True** o **False**.|

   > [!NOTE]
   > La opción de administración de vínculos no se recomienda. No se admite la recuperación de contenido de los archivos.

## <a name="enable-activity-log-connection"></a>Habilitar la conexión del registro de actividad

Desde la página **Change Tracking** en la máquina virtual, seleccione **Administrar conexión de registro de actividad**. Esta tarea abrirá la página **Registro de actividad de Azure**. Seleccione **Conectar** para conectar Change Tracking al registro de actividad de Azure en la máquina virtual.

Con esta opción habilitada, navegue hasta la página **Información general** en la máquina virtual y seleccione **Detener** para detenerla. Cuando se le solicite, seleccione **Sí** para detener la máquina virtual. Cuando esté desasignada, seleccione **Iniciar** para reiniciar la máquina virtual.

Si se detiene y se inicia una máquina virtual, se registra un evento en el registro de actividad. Vuelva a la página **Change Tracking**. Seleccione la pestaña **Eventos** en la parte inferior de la página. Después de un tiempo, los eventos se mostrarán en el gráfico y la tabla. Al igual que en el paso anterior, cada evento puede seleccionarse para ver información detallada.

![Visualización de los detalles de cambio en el portal](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Ver los cambios

Una vez habilitadas la solución Change Tracking e Inventario, puede ver los resultados en la página **Change tracking**.

Desde dentro de la máquina virtual, seleccione **Change Tracking** en **OPERACIONES**.

![Captura de pantalla que muestra la lista de cambios de la máquina virtual](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

El gráfico muestra los cambios que se han producido con el tiempo.
Después de agregar una conexión del registro de actividad, el gráfico de líneas en la parte superior muestra los eventos del registro de actividad de Azure.
Cada fila de los gráficos de barras representa un tipo de cambio diferente del cual se puede realizar un seguimiento.
Estos tipos son demonios de Linux, archivos, claves del Registro de Windows, software y servicios de Windows.
En la pestaña de cambios se muestran los detalles de los cambios mostrados en la visualización en orden descendente según la hora en la cual se produjo el cambio (los más recientes se muestran primero).
En la tabla de la pestaña **Eventos** se muestran los eventos del registro de actividad conectados y sus detalles correspondientes, con el más reciente primero.

En los resultados puede ver que se produjeron varios cambios en el sistema, incluidos cambios en el software y los servicios. Puede usar los filtros en la parte superior de la página para filtrar los resultados por **Tipo de cambio** o por el intervalo de tiempo.

Seleccione un cambio de tipo **WindowsServices**. Se abrirá la ventana **Detalles del cambio**. En la ventana de detalles del cambio se muestran los detalles, así como los valores antes y después del cambio. El servicio de protección de software se detuvo en esta instancia.

![Visualización de los detalles de cambio en el portal](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="configure-alerts"></a>Configurar alertas

Ver los cambios en Azure Portal puede ser útil, pero poder recibir una alerta cuando ocurre un cambio, como por ejemplo cuando se detiene un servicio, es más beneficioso.

Para agregar una alerta para un servicio detenido, en Azure Portal, vaya a **Monitor**. Y, después, en **Servicios compartidos**, seleccione **Alertas** y haga clic en **+ Nueva regla de alertas**.

Haga clic en **Seleccionar** para elegir un recurso. En la página **Seleccionar un recurso**, seleccione **Log Analytics** en el menú desplegable **Filtrar por tipo de recurso**. Seleccione el área de trabajo de Log Analytics y, a continuación, seleccione **Listo**.

![Selección de un recurso](./media/automation-tutorial-troubleshoot-changes/select-a-resource.png)

Haga clic en **Agregar condición**, en la página **Configurar lógica de señal**, en la tabla, seleccione **Búsqueda de registros personalizada**. Escriba la consulta siguiente en el cuadro de texto de consulta de búsqueda:

```loganalytics
ConfigurationChange | where ConfigChangeType == "WindowsServices" and SvcName == "W3SVC" and SvcState == "Stopped" | summarize by Computer
```

Esta consulta devuelve los equipos que tenían el servicio W3SVC detenido en el tiempo especificado.

En **Lógica de alerta**, en **Umbral**, escriba **0**. Cuando haya finalizado, seleccione **Listo**.

![Configuración de la lógica de señal](./media/automation-tutorial-troubleshoot-changes/configure-signal-logic.png)

En **Grupos de acciones**, seleccione **Crear nuevo**. Un grupo de acciones es un conjunto de acciones que puede usar en varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks y muchas más. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

En **Detalles de la alerta**, escriba un nombre y una descripción para la alerta. Establezca **Gravedad** en **Informativo (gravedad 2)** , **Advertencia (gravedad 1)** o **Crítico (gravedad 0)** .

En el cuadro **Nombre del grupo de acción**, escriba un nombre para la alerta y un nombre corto. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

En **Acciones**, escriba un nombre para la acción, como **Administradores de correo electrónico**. En **Tipo de acción**, seleccione **Correo electrónico/SMS/Push/Voz**. En **DETALLES**, seleccione **Editar detalles**.

![Adición del grupo de acciones](./media/automation-tutorial-troubleshoot-changes/add-action-group.png)

En el panel **Correo electrónico/SMS/Push/Voz**, escriba un nombre. Seleccione la casilla de verificación **Correo electrónico** y, a continuación, escriba una dirección de correo electrónico válida. Haga clic en **Aceptar** en la página **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz) y haga clic en **Aceptar** en la página **Agregar grupo de acciones**.

Para personalizar el asunto de las alertas por correo electrónico, en **Crear regla**, en **Personalizar las acciones**, seleccione **Asunto del correo electrónico**. Cuando finalice, seleccione **Crear regla de alertas**. La regla le indica cuando la implementación de una actualización es correcta y qué máquinas formaron parte de la ejecución de la implementación de actualizaciones.

La siguiente imagen es un ejemplo de correo electrónico recibido cuando el servicio W3SVC se detiene.

![email](./media/automation-tutorial-troubleshoot-changes/email.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Incorporar una máquina virtual para Change Tracking e Inventario
> * Buscar servicios detenidos en los registros de cambios
> * Configurar el seguimiento de cambios
> * Habilitar la conexión del registro de actividad
> * Desencadenar un evento
> * Ver los cambios
> * Configurar alertas

Continúe hacia la introducción sobre la solución Change Tracking e Inventario para obtener más información.

> [!div class="nextstepaction"]
> [Solución de Inventario y administración de cambios](automation-change-tracking.md)

