---
title: Administración de la recopilación del inventario de Azure Automation desde máquinas virtuales | Microsoft Docs
description: En este artículo se explica cómo administrar la recopilación del inventario desde máquinas virtuales.
services: automation
ms.subservice: change-inventory-management
keywords: inventario, automatización, cambio, seguimiento
ms.date: 01/28/2020
ms.topic: conceptual
ms.openlocfilehash: d237b016b8f3430ed0b28becd2712bd0c41d17b4
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830623"
---
# <a name="manage-inventory-collection-from-vms"></a>Administración de la recopilación del inventario desde máquinas virtuales

Puede habilitar el seguimiento del inventario para una máquina virtual de Azure desde la página de recursos de esta. Puede recopilar y ver la siguiente información de inventario en los equipos:

- Actualizaciones de Windows, aplicaciones de Windows, servicios, archivos y claves del Registro
- Paquetes de software, demonios y archivos de Linux

Change Tracking e Inventario de Azure Automation ofrece una interfaz de usuario basada en explorador para instalar y configurar la recopilación de inventario.

## <a name="before-you-begin"></a>Antes de empezar

Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/).

En este artículo se presupone que tiene una máquina virtual que habilitar con Change Tracking e Inventario. Si aún no tiene una máquina virtual de Azure, puede [crear una máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-vm-resource-page"></a>Habilitación de la recopilación de inventario desde la página de recursos de la máquina virtual

1. En el panel izquierdo de Azure Portal, seleccione **Máquinas virtuales**.
2. En la lista de máquinas virtuales, seleccione una máquina.
3. En el menú **Recurso**, en **Operaciones**, seleccione **Inventario**.
4. Seleccione un área de trabajo de Log Analytics para almacenar los registros de datos.
    Si no hay áreas de trabajo disponibles para esa región, deberá crear una predeterminada y una cuenta de Automation.
5. Para empezar a habilitar el equipo, seleccione **Habilitar**.

   ![Vista de las opciones de incorporación](./media/automation-vm-inventory/inventory-onboarding-options.png)

    Una barra de estado le notifica que se está habilitando la característica Change Tracking e Inventario. Este proceso puede tardar hasta 15 minutos. Durante este tiempo, puede cerrar la ventana o mantenerla abierta, y se le notificará cuando la característica esté habilitada. Puede supervisar el estado de la implementación desde el panel de notificaciones.

   ![Visualización del inventario](./media/automation-vm-inventory/inventory-onboarded.png)

Una vez completada la implementación, la barra de estado desaparece. El sistema sigue recopilando datos de inventario y puede que estos aún no se vean. La recopilación de datos puede tardar 24 horas.

## <a name="configure-your-inventory-settings"></a>Configuración del inventario

De forma predeterminada, el software, los servicios de Windows y los demonios de Linux están configurados para la recopilación. Para recopilar el registro de Windows y el inventario de archivos, configure las opciones de recopilación de inventario.

1. En la página Inventario, haga clic en **Editar configuración** en la parte superior de la página.
2. Para agregar una nueva configuración de recopilación, vaya a la categoría de configuración que desee agregar seleccionando la pestaña **Registro de Windows**, **Archivos de Windows** o **Archivos de Linux**.
3. Seleccione la categoría correspondiente y haga clic en **Agregar** en la parte superior de la página.

En las secciones siguientes se ofrece información sobre cada propiedad que se puede configurar para las distintas categorías.

### <a name="windows-registry"></a>Registro de Windows

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | Determina si se aplica la configuración        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos        |
|Clave del registro de Windows   | La ruta de acceso para buscar el archivo, por ejemplo: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="windows-files"></a>Archivos de Windows

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | True si se aplica la configuración y False en caso contrario.        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar.        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos.       |
|Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo, por ejemplo: **c:\temp\miarchivo.txt**.

### <a name="linux-files"></a>Archivos de Linux

|Propiedad  |Descripción  |
|---------|---------|
|habilitado     | True si se aplica la configuración y False en caso contrario.        |
|Nombre del elemento     | Nombre descriptivo del archivo cuyo seguimiento se va a realizar.        |
|Grupo     | Un nombre de grupo para agrupar lógicamente los archivos.        |
|Escriba la ruta de acceso     | La ruta de acceso para buscar el archivo, por ejemplo: **/etc/*.conf**.       |
|Tipo de ruta de acceso     | Tipo de elemento cuyo seguimiento se va a realizar. Los valores son Archivo y Directorio.        |
|Recursividad     | True si se usa recursividad al buscar el elemento cuyo seguimiento se va a realizar y False en caso contrario.        |
|Usar sudo     | True si se usa sudo al buscar el elemento y False en caso contrario.         |
|Vínculos     | Valor que indica cómo se tratan los vínculos simbólicos cuando se recorren directorios. Los valores posibles son: <br> Omitir: Ignora los vínculos simbólicos y no incluye los archivos y directorios de referencia.<br>Seguir: Sigue los vínculos simbólicos durante la recursión y también incluye los archivos y directorios de referencia.<br>Administrar: Sigue los vínculos simbólicos y permite modificar el tratamiento del contenido devuelto.      |

## <a name="manage-machine-groups"></a>Administración de grupos de máquinas

El inventario le permite crear y ver grupos de máquinas de los registros de Azure Monitor. Los grupos de máquinas son colecciones de máquinas definidas por una consulta en los registros de Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Para ver los grupos de máquinas, seleccione la pestaña **Grupos de máquinas** en la página Inventario.

![Visualización de grupos de máquinas en la página Inventario](./media/automation-vm-inventory/inventory-machine-groups.png)

Si selecciona un grupo de máquinas de la lista se abrirá la página Grupos de máquinas. Esta página muestra detalles sobre el grupo de máquinas. Estos detalles incluyen la consulta de Log Analytics que se usa para definir el grupo. En la parte inferior de la página, hay una lista paginada de las máquinas que forman parte de ese grupo.

![Ver página Grupo de máquinas](./media/automation-vm-inventory/machine-group-page.png)

Haga clic en **+ Clonar** para clonar el grupo de máquinas. Debe asignar al grupo un nuevo nombre y un alias. La definición se puede modificar en este momento. Después de cambiar la consulta, haga clic en **Validar consulta** para obtener una vista previa de las máquinas que se pueden seleccionar. Cuando esté satisfecho con el grupo, haga clic en **Crear** para crear el grupo de máquinas.

Si quiere crear un nuevo grupo de máquinas, haga clic en **+ Crear un grupo de máquinas**. Este botón abre la página **Crear un grupo de máquinas** en la que puede definir el nuevo grupo. Haga clic en **Crear** para crear el grupo.

![Crear grupo de máquinas](./media/automation-vm-inventory/create-new-group.png)

## <a name="disconnect-your-vm-from-management"></a>Desconexión de la máquina virtual de la administración

Para quitar la máquina virtual de la administración de Change Tracking e Inventario:

1. En el panel izquierdo de Azure Portal, seleccione **Log Analytics** y elija el área de trabajo que usó al habilitar la máquina virtual para Change Tracking e Inventario.
2. En la página Log Analytics, abra el menú **Recurso**.
3. Seleccione **Máquinas virtuales** en **Orígenes de datos del área de trabajo**.
4. En la lista, seleccione la máquina virtual que quiere desconectar. La máquina tiene una marca de verificación verde junto a **Esta área de trabajo** en la columna **Conexión a OMS**.

   >[!NOTE]
   >Operations Management Suite (OMS) ahora se conoce como registros de Azure Monitor.
   
5. En la parte superior de la página siguiente, haga clic en **Desconectar**.
6. En la ventana de confirmación, haga clic en **Sí** para desconectar la máquina de la administración.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre cómo trabajar con la característica, consulte [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para más información sobre el seguimiento de cambios de software, consulte [Información general de Change Tracking e Inventario](../log-analytics/log-analytics-change-tracking.md).
* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).