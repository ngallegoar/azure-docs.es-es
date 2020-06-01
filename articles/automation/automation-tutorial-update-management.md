---
title: Administración de actualizaciones y revisiones para las máquinas virtuales de Azure en Azure Automation
description: En este artículo se indica cómo usar Update Management para administrar actualizaciones y revisiones para las máquinas virtuales de Azure.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: 5b5172df6ed6993742a08d5ac08cf700681dfc6a
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83829161"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Administración de actualizaciones y revisiones para las máquinas virtuales de Azure

En este artículo se describe cómo se puede usar la característica [Update Management](automation-update-management.md) de Azure Automation para administrar las actualizaciones y revisiones para las máquinas virtuales de Azure. 

Para obtener información de precios, consulte [Precios de Automation para Update Management](https://azure.microsoft.com/pricing/details/automation/).

## <a name="prerequisites"></a>Prerrequisitos

* La característica [Update Management](automation-update-management.md) habilitada para una o varias de las máquinas virtuales. 
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md) habilitada para Update Management.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="view-update-assessment"></a>Ver evaluación de la actualización

Cuando habilite Update Management, se abrirá la página Update Management. Si alguna actualización se identifica como que falta, verá una lista de las actualizaciones que faltan en la pestaña **Actualizaciones que faltan**.

En **Vínculo de información**, seleccione el vínculo de la actualización para abrir el artículo de soporte técnico sobre la actualización. Encontrará información importante acerca de la actualización.

![Ver el estado de la actualización](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Haga clic en cualquier otro lugar de la actualización para abrir el panel Búsqueda de registros de la actualización seleccionada. La consulta para la búsqueda de registros está predefinida para esa actualización específica. Puede modificar esta consulta o crear una propia para ver información detallada sobre las actualizaciones implementadas o las actualizaciones que faltan en el entorno.

![Ver el estado de la actualización](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Configurar alertas

En este paso, aprenderá a configurar una alerta, cosa que le permitirá saber el estado de una implementación de actualizaciones.

### <a name="alert-conditions"></a>Condiciones de alerta

En su cuenta de Automation, en **Supervisión**, vaya a **Alertas** y haga clic en **Nueva regla de alertas**.

La cuenta de Automation ya está seleccionada como recurso. Si desea cambiarla, haga clic en **Seleccionar**. En la página Seleccionar un recurso, elija **Cuentas de Automation** en el menú desplegable **Filtrar por tipo de recurso**. Seleccione la cuenta de Automation y haga clic en **Listo**.

Haga clic en **Agregar condición** para seleccionar la señal adecuada para su implementación de actualizaciones. En la tabla siguiente, se muestran los detalles de las dos señales disponibles.

|Nombre de señal|Dimensions|Descripción|
|---|---|---|
|`Total Update Deployment Runs`|- Nombre de implementación de actualizaciones<br>- Estado|Esta señal alerta del estado general de una implementación de actualizaciones.|
|`Total Update Deployment Machine Runs`|- Nombre de implementación de actualizaciones</br>- Estado</br>- Equipo de destino</br>- Identificador de ejecución de implementación de actualizaciones|Esta señal alerta del estado de una implementación de actualizaciones dirigida a máquinas específicas|

En el caso de las dimensiones, seleccione un valor válido de la lista. Si el valor que desea no está en la lista, haga clic en el signo **\+** situado junto a la dimensión y escriba el nombre personalizado. A continuación, seleccione el valor que quiere buscar. Si quiere seleccionar todos los valores de una dimensión, haga clic en el botón **Seleccionar \*** . Si no elige ningún valor para una dimensión, Update Management no tendrá en cuenta esa dimensión.

![Configuración de la lógica de señal](./media/automation-tutorial-update-management/signal-logic.png)

En **Lógica de alerta**, en **Umbral**, escriba **1**. Cuando haya finalizado, seleccione **Listo**.

### <a name="alert-details"></a>Detalles de alertas

En **2. Definir detalles de la alerta**, escriba un nombre y una descripción para la alerta. En **Gravedad**, seleccione **Informativo (gravedad 2)** para ejecuciones correctas, o bien **Informativo (gravedad 1)** para ejecuciones con errores.

![Configuración de la lógica de señal](./media/automation-tutorial-update-management/define-alert-details.png)

En **Grupos de acciones**, seleccione **Crear nuevo**. Un grupo de acciones es un conjunto de acciones que puede usar en varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks, etc. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

En el campo **Nombre del grupo de acciones**, escriba el nombre de la alerta y un nombre abreviado. Update Management usa el nombre abreviado en lugar del nombre completo para enviar notificaciones mediante el grupo especificado.

En **Acciones**, escriba el nombre de la acción, como **Notificaciones por correo electrónico**. En **Tipo de acción**, seleccione **Correo electrónico/SMS/Push/Voz**. En **Detalles**, seleccione **Editar detalles**.

En el panel Correo electrónico/SMS/Push/Voz, escriba un nombre. Seleccione la casilla de verificación **Correo electrónico** y, a continuación, escriba una dirección de correo electrónico válida.

![Configuración de un grupo de acciones de correo electrónico](./media/automation-tutorial-update-management/configure-email-action-group.png)

En el panel Correo electrónico/SMS/Push/Voz, haga clic en **Aceptar**. En el panel Agregar grupo de acciones, haga clic en **Aceptar**.

Para personalizar el asunto del correo electrónico de alerta, en **Crear regla**, en **Personalizar las acciones**, seleccione **Asunto del correo electrónico**. Cuando finalice, seleccione **Crear regla de alertas**. La regla le indica si la implementación de una actualización es correcta y qué máquinas formaron parte de su ejecución.

## <a name="schedule-an-update-deployment"></a>Programación de una implementación de actualizaciones

Programe una implementación que se ajuste a la ventana de programación y servicio de su versión para instalar las actualizaciones. Puede elegir los tipos de actualizaciones que desea incluir en la implementación. Por ejemplo, puede incluir actualizaciones de seguridad o críticas y excluir paquetes acumulativos de actualizaciones.

>[!NOTE]
>Cuando se programa una implementación de actualizaciones, se crea un recurso de [programación](shared-resources/schedules.md) vinculado al runbook **Patch-MicrosoftOMSComputers** que controla la implementación de actualizaciones en las máquinas de destino. Si elimina el recurso de programación desde Azure Portal o mediante PowerShell después de crear la implementación, esta operación de eliminación interrumpe la implementación de actualizaciones programada y, cuando se intenta volver a configurar el recurso de programación desde el portal, aparece un error. Solo se puede eliminar el recurso de programación si se elimina la programación de implementaciones correspondiente.  

Para programar una nueva implementación de actualizaciones para la máquina virtual, vaya a **Update Management** y, a continuación, seleccione **Programar implementación de actualizaciones**.

En **Nueva implementación de actualizaciones**, especifique la siguiente información:

* **Name**: escriba un nombre único para la implementación de actualizaciones.

* **Sistema operativo**: seleccione el sistema operativo que es el destino de la implementación de actualizaciones.

* **Grupos que se deben actualizar (versión preliminar)** : Defina una consulta que combine una suscripción, grupos de recursos, ubicaciones y etiquetas para crear un grupo dinámico de máquinas virtuales de Azure e incluirlo en la implementación. Para más información, consulte [Grupos dinámicos](automation-update-management-groups.md).

* **Máquinas para actualizar**: Seleccione una búsqueda guardada, un grupo importado o la opción **Máquinas** del menú desplegable y elija máquinas específicas. Si elige la opción **Máquinas**, el grado de preparación de cada máquina se indicará en la columna **Preparación de actualizaciones del agente**. Para más información sobre los distintos métodos de creación de grupos de equipos en los registros de Azure Monitor, consulte el artículo sobre los [grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md).

* **Actualizar clasificación**: En cada producto, anule la selección de todas las clasificaciones de actualizaciones compatibles, salvo las que se incluirán en la implementación de actualizaciones. Para ver una descripción de los tipos de clasificación, consulte [Clasificaciones de actualizaciones](automation-view-update-assessments.md#work-with-update-classifications).

* **Actualizaciones para incluir/excluir**: abre la página Incluir/Excluir. Las actualizaciones que se van a incluir o excluir aparecen en pestañas independientes y se especifican mediante el número de identificador del artículo de KB. Si especifica uno o varios números de identificador, debe quitar o desactivar todas las clasificaciones con la implementación de actualizaciones. De ese modo, se asegurará de que no se incluyen otras actualizaciones en el paquete cuando se especifiquen los identificadores de las actualizaciones.

> [!NOTE]
> Es importante saber que las exclusiones tienen prioridad sobre las inclusiones. Por ejemplo, si define una regla de exclusión con `*`, Update Management no instalará ninguna revisión ni ningún paquete, ya que todos quedarán excluidos. Las revisiones excluidas aparecen todavía como que faltan en la máquina. En el caso de las máquinas Linux, si incluye un paquete que tiene un paquete dependiente excluido, Update Management no instalará el paquete principal.

> [!NOTE]
> No puede especificar actualizaciones que se hayan descartado para incluirlas en la implementación de actualizaciones.
>
* **Configuración de la programación**: se abre el panel de configuración de la programación. La hora de inicio predeterminada es 30 minutos después de la hora actual. Puede establecer la hora de inicio en cualquier momento a partir de 10 minutos en el futuro.

   También puede especificar si la implementación se produce una vez o configurar una programación periódica. Seleccione **Una vez** en **Periodicidad**. Deje el valor predeterminado de 1 día y haga clic en **Aceptar**. Estas entradas definen una programación periódica.

* **Scripts previos + scripts posteriores**: seleccione los scripts que se ejecutarán antes y después de la implementación. Para obtener más información, consulte [Administración de scripts previos y posteriores](pre-post-scripts.md).

* **Ventana de mantenimiento (minutos)** : Deje el valor predeterminado. Las de ventanas de mantenimiento controlan la cantidad de tiempo permitido para que se instalen las actualizaciones. Tenga en cuenta los siguientes detalles al especificar una ventana de mantenimiento:

  * Las ventanas de mantenimiento controlan el número de actualizaciones que se instalan.
  * Update Management no detiene la instalación de nuevas actualizaciones si se está aproximando el final de una ventana de mantenimiento.
  * Update Management no finaliza las actualizaciones en curso si se supera la ventana de mantenimiento.
  * Normalmente, si se supera la ventana de mantenimiento de Windows, se debe a que una actualización de Service Pack tarda mucho tiempo en instalarse.

  > [!NOTE]
  > Para evitar que las actualizaciones se apliquen fuera de una ventana de mantenimiento en Ubuntu, vuelva a configurar el paquete de actualizaciones desatendidas para deshabilitar las actualizaciones automáticas. Para más información sobre cómo configurar el paquete, consulte el tema [Actualizaciones automáticas](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) en la Guía de Ubuntu Server.

* **Opciones de reinicio**: se usan para especificar las opciones que controlan los reinicios. Están disponibles las siguientes opciones:
  * Reboot if necessary (default) [Reiniciar si es necesario (valor predeterminado)]
  * Always reboot (Reiniciar siempre)
  * Never reboot (No reiniciar nunca)
  * Only reboot (Solo reiniciar): no se instalarán las actualizaciones

> [!NOTE]
> Las claves del Registro que se muestran en la sección [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) pueden provocar un evento de reinicio si el valor de **Opciones de reinicio** es **No reiniciar nunca**.

Cuando haya terminado de configurar la programación, haga clic en **Crear**.

![Panel de configuración de la programación de actualizaciones](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Volverá al panel de estado. Seleccione **Implementaciones de actualizaciones programadas** para mostrar la programación de implementación que acaba de crear.

> [!NOTE]
> Update Management permite implementar actualizaciones de origen y descargar revisiones con antelación. Para ello, es necesario realizar cambios en los sistemas en los que se van a aplicar revisiones. Para aprender a configurar estos valores en sus sistemas, consulte este artículo sobre la [compatibilidad con actualizaciones de origen y revisiones descargadas previamente](automation-configure-windows-update.md).

También se pueden crear implementaciones de actualizaciones mediante programación. Para aprender a crear una implementación de actualizaciones con la API REST, consulte [Configuraciones de actualizaciones de software: crear](/rest/api/automation/softwareupdateconfigurations/create). También hay un runbook de ejemplo que puede usar para crear una implementación de actualizaciones semanal. Para más información acerca de este runbook, consulte [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Crear una implementación de actualización semanal para una o más VM en un grupo de recursos).

## <a name="view-results-of-an-update-deployment"></a>Visualización de los resultados de una implementación de actualizaciones

Después de que se inicie la implementación programada, puede ver su estado en la pestaña **Implementaciones de actualizaciones** en **Update Management**. Si la implementación se está ejecutando, su estado es **En curso**. Cuando la implementación finaliza correctamente, el estado cambia a **Correcto**. Si se producen errores con una o varias actualizaciones en la implementación, el estado es **Error parcial**.

Seleccione la implementación de actualizaciones completada para ver el panel de dicha implementación.

![Panel de estado de implementación de actualizaciones de la implementación específica](./media/automation-tutorial-update-management/manageupdates-view-results.png)

En **Resultados de actualización** encontrará un resumen del número total de actualizaciones y los resultados de la implementación en la máquina virtual. En la tabla de la derecha se muestra un análisis detallado de cada actualización y los resultados de la instalación.

Los valores disponibles son:

* **No intentado**: la actualización no se instaló porque, según la duración definida en la ventana de mantenimiento, no había tiempo suficiente.
* **Correcto**: actualización realizada correctamente.
* **Error**: error en la actualización.

Seleccione **Todos los registros** para ver todas las entradas de registro creadas por la implementación.

Seleccione **Salida** para ver el flujo de trabajo del runbook responsable de administrar la implementación de actualizaciones en la máquina virtual de destino.

Seleccione **Errores** para ver información detallada acerca de los errores de la implementación.

Cuando la implementación de actualizaciones se realice correctamente, recibirá un correo electrónico de confirmación similar al siguiente:

![Configuración del grupo de acciones de correo electrónico](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Update Management, consulte [Introducción a Update Management](automation-update-management.md).