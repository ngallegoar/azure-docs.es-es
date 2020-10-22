---
title: Solución de problemas asociados a los cambios en una máquina virtual de Azure en Azure Automation| Microsoft Docs
description: Este artículo explica cómo solucionar los problemas asociados a los cambios en una máquina virtual de Azure.
services: automation
ms.subservice: change-inventory-management
keywords: change, tracking, change tracking, inventory, automation
ms.date: 12/05/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 28c440f27dcbd4ac509adea83d5c3085488cb488
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92204256"
---
# <a name="troubleshoot-changes-on-an-azure-vm"></a>Solución de problemas asociados a los cambios en una máquina virtual de Azure

En este tutorial aprenderá a solucionar problemas de cambios en una máquina virtual de Azure. Si habilita Change Tracking, podrá hacer un seguimiento de los cambios de software, archivos, demonios de Linux, servicios de Windows y claves del Registro de Windows en sus equipos.
Identificar esos cambios de configuración puede ayudarle a localizar problemas operativos de su entorno.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Habilitación de Change Tracking e Inventario para una máquina virtual
> * Buscar servicios detenidos en los registros de cambios
> * Configurar el seguimiento de cambios
> * Habilitar la conexión del registro de actividad
> * Desencadenar un evento
> * Ver los cambios
> * Configurar alertas

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

* Suscripción a Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](./index.yml) para que contenga los runbooks de monitor y de acción, y la tarea de monitor.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para habilitar la característica.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

En primer lugar, debe habilitar Change Tracking e Inventario para este tutorial. Si ya habilitó la característica, este paso no es necesario.

>[!NOTE]
>Si los campos aparecen atenuados, significa que otra característica de Automation está habilitada para la máquina virtual, y debe utilizar la misma área de trabajo y cuenta de Automation.

1. Seleccione **Máquinas virtuales** y elija una máquina virtual de la lista.
2. En el menú de la izquierda, seleccione **Inventario** en **Operaciones**. Se abre la página Inventario.

    ![Habilitación del cambio](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

3. Elija el área de trabajo de [Log Analytics](../azure-monitor/log-query/log-query-overview.md). Esta área de trabajo recopila datos generados por características como Change Tracking e Inventario. El área de trabajo proporciona una única ubicación para revisar y analizar datos desde varios orígenes.

    [!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

4. Seleccione la cuenta de Automation que desea utilizar.

5. Configure la ubicación para la implementación.

5. Haga clic en **Habilitar** para implementar la característica en la máquina virtual. 

Durante la configuración, la máquina virtual se aprovisiona con el agente de Log Analytics para Windows y un [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md). La habilitación de Change Tracking e Inventario puede tardar hasta 15 minutos. Durante este tiempo, no debería cerrar la ventana del explorador.

Después de habilitar la característica, la información sobre el software instalado y los cambios en la máquina virtual se transfiere a los registros de Azure Monitor.
Los datos pueden tardar entre 30 minutos y 6 horas en estar disponibles para el análisis.

## <a name="use-change-tracking-and-inventory-in-azure-monitor-logs"></a>Uso de Change Tracking e Inventario en los registros de Azure Monitor

Change Tracking e Inventario generan datos de registro que se envían a los registros de Azure Monitor. Para buscar los registros mediante la ejecución de consultas, seleccione **Log Analytics** en la parte superior de la página Change Tracking. Los datos de seguimiento de cambios se almacenan bajo el tipo `ConfigurationChange`.

La siguiente consulta de Log Analytics de ejemplo devuelve todos los servicios de Windows que se hayan detenido.

```loganalytics
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Para más información sobre la ejecución y la búsqueda de archivos de registro en los registros de Azure Monitor, consulte el artículo sobre los [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="configure-change-tracking"></a>Configurar el seguimiento de cambios

Con el seguimiento de cambios, puede elegir los archivos y las claves del Registro para recopilar y hacer un seguimiento en **Editar configuración**, en la parte superior de la página Seguimiento de cambios de la máquina virtual. Puede agregar claves del registro de Windows, archivos de Windows o archivos de Linux para realizar el seguimiento en la página Configuración del área de trabajo.

> [!NOTE]
> Tanto el seguimiento de cambios como el inventario usan la misma configuración de recopilación y se configuran en el nivel de área de trabajo.

### <a name="add-a-windows-registry-key"></a>Agregar una clave del Registro de Windows

1. En la pestaña **Registro de Windows**, seleccione **Agregar**. 

1. En la página Agregar registro de Windows a Change Tracking, escriba la información de la clave cuyo seguimiento se va a realizar y haga clic en **Guardar**.

    |Propiedad  |Descripción  |
    |---------|---------|
    |habilitado     | Determina si se aplica la configuración        |
    |Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar        |
    |Grupo     | Un nombre de grupo para agrupar lógicamente los archivos        |
    |Clave del registro de Windows   | La ruta de acceso para buscar el archivo, por ejemplo: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="add-a-windows-file"></a>Agregar un archivo de Windows

1. En la pestaña **Archivos de Windows**, seleccione **Agregar**. 

1. En la página Agregar archivo de Windows para el seguimiento de cambios, escriba la información del archivo o directorio cuyo seguimiento se va a realizar y haga clic en **Guardar**.

    |Propiedad  |Descripción  |
    |---------|---------|
    |habilitado     | Determina si se aplica la configuración        |
    |Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar        |
    |Grupo     | Un nombre de grupo para agrupar lógicamente los archivos        |
    |Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo, por ejemplo: "c:\temp\\\*.txt"<br>También puede usar variables de entorno, como "%winDir%\System32\\\*. *"         |
    |Recursividad     | Determina si se usa recursividad al buscar el elemento cuyo seguimiento se va a realizar.        |
    |Cargar archivo de contenido para toda la configuración| Activa o desactiva la carga de contenido de archivos en los cambios sometidos a seguimiento. Opciones disponibles: **True** o **False**.|

### <a name="add-a-linux-file"></a>Agregar un archivo de Linux

1. En la pestaña **Archivos de Linux**, seleccione **Agregar**. 

1. En la página Agregar archivo de Linux para el seguimiento de cambios, escriba la información del archivo o directorio cuyo seguimiento se va a realizar y haga clic en **Guardar**.

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
    |Cargar archivo de contenido para toda la configuración| Activa o desactiva la carga de contenido de archivos en los cambios sometidos a seguimiento. Opciones disponibles: True o False.|

   > [!NOTE]
   > No se recomienda usar el valor **Administrar** para la propiedad **Vínculos**. No se admite la recuperación de contenido de los archivos.

## <a name="enable-activity-log-connection"></a>Habilitar la conexión del registro de actividad

1. Desde la página Change Tracking en la máquina virtual, seleccione **Administrar conexión de registro de actividad**. 

2. En la página Registro de actividad de Azure, haga clic en **Conectar** para conectar Change Tracking e Inventario al registro de actividad de Azure de su máquina virtual.

3. Vaya a la página Información general de la máquina virtual y seleccione **Detener** para detenerla. 

4. Cuando se le solicite, seleccione **Sí** para detener la máquina virtual. 

5. Después de haber desasignado la máquina virtual, seleccione **Iniciar** para reiniciarla. Al detener e iniciar una máquina virtual, se registrará un evento en su registro de actividad. 

## <a name="view-changes"></a>Ver los cambios

1. Vuelva a la página de seguimiento de cambios y seleccione la pestaña **Eventos** en la parte inferior de la página. 

2. Al cabo de un rato, los eventos de seguimiento de cambios se mostrarán en el gráfico y la tabla. El gráfico muestra los cambios que se han producido con el tiempo. El gráfico de líneas de la parte superior muestra los eventos de Azure Activity Log. Cada una de las filas de los gráficos de barras representa un tipo de cambio diferente al que se puede realizar un seguimiento. Estos tipos son demonios de Linux, archivos, claves del Registro de Windows, software y servicios de Windows. La pestaña de cambios muestra los detalles de los cambios mostrados, con el cambio más reciente en primer lugar.

    ![Visualización de eventos en el portal](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

3. Observe que se ha producido múltiples cambios en el sistema, incluidos cambios en el software y los servicios. Puede usar los filtros en la parte superior de la página para filtrar los resultados por **Tipo de cambio** o por el intervalo de tiempo.

    ![Lista de los cambios en la máquina virtual](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

4. Seleccione un cambio de **WindowsServices**. Esta selección abre la página Cambiar detalles que muestra los detalles del cambio y los valores antes y después del cambio. El servicio de protección de software se detuvo en esta instancia.

    ![Visualización de los detalles de cambio en el portal](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="configure-alerts"></a>Configurar alertas

Ver los cambios en Azure Portal puede ser útil, pero poder recibir una alerta cuando ocurre un cambio, como por ejemplo cuando se detiene un servicio, es más beneficioso. Vamos a agregar una alerta en relación con un servicio detenido. 

1. En Azure Portal, vaya a **Monitor**. 

2. Seleccione **Alertas** en **Servicios compartidos** y haga clic en **Nueva regla de alertas**.

3. Haga clic en **Seleccionar** para elegir un recurso. 

4. En la página Seleccionar un recurso, seleccione **Log Analytics** en el menú desplegable **Filtrar por tipo de recurso**. 

5. Seleccione el área de trabajo de Log Analytics y, a continuación, haga clic en **Listo**.

    ![Selección de un recurso](./media/automation-tutorial-troubleshoot-changes/select-a-resource.png)

6. Haga clic en **Agregar condición**.

7. En la tabla de la página Configurar lógica de señal, seleccione **Búsqueda de registros personalizada**. 

8. Escriba la consulta siguiente en el cuadro de texto de consulta de búsqueda:

    ```loganalytics
    ConfigurationChange | where ConfigChangeType == "WindowsServices" and SvcName == "W3SVC" and SvcState == "Stopped" | summarize by Computer
    ```

    Esta consulta devuelve los equipos que tenían el servicio W3SVC detenido en el tiempo especificado.

9. En **Umbral**, en **Lógica de alerta**, escriba **0**. Cuando haya terminado, haga clic en **Listo**.

    ![Configuración de la lógica de señal](./media/automation-tutorial-troubleshoot-changes/configure-signal-logic.png)

10. Seleccione **Crear nuevo** en **Grupos de acciones**. Un grupo de acciones es un conjunto de acciones que puede usar en varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks y muchas más. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

11. En **Detalles de la alerta**, escriba un nombre y una descripción para la alerta. 

12. Establezca **Gravedad** en **Informativo (gravedad 2)** , **Advertencia (gravedad 1)** o **Crítico (gravedad 0)** .

13. En el cuadro **Nombre del grupo de acción**, escriba un nombre para la alerta y un nombre corto. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

14. En **Acciones**, escriba un nombre para la acción, como **Administradores de correo electrónico**. 

15. En **TIPO DE ACCIÓN**, seleccione **Correo electrónico/SMS/Inserción/Voz**. 

16. En **DETALLES**, seleccione **Editar detalles**.

    ![Adición del grupo de acciones](./media/automation-tutorial-troubleshoot-changes/add-action-group.png)

17. En el panel de Correo electrónico/SMS/Inserción/Voz, escriba un nombre, seleccione la casilla **Correo electrónico** y, a continuación, escriba una dirección de correo electrónico válida. Cuando haya terminado, haga clic en **Aceptar** en el panel y, a continuación, haga clic en **Aceptar** en la página Agregar grupo de acciones.

18. Para personalizar el asunto del correo electrónico de alerta, seleccione **Personalizar acciones**. 

19. En **Crear regla**, seleccione **Asunto de correo electrónico** y, a continuación, elija **Crear regla de alertas**. La regla le indica cuando la implementación de una actualización es correcta y qué máquinas formaron parte de la ejecución de la implementación de actualizaciones. La siguiente imagen es un ejemplo de correo electrónico recibido cuando el servicio W3SVC se detiene.

    ![La captura de pantalla muestra una notificación de correo electrónico recibida cuando el servicio W3SVC se detiene.](./media/automation-tutorial-troubleshoot-changes/email.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Habilitación de Change Tracking e Inventario para una máquina virtual
> * Buscar servicios detenidos en los registros de cambios
> * Configurar el seguimiento de cambios
> * Habilitación de la conexión del registro de actividad
> * Desencadenar un evento
> * Ver los cambios
> * Configurar alertas

Continúe con la información general de la característica Change Tracking e Inventario para más información.

> [!div class="nextstepaction"]
> [Información general de Change Tracking e Inventario](change-tracking/overview.md)
