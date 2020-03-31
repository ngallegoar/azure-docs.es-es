---
title: Seguimiento de cambios con Azure Automation
description: La solución Change Tracking ayuda a identificar los cambios en el software y el servicio de Windows que se producen en su entorno.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/28/2019
ms.topic: conceptual
ms.openlocfilehash: 83babd65fdf22ab40b0137d93a1cbe7f1fd7ff04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844809"
---
# <a name="track-changes-in-your-environment-with-the-change-tracking-solution"></a>Seguimiento de cambios en el entorno con la solución Change Tracking

Este artículo le ayuda a usar la solución Change Tracking para identificar fácilmente los cambios del entorno. La solución realiza un seguimiento de los siguientes cambios de configuración para ayudarle a identificar problemas operativos:

- Software de Windows
- Software de Linux (paquetes)

    >[!NOTE]
    >Change Tracking solo realiza el seguimiento del software que se administra con el administrador de paquetes de la distribución.

- Archivos de Windows y Linux
- Clave del Registro de Windows
- Servicios de Windows
- Demonios de Linux

Los cambios en los demonios de Linux, en el registro y los archivos de Windows, en los servicios de Windows y en el software instalado en los servidores supervisados se envían al servicio de Azure Monitor en la nube para su procesamiento. Se aplica la lógica a los datos recibidos y el servicio de nube registra los datos. Con la información en el panel de seguimiento de cambios, puede ver fácilmente los cambios realizados en la infraestructura de servidores.

> [!NOTE]
> Azure Automation Change Tracking hace el seguimiento de los cambios en las máquinas virtuales. Para hacer el seguimiento de los cambios en la propiedad de Azure Resource Manager, consulte [Historial de cambios](../governance/resource-graph/how-to/get-resource-changes.md) de Azure Resource Graph.

## <a name="supported-windows-operating-systems"></a>Sistemas operativos Windows compatibles

Las siguientes versiones del sistema operativo Windows son compatibles oficialmente con el agente de Windows:

* Windows Server 2008 R2 o versiones posteriores

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles

Las distribuciones Linux siguientes son compatibles oficialmente. Aunque también se puede ejecutar el agente de Linux en otras distribuciones que no se enumeran. A menos que se indique lo contrario, todas las versiones secundarias son compatibles con cada versión principal de la lista.

### <a name="64-bit"></a>64 bits

* CentOS 6 y 7
* Amazon Linux 2017.09
* Oracle Linux 6 y 7
* Red Hat Enterprise Linux Server 6 y 7
* Debian GNU/Linux 8 y 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS y 18.04 LTS
* SUSE Linux Enterprise Server 12

### <a name="32-bit"></a>32 bits

* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 y 9
* Ubuntu Linux 14.04 LTS y 16.04 LTS

## <a name="enable-change-tracking-and-inventory"></a><a name="onboard"></a>Habilitación de Change Tracking e Inventario

Para iniciar el seguimiento de cambios, debe habilitar la solución Change Tracking e Inventory. Hay muchas maneras de incorporar máquinas a la solución Change Tracking e Inventory. Las siguientes son las maneras recomendadas y admitidas para incorporar la solución:

* [Desde una máquina virtual](automation-onboard-solutions-from-vm.md)
* [Navegando desde diferentes máquinas](automation-onboard-solutions-from-browse.md)
* [Desde la cuenta de Automation](automation-onboard-solutions-from-automation-account.md)
* [Con un runbook de Azure Automation](automation-onboard-solutions.md)

## <a name="configuring-change-tracking-and-inventory"></a>Configuración de Change Tracking e Inventory

Para obtener información sobre cómo incorporar equipos a la solución, consulte: [Soluciones de automatización de la incorporación](automation-onboard-solutions-from-automation-account.md). Una vez que tenga la incorporación de una máquina con las soluciones Change Tracking e Inventory puede configurar los elementos a los que se va a realizar un seguimiento. Cuando se habilita un nuevo archivo o una clave del registro para realizar su seguimiento, está habilitado tanto para Change Tracking como para Inventory.

Para realizar el seguimiento de los cambios en los archivos de Windows y Linux, se utilizan los algoritmos hash MD5 de los archivos. Estos algoritmos hash se utilizan para detectar si se ha realizado algún cambio desde el último inventario.

### <a name="file-integrity-monitoring-in-azure-security-center"></a>Supervisar la integridad de los archivos en Azure Security Center

Azure Security Center ha agregado Supervisión de la integridad de los archivos (FIM) integrada en Azure Change Tracking. Mientras que FIM solo supervisa archivos y Registros, la solución Change Tracking completa también incluye:

- Cambios de software
- Servicios de Windows
- Demonios de Linux

Si ya ha habilitado FIM y quiere probar la solución Change Tracking completa, debe seguir los pasos a continuación. Este proceso no quita los valores de configuración.

> [!NOTE]
> Habilitar la solución Change Tracking completa puede generar cargos adicionales, para más información, consulte [Precios de Automation](https://azure.microsoft.com/pricing/details/automation/).

1. Para quitar la solución de supervisión, desplácese hasta el área de trabajo y búsquela en la [Lista de soluciones de supervisión instaladas](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions).
2. Haga clic en el nombre de la solución para abrir su página de resumen y haga clic en Eliminar, según se detalla en [Eliminación de una solución de supervisión](../azure-monitor/insights/solutions.md#remove-a-monitoring-solution).
3. Vuelva a habilitar la solución; para ello, vaya a la cuenta de Automation y seleccione **Change Tracking** en el menú de recursos de **Administración de configuración**.
4. Confirme los detalles de configuración del área de trabajo y haga clic en **Habilitar**.

### <a name="configure-linux-files-to-track"></a>Configuración de los archivos de Linux de los que se realizará un seguimiento

Use los pasos siguientes para configurar el seguimiento de archivos en equipos Linux:

1. En la cuenta de Automation, seleccione **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN**. Haga clic en **Editar configuración** (el símbolo de engranaje).
2. En la página **Change Tracking**, seleccione **Archivos Linux** y, a continuación, haga clic en **+ Agregar** para agregar un nuevo archivo para realizar su seguimiento.
3. En **Agregar archivo de Linux a Change Tracking**, escriba la información del archivo o directorio cuyo seguimiento se va a realizar y haga clic en **Guardar**.

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración.        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar.        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos.        |
|Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo. Por ejemplo: "/etc/*.conf"       |
|Tipo de ruta de acceso     | Tipo de elemento cuyo seguimiento se va a realizar; los valores posibles son Archivo y Directorio.        |
|Recursividad     | Determina si se usa recursividad al buscar el elemento cuyo seguimiento se va a realizar.        |
|Usar sudo     | Esta configuración determina si se va a utilizar sudo al buscar el elemento.         |
|Vínculos     | Esta configuración determina cómo se tratan los vínculos simbólicos cuando se recorren directorios.<br> **Omitir**: ignora los vínculos simbólicos y no incluye los archivos y directorios de referencia.<br>**Seguir**: sigue los vínculos simbólicos durante la recursión y también incluye los archivos y directorios de referencia.<br>**Administrar**: sigue los vínculos simbólicos y permite modificar el contenido devuelto.     |
|Cargar archivo de contenido para toda la configuración| Activa o desactiva la carga de contenido de archivos en los cambios sometidos a seguimiento. Opciones disponibles: **True** o **False**.|

> [!NOTE]
> La opción de administración de vínculos no se recomienda. No se admite la recuperación de contenido de los archivos.

### <a name="configure-windows-files-to-track"></a>Configuración de los archivos de Windows de los que se realizará un seguimiento

Use los pasos siguientes para configurar los archivos de los que se realizará un seguimiento en los equipos Windows:

1. En la cuenta de Automation, seleccione **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN**. Haga clic en **Editar configuración** (el símbolo de engranaje).
2. En la página **Change Tracking**, seleccione **Archivos de Windows** y, a continuación, haga clic en **+ Agregar** para agregar un nuevo archivo para realizar su seguimiento.
3. En **Agregar archivo de Windows a Change Tracking**, escriba la información del archivo cuyo seguimiento se va a realizar y haga clic en **Guardar**.

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración.        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar.        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos.        |
|Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo, por ejemplo: "c:\temp\\\*.txt"<br>También puede usar variables de entorno, como "%winDir%\System32\\\*. *"       |
|Recursividad     | Determina si se usa recursividad al buscar el elemento cuyo seguimiento se va a realizar.        |
|Cargar archivo de contenido para toda la configuración| Activa o desactiva la carga de contenido de archivos en los cambios sometidos a seguimiento. Opciones disponibles: **True** o **False**.|

## <a name="wildcard-recursion-and-environment-settings"></a>Configuración de caracteres comodín, recursividad y entorno

La recursividad le permite especificar caracteres comodín para simplificar el seguimiento entre directorios y variables de entorno para que pueda realizar el seguimiento de los archivos en entornos con nombres de unidad múltiples o dinámicos. En la lista siguiente se muestra información común que debe conocer al configurar la recursividad:

* Los caracteres comodín son necesarios para realizar el seguimiento de varios archivos.
* Si usa caracteres comodín, solo se pueden usar en el último segmento de una ruta de acceso. (como `c:\folder\*file*` o `/etc/*.conf`)
* Si una variable de entorno tiene una ruta de acceso no válida, la validación será correcta pero se producirá un error en dicha ruta cuando se ejecute el inventario.
* Al establecer la ruta de acceso, evite las generales como `c:\*.*`, ya que daría lugar a que se recorrieran demasiadas carpetas.

## <a name="configure-file-content-tracking"></a>Configuración del seguimiento del contenido de los archivos

Puede visualizar el contenido antes y después de un cambio de un archivo con File Content Change Tracking. Está disponible para archivos de Windows y Linux, por cada cambio en el archivo, el contenido del archivo se almacena en una cuenta de almacenamiento, y muestra el archivo de antes y después del cambio, alineado o en paralelo. Para más información, consulte [Visualización del contenido de un archivo del que se está realizando un seguimiento](change-tracking-file-contents.md).

![visualización de los cambios en un archivo](./media/change-tracking-file-contents/view-file-changes.png)

### <a name="configure-windows-registry-keys-to-track"></a>Configuración de las claves del Registro de Windows para realizar un seguimiento

Use los pasos siguientes para configurar las claves del registro para realizar un seguimiento en los equipos Windows:

1. En la cuenta de Automation, seleccione **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN**. Haga clic en **Editar configuración** (el símbolo de engranaje).
2. En la página **Change Tracking**, seleccione **Registro de Windows** y, a continuación, haga clic en **+ Agregar** para agregar una nueva clave del registro para realizar su seguimiento.
3. En **Agregar registro de Windows a Change Tracking**, escriba la información de la clave cuyo seguimiento se va a realizar y haga clic en **Guardar**.

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración.        |
|Nombre del elemento     | Nombre descriptivo de la clave del Registro cuyo seguimiento se va a realizar.        |
|Grupo     | Un nombre de grupo para agrupar lógicamente las claves del Registro.        |
|Clave del registro de Windows   | La ruta de acceso para buscar la clave del Registro. Por ejemplo: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

## <a name="limitations"></a>Limitaciones

Actualmente, la solución Change Tracking no admite los elementos siguientes:

* Recursión para el seguimiento del registro de Windows
* Sistemas de archivos de red
* No se realiza un seguimiento de los distintos métodos de instalación
* No se realiza el seguimiento de los archivos *.exe en Windows

Otras limitaciones:

* Los valores y la columna **Tamaño máximo de archivos** no se usan en la implementación actual.
* Si recopila más de 2500 archivos en el ciclo de recopilación de 30 minutos, el rendimiento podría degradarse.
* Cuando el tráfico de red es elevado, los registros de cambios pueden tardar hasta seis horas en aparecer.
* Si modifica la configuración mientras un equipo está apagado, el equipo podría publicar cambios que pertenecían a la configuración anterior.

## <a name="known-issues"></a>Problemas conocidos

Actualmente, la solución Change Tracking tiene los siguientes problemas:

* Las actualizaciones de revisiones no se recopilan en máquinas Windows Server 2016 Core RS3.
* Los demonios de Linux pueden mostrar un estado modificado incluso si no ha habido cambios. Esto se debe a cómo se captura el campo `SvcRunLevels`.

## <a name="change-tracking-data-collection-details"></a>Detalles de la recopilación de datos de seguimiento de cambios

En la tabla siguiente se muestra la frecuencia de recopilación de datos para los tipos de cambios. Para cada tipo, la instantánea del estado actual de los datos también se actualiza al menos cada 24 horas:

| **Tipo de cambio** | **Frecuencia** |
| --- | --- |
| Registro de Windows | 50 minutos |
| Archivo de Windows | 30 minutos |
| Archivo de Linux | 15 minutos |
| Servicios de Windows | 10 segundos hasta 30 minutos</br> Valor predeterminado: 30 minutos |
| Demonios de Linux | 5 minutos |
| Software de Windows | 30 minutos |
| Software Linux | 5 minutos |

La siguiente tabla muestra los límites del elemento sometido a seguimiento por máquina para Change Tracking.

| **Recurso** | **Límite**| **Notas** |
|---|---|---|
|Archivo|500||
|Registro|250||
|Software de Windows|250|No se incluyen las revisiones de software.|
|Paquetes Linux|1250||
|Servicios|250||
|Demonio|250||

El uso medio de datos de Log Analytics para una máquina con Change Tracking e inventario es aproximadamente de 40MB al mes. Este valor es solo una aproximación y está sujeto a cambios en función de su entorno. Se recomienda que supervise el entorno para ver el uso exacto que tiene.

### <a name="windows-service-tracking"></a>Seguimiento del servicio de Windows

La frecuencia de recopilación predeterminado para los servicios de Windows es de 30 minutos. Para configurar la frecuencia, vaya a **Change Tracking**. En **Editar configuración** en la pestaña **Servicios de Windows**, hay un control deslizante que le permite cambiar la frecuencia de recopilación para los servicios de Windows desde el valor menor de 10 segundos al mayor de 30 minutos. Mueva el control deslizante hasta la frecuencia que desee y el valor se guardará automáticamente.

![Control deslizante de servicios de Windows](./media/change-tracking/windowservices.png)

El agente solo realiza un seguimiento de los cambios, esto optimiza su rendimiento. Al establecer un umbral alto, se pueden perder los cambios si el servicio se revierte a su estado original. Si la frecuencia se establece en un valor menor, es posible detectar los cambios que de otra forma podrían pasar desapercibidos.

> [!NOTE]
> Aunque el agente puede seguir los cambios hasta en intervalos de 10 segundos, los datos todavía tardan unos minutos en mostrarse en el portal. Durante el tiempo que lleva la visualización en el portal, se continúa realizando el seguimiento y registro de los cambios.

### <a name="registry-key-change-tracking"></a>Seguimiento de cambios en las claves del Registro

El objetivo de supervisar los cambios en las claves del Registro identificar los puntos de extensibilidad en los que se puede activar código de terceros y malware. En la lista siguiente se muestra la lista de claves del registro configuradas previamente. Estas claves están configuradas pero no habilitadas. Para realizar el seguimiento de estas claves del registro, debe habilitar cada una de ellas.

> [!div class="mx-tdBreakAll"]
> |Clave del Registro | Propósito |
> |---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Supervisa las entradas de inicio automático comunes que se enlazan directamente en el Explorador de Windows y suele ejecutar In-Process con Explorer.exe.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup` | Supervisa los scripts que se ejecutan al arrancar.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown` | Supervisa los scripts que se ejecutan al apagar el equipo.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run` | Supervisa las claves que se cargan antes de que el usuario inicie sesión en su cuenta de Windows. La clave se usa para programas de 32 bits que se ejecutan en equipos de 64 bits.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components` | Supervisa los cambios en la configuración de la aplicación.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers` | Supervisa las entradas de inicio automático comunes que se enlazan directamente en el Explorador de Windows y suele ejecutar In-Process con Explorer.exe.
> |`HKEY\LOCAL\MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers` | Supervisa las entradas de inicio automático comunes que se enlazan directamente en el Explorador de Windows y suele ejecutar In-Process con Explorer.exe.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Supervisa el registro del controlador de superposición de iconos.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers` | Supervisa el registro del controlador de superposición de iconos para programas de 32 bits que se ejecutan en equipos de 64 bits.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Supervisa si hay nuevos complementos de objetos auxiliares de explorador para Internet Explorer. Se utiliza para acceder a Document Object Model (DOM) de la página actual y controlar la navegación.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects` | Supervisa si hay nuevos complementos de objetos auxiliares de explorador para Internet Explorer. Sirve para acceder a Document Object Model de la página actual y controlar la navegación de programas de 32 bits que se ejecutan en equipos de 64 bits.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Internet Explorer\Extensions` | Supervisa si hay nuevas extensiones de Internet Explorer, como menús de la herramienta personalizada y botones de la barra de herramientas personalizada.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions` | Supervisa si hay nuevas extensiones de Internet Explorer, como menús de la herramienta personalizada y botones de la barra de herramientas personalizada para programas de 32 bits que se ejecutan en equipos de 64 bits.
> |`HKEY\LOCAL\MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32` | Supervisa los controladores de 32 bits asociados a wavemapper, wave1 y wave2, msacm.imaadpcm, .msadpcm, .msgsm610 y vidc. Es similar a la sección [drivers] del archivo SYSTEM.INI.
> |`HKEY\LOCAL\MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32` | Supervisa los controladores de 32 bits asociados a wavemapper, wave1 y wave2, msacm.imaadpcm, .msadpcm, .msgsm610 y vidc para programas de 32 bits que se ejecutan en equipos de 64 bits. Es similar a la sección [drivers] del archivo SYSTEM.INI.
> |`HKEY\LOCAL\MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls` | Supervisa la lista de DLL del sistema conocidas o utilizadas; este sistema impide que las personas se aprovechen de permisos de directorio de aplicación débiles colocando versiones de troyanos en DLL del sistema.
> |`HKEY\LOCAL\MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify` | Supervisa la lista de paquetes que pueden recibir notificaciones de eventos de Winlogon, el modelo de compatibilidad de inicio de sesión interactivo para el sistema operativo Windows.

## <a name="network-requirements"></a>Requisitos de red

Las direcciones siguientes se requieren específicamente para Change Tracking. La comunicación con estas direcciones se realiza a través del puerto 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*.azure-automation.us|

## <a name="use-change-tracking"></a>Uso de seguimiento de cambios

Después de habilitar la solución, puede ver el resumen de los cambios en los equipos supervisados seleccionando **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN** en la cuenta de Automation.

Puede ver los cambios en los equipos y, a continuación, profundizar en los detalles de cada evento. Hay disponibles listas desplegables en la parte superior del gráfico para limitar el gráfico y la información detallada en función del tipo de cambio y los intervalos de tiempo. También puede hacer clic y arrastrar en el gráfico para seleccionar un intervalo de tiempo personalizado. El **tipo de cambio** será uno de estos valores: **Eventos**, **Demonios**, **Archivos**, **Registro**, **Software**, **Servicios de Windows**. En la categoría se muestra el tipo de cambio y se pueden **Agregar**, **Modificar** o **Quitar**.

![imagen del panel de seguimiento de cambios](./media/change-tracking/change-tracking-dash01.png)

Al hacer clic en un cambio o un evento, se abrirá la información detallada sobre dicho cambio. Como puede ver en el ejemplo, el tipo de inicio del servicio cambió de Manual a Automático.

![imagen de los detalles del seguimiento de cambios](./media/change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Búsqueda de registros

Además de los detalles que se proporcionan en el portal, se puede buscar en los registros. Con la página **Change Tracking** abierta, haga clic en **Log Analytics** y se abrirá la página **Registros**.

### <a name="sample-queries"></a>Consultas de ejemplo

En la tabla siguiente se proporcionan ejemplos de búsquedas de registros para los registros de cambios recopilados por esta solución:

|Consultar  |Descripción  |
|---------|---------|
|ConfigurationData<br>&#124; where   ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"<br>&#124; where SvcState == "Stopped"<br>&#124; summarize arg_max(TimeGenerated, *) by SoftwareName, Computer         | Muestra los registros de inventario más recientes para los servicios de Windows que se establecieron en Automático, pero se han notificado como Detenidos<br>Los resultados se limitan a la entrada más reciente para esos valores de SoftwareName y Computer      |
|ConfigurationChange<br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Removed"<br>&#124; order by TimeGenerated desc|Muestra los registros de cambios para el software eliminado|

## <a name="alert-on-changes"></a>Alerta sobre los cambios

Una funcionalidad clave de Change Tracking y del inventario es la posibilidad de recibir alertas sobre el estado de configuración y de todos los cambios del estado de configuración de su entorno híbrido.

En el ejemplo siguiente, la captura de pantalla muestra que el archivo `C:\windows\system32\drivers\etc\hosts` se ha modificado en una máquina. Este archivo es importante porque Windows utiliza el archivo de hosts para resolver los nombres de host en direcciones IP y prevalece incluso sobre DNS, que podría provocar problemas de conectividad o el redireccionamiento del tráfico a sitios web malintencionados o peligrosos.

![Un gráfico en el que se muestra el cambio del archivo de hosts](./media/change-tracking/changes.png)

Para analizar aún más este cambio, haga clic en **Log Analytics** para acceder a la búsqueda de registros. En la búsqueda de registros, busque los cambios de contenido del archivo de hosts con la consulta `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Esta consulta busca los cambios que incluyen un cambio del contenido en los archivos cuya ruta de acceso absoluta contiene la palabra “hosts”. También puede solicitar un archivo específico si cambia la parte de la ruta de acceso a su forma absoluta (como `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`).

Una vez que la consulta devuelve los resultados deseados, haga clic en el botón **Nueva regla de alertas** en la experiencia de búsqueda de registros para abrir la página de creación de alertas. También puede acceder a esta experiencia mediante **Azure Monitor** en Azure Portal. En la experiencia de creación de la alerta, vuelva a consultar nuestra consulta y modifique la lógica de alerta. En este caso, desea que la alerta se desencadene incluso aunque se detecte un solo cambio en todas las máquinas del entorno.

![Una imagen en la que se muestra la consulta de cambio para realizar un seguimiento de los cambios en el archivo de hosts](./media/change-tracking/change-query.png)

Después de establecer la lógica de la condición, asigne grupos de acciones para realizar acciones como respuesta a la alerta que se va a desencadenar. En este caso, he configurado correos electrónicos para enviarlos y un vale de ITSM para crearlo.  También se pueden realizar muchas otras acciones útiles, como desencadenar una función de Azure, un runbook de Automation, un webhook o una aplicación lógica.

![Una imagen en la que se configura un grupo de acciones para enviar alertas sobre el cambio](./media/change-tracking/action-groups.png)

Una vez configurados todos los parámetros y la lógica, la alerta se puede aplicar al entorno.

### <a name="alert-suggestions"></a>Sugerencias de alertas

Si bien la creación de alertas sobre los cambios en el archivo de hosts es una aplicación conveniente de las alertas para los datos de inventario o Change Tracking, existen muchos más escenarios para las alertas, entre otros, los casos definidos junto con las consultas de ejemplo en la sección siguiente.

|Consultar  |Descripción  |
|---------|---------|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Files" and FileSystemPath contains " c:\\windows\\system32\\drivers\\"|Útil para el seguimiento de cambios en archivos críticos del sistema.|
|ConfigurationChange <br>&#124; where FieldsChanged contains "FileContentChecksum" and FileSystemPath == "c:\\windows\\system32\\drivers\\etc\\hosts"|Útil para el seguimiento de las modificaciones de los archivos de configuración de claves.|
|ConfigurationChange <br>&#124; where ConfigChangeType == "WindowsServices" and SvcName contains "w3svc" and SvcState == "Stopped"|Útil para el seguimiento de cambios en servicios críticos del sistema.|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Daemons" and SvcName contains "ssh" and SvcState != "Running"|Útil para el seguimiento de cambios en servicios críticos del sistema.|
|ConfigurationChange <br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Added"|Útil para entornos que necesitan configuraciones de software bloqueadas.|
|ConfigurationData <br>&#124; where SoftwareName contains "Monitoring Agent" and CurrentVersion != "8.0.11081.0"|Útil para ver qué máquinas tienen instalada una versión de software obsoleta o incompatible. Notifica el último estado de configuración notificado, no los cambios.|
|ConfigurationChange <br>&#124; where RegistryKey == @"HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\QualityCompat"| Útil para el seguimiento de los cambios en claves de antivirus fundamentales.|
|ConfigurationChange <br>&#124; where RegistryKey contains @"HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\SharedAccess\\Parameters\\FirewallPolicy"| Útil para el seguimiento de los cambios en la configuración del firewall.|

## <a name="next-steps"></a>Pasos siguientes

Consulte el tutorial de Change Tracking para más información acerca del uso de la solución:

> [!div class="nextstepaction"]
> [Solución de problemas de cambios en el entorno](automation-tutorial-troubleshoot-changes.md)

* Consulte [Búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md) para ver datos detallados sobre el seguimiento de cambios.
