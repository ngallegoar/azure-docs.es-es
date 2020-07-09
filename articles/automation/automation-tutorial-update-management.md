---
title: Administración de actualizaciones y revisiones para las máquinas virtuales de Azure en Azure Automation
description: En este artículo se indica cómo usar Update Management para administrar actualizaciones y revisiones para las máquinas virtuales de Azure.
services: automation
ms.subservice: update-management
ms.topic: conceptual
ms.date: 04/06/2020
ms.custom: mvc
ms.openlocfilehash: a701a5a9fd77bd801bb535fe1f26bfa17c97757b
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185796"
---
# <a name="manage-updates-and-patches-for-your-azure-vms"></a>Administración de actualizaciones y revisiones para las máquinas virtuales de Azure

En este artículo se describe cómo se puede usar la característica [Update Management](automation-update-management.md) de Azure Automation para administrar las actualizaciones y revisiones para las máquinas virtuales de Azure. Para obtener información de precios, consulte [Precios de Automation para Update Management](https://azure.microsoft.com/pricing/details/automation/).

> [!NOTE]
> Update Management permite implementar actualizaciones de origen y descargar revisiones con antelación. Para ello, es necesario realizar cambios en los sistemas en los que se van a aplicar revisiones. Consulte [Configuración de los parámetros de Windows Update para Azure Automation Update Management](automation-configure-windows-update.md) para aprender a configurar estas opciones en sus sistemas.

Antes de realizar los procedimientos de este artículo, asegúrese de haber habilitado Update Management en las máquinas virtuales mediante una de estas técnicas:

* [Habilitación de Update Management desde una cuenta de Automation](automation-onboard-solutions-from-automation-account.md)
* [Habilitación de Update Management desde Azure Portal](automation-onboard-solutions-from-browse.md)
* [Habilitación de Update Management desde un runbook](automation-onboard-solutions.md)
* [Habilitación de Update Management desde una maquina virtual de Azure](automation-onboard-solutions-from-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Limitación del ámbito de implementación

Update Management usa una configuración de ámbito dentro del área de trabajo para dirigirse a los equipos que tienen que recibir actualizaciones. Para más información, consulte [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).

## <a name="view-update-assessment"></a>Ver evaluación de la actualización

Para ver una evaluación de la actualización:

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**. 

2. Las actualizaciones de su entorno se muestran en la página de Update Management. Si alguna actualización se identifica como que falta, verá una lista de las actualizaciones que faltan en la pestaña **Actualizaciones que faltan**.

3. En **Vínculo información**, seleccione el vínculo de una actualización para abrir el artículo de soporte técnico que le proporciona información importante acerca de la actualización.

    ![Ver el estado de la actualización](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

4. Haga clic en cualquier otro lugar de la actualización para abrir el panel de búsqueda de registros. La consulta para la búsqueda de registros está predefinida para esa actualización específica. Puede modificar esta consulta o crear su propia consulta para ver información detallada.

    ![Ver el estado de la actualización](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerts"></a>Configurar alertas

Realice los pasos siguientes para configurar alertas que le permitan saber el estado de una implementación de actualizaciones:

1. En su cuenta de Automation, en **Supervisión**, vaya a **Alertas** y haga clic en **Nueva regla de alertas**.

2. En la página de creación de reglas de alertas, su cuenta de Automation aparece seleccionada como recurso. Si desea cambiar el recurso, haga clic en **Editar recurso**. 

3. En la página Seleccionar un recurso, elija **Cuentas de Automation** en el menú desplegable **Filtrar por tipo de recurso**. 

4. Seleccione la cuenta de Automation que desea usar y, a continuación, haga clic en **Listo**.

5. Haga clic en **Agregar condición** para seleccionar la señal adecuada para su implementación de actualizaciones. En la tabla siguiente, se muestran los detalles de las dos señales disponibles.

    |Nombre de señal|Dimensions|Descripción
    |---|---|---|
    |`Total Update Deployment Runs`|- Nombre de implementación de actualizaciones<br>- Estado    |Muestra una alerta sobre el estado general de una implementación de actualizaciones.|
    |`Total Update Deployment Machine Runs`|- Nombre de implementación de actualizaciones</br>- Estado</br>- Equipo de destino</br>- Identificador de ejecución de implementación de actualizaciones    |Muestra una alerta sobre el estado de una implementación de actualizaciones dirigida a máquinas específicas.|

6. En el caso de las dimensiones, seleccione un valor válido de la lista. Si el valor que desea no está en la lista, haga clic en el signo **\+** , situado junto a la dimensión y escriba el nombre personalizado. A continuación, seleccione el valor que quiere buscar. Si quiere seleccionar todos los valores de una dimensión, haga clic en el botón **Seleccionar \*** . Si no elige ningún valor para una dimensión, Update Management no tendrá en cuenta esa dimensión.

    ![Configuración de la lógica de señal](./media/automation-tutorial-update-management/signal-logic.png)

7. En **Lógica de alerta**, escriba los valores en los campos **Agregación de tiempo** y **Umbral**, y haga clic en **Listo**.

8. En el siguiente panel, escriba un nombre y una descripción para la alerta.

9. En el campo **Gravedad**, seleccione **Información (gravedad 2)** para una ejecución correcta, o bien **Información (gravedad 1)** para una ejecución con errores.

    ![Configuración de la lógica de señal](./media/automation-tutorial-update-management/define-alert-details.png)

10. Haga clic en **Sí** o en **No**, en función de cómo desee habilitar la regla de alertas.

11. Si no desea recibir alertas para esta regla, seleccione **Suprimir alertas**.

## <a name="configure-action-groups-for-your-alerts"></a>Configuración de grupos de acciones para las alertas

Una vez que haya configurado las alertas, puede configurar un grupo de acciones para usarlas con varias alertas. Por ejemplo, notificaciones por correo electrónico, runbooks, webhooks, etc. Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

1. Seleccione una alerta y, a continuación, **Crear nueva** en **Grupos de acciones**. 

2. Escriba un nombre completo y un nombre corto para el grupo de acciones. Update Management usa el nombre corto en lugar del nombre completo para enviar notificaciones mediante el grupo especificado.

3. En **Acciones**, escriba un nombre para especificar la acción (por ejemplo, **Notificación por correo electrónico**). 

4. En **Tipo de acción**, seleccione el tipo adecuado (por ejemplo, **Correo electrónico, SMS, Push o Voz**). 

5. Haga clic en **Editar detalles**.

6. Escriba el tipo de acción en el panel. Por ejemplo, si usa una acción **Correo electrónico, SMS, Push o Voz**, escriba un nombre de acción, seleccione la casilla **Correo electrónico**, escriba una dirección de correo electrónico válida y, por último, haga clic en **Aceptar**.

    ![Configuración de un grupo de acciones de correo electrónico](./media/automation-tutorial-update-management/configure-email-action-group.png)

7. En el panel Agregar grupo de acciones, haga clic en **Aceptar**.

8. Para un correo electrónico de alerta, puede personalizar el asunto. Seleccione **Personalizar acciones** en **Crear regla** y, a continuación, seleccione **Asunto del correo electrónico**. 

9. Cuando haya finalizado, haga clic en **Crear regla de alertas**. 

## <a name="schedule-an-update-deployment"></a>Programación de una implementación de actualizaciones

Cuando se programa una implementación de actualizaciones, se crea un recurso de [programación](shared-resources/schedules.md) vinculado al runbook **Patch-MicrosoftOMSComputers** que controla la implementación de actualizaciones en las máquinas de destino. Tiene que programar una implementación que se ajuste a la programación y a la ventana de servicio de su versión para instalar las actualizaciones. Puede elegir los tipos de actualizaciones que desea incluir en la implementación. Por ejemplo, puede incluir actualizaciones de seguridad o críticas y excluir paquetes acumulativos de actualizaciones.

>[!NOTE]
>Si elimina el recurso de programación desde Azure Portal o mediante PowerShell después de crear la implementación, esta operación de eliminación interrumpe la implementación de actualizaciones programada y, cuando se intenta volver a configurar el recurso de programación desde el portal, aparece un error. Solo se puede eliminar el recurso de programación si se elimina la programación de implementaciones correspondiente.  

Para programar una implementación de actualizaciones:

1. En la cuenta de Automation, vaya a **Update Management** y, a continuación,**** seleccione **Programar implementación de actualizaciones**.

2. En **Nueva implementación de actualizaciones**, utilice el campo **Nombre** para escribir un nombre único para la implementación.

3. Seleccione el sistema operativo para la implementación de actualizaciones.

4. En el área **Grupos que se deben actualizar (versión preliminar)** , defina una consulta que combine una suscripción, grupos de recursos, ubicaciones y etiquetas con el fin de crear un grupo dinámico de máquinas virtuales de Azure e incluirlo en su implementación. Para más información, consulte [Uso de grupos dinámicos con Update Management](automation-update-management-groups.md).

5. En el área **Máquinas para actualizar**, seleccione una búsqueda guardada, un grupo importado, o bien la opción **Máquinas** del menú desplegable, y elija máquinas específicas. Mediante esta opción, puede ver el grado de preparación del agente de Log Analytics para cada máquina. Para más información sobre los distintos métodos de creación de grupos de equipos en los registros de Azure Monitor, consulte el artículo sobre los [grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md).

6. Use el área **Clasificaciones de actualizaciones** para especificar [clasificaciones de actualizaciones](automation-view-update-assessments.md#work-with-update-classifications) para productos. En cada producto, anule la selección de todas las clasificaciones de actualizaciones compatibles, salvo las que se incluirán en la implementación de actualizaciones.

7. Use el área **Incluir o excluir actualizaciones** para seleccionar actualizaciones específicas con fines de implementación. En la página Incluir o excluir, se muestran las actualizaciones con arreglo a los números de identificador de artículo de KB que se van a incluir o excluir. 
    
   > [!IMPORTANT]
   > Es importante recordar que las exclusiones tienen prioridad sobre las inclusiones. Por ejemplo, si define una regla de exclusión con `*`, Update Management excluirá todas las revisiones o los paquetes de la instalación. Las revisiones excluidas aparecen todavía como que faltan en la máquina. En el caso de las máquinas Linux, si incluye un paquete que tiene un paquete dependiente excluido, Update Management no instalará el paquete principal.

   > [!NOTE]
   > No se pueden especificar actualizaciones reemplazadas para incluirlas en la implementación de actualizaciones.

8. Seleccione **Configuración de programación**. La hora de inicio predeterminada es 30 minutos después de la hora actual. Puede establecer la hora de inicio en cualquier momento a partir de 10 minutos en el futuro.

9. Use el campo **Periodicidad** para especificar si la implementación se producirá una vez o periódicamente y, a continuación, haga clic en **Aceptar**.

10. En el área **Scripts previos + scripts posteriores (versión preliminar)** , seleccione los scripts que se van a ejecutar antes y después de la implementación. Para más información, consulte [Administración de scripts previos y posteriores](pre-post-scripts.md).
    
11. Use el campo **Ventana de mantenimiento (minutos)** con el fin de indicar la cantidad de tiempo permitido para la instalación de actualizaciones. Tenga en cuenta los siguientes detalles al especificar una ventana de mantenimiento:

    * Las ventanas de mantenimiento controlan el número de actualizaciones que se instalan.
    * Update Management no detiene la instalación de nuevas actualizaciones si se está aproximando el final de una ventana de mantenimiento.
    * Update Management no finaliza las actualizaciones en curso si se supera la ventana de mantenimiento.
    * Normalmente, si se supera la ventana de mantenimiento de Windows, se debe a que una actualización de Service Pack tarda mucho tiempo en instalarse.

    > [!NOTE]
    > Para evitar que las actualizaciones se apliquen en Ubuntu fuera de la ventana de mantenimiento, vuelva a configurar el paquete `Unattended-Upgrade` para deshabilitar las actualizaciones automáticas. Para más información sobre cómo configurar el paquete, consulte el tema sobre [actualizaciones automáticas en la guía de Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

12. En el campo **Opciones de reinicio**, especifique cómo abordar los reinicios durante la implementación. Están disponibles las siguientes opciones: 
    * Reboot if necessary (default) [Reiniciar si es necesario (valor predeterminado)]
    * Always reboot (Reiniciar siempre)
    * Never reboot (No reiniciar nunca)
    * Solo reiniciar (no se instalarán actualizaciones)

    > [!NOTE]
    > Las claves del Registro que se muestran en la sección [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) pueden provocar un evento de reinicio si el valor de **Opciones de reinicio** es **No reiniciar nunca**.

13. Cuando haya terminado de configurar la implementación, haga clic en **Crear**.

    ![Panel de configuración de la programación de actualizaciones](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

14. Volverá al panel de estado. Seleccione **Implementaciones de actualizaciones programadas** para mostrar la programación de implementación que ha creado.

## <a name="schedule-an-update-deployment-programmatically"></a>Programación de una implementación de actualizaciones mediante programación

Para aprender a crear una implementación de actualizaciones con la API REST, consulte [Configuraciones de actualizaciones de software: crear](/rest/api/automation/softwareupdateconfigurations/create). 

También puede usar un runbook de ejemplo para crear una implementación semanal de actualizaciones. Para más información acerca de este runbook, consulte [Create a weekly update deployment for one or more VMs in a resource group](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) (Crear una implementación de actualización semanal para una o más VM en un grupo de recursos).

## <a name="check-deployment-status"></a>Comprobar el estado de implementación

Una vez que se ha iniciado la implementación programada, puede ver el estado en la pestaña **Implementaciones de actualizaciones** en **Update Management**. Si la implementación se está ejecutando, su estado es **En curso**. Cuando la implementación finaliza correctamente, el estado cambia a **Correcto**. Si se producen errores con una o varias actualizaciones en la implementación, el estado es **Error parcial**.

## <a name="view-results-of-a-completed-update-deployment"></a>Visualización de los resultados de una implementación de actualizaciones que se ha completado

Una vez finalizada la implementación, puede seleccionarla para consultar el panel.

![Panel de estado de implementación de actualizaciones de la implementación específica](./media/automation-tutorial-update-management/manageupdates-view-results.png)

**Resultados de actualización** ofrece un resumen del número total de actualizaciones y de los resultados de la implementación en las máquinas virtuales de destino. En la tabla de la derecha, se muestra un análisis detallado de las actualizaciones y de los resultados individuales de la instalación.

Los valores disponibles son:

* **No intentado**: la actualización no se instaló porque no se disponía de tiempo suficiente, de acuerdo con la duración especificada para la ventana de mantenimiento.
* **No seleccionado**: la actualización no se seleccionó para su implementación.
* **Correcta**: la actualización se realizó correctamente.
* **Error**: la actualización generó un error.

Seleccione **Todos los registros** para ver todas las entradas de registro creadas por la implementación.

Seleccione **Salida** para ver el flujo de trabajo del runbook que administra la implementación de actualizaciones en las máquinas virtuales de destino.

Seleccione **Errores** para ver información detallada acerca de los errores de la implementación.

## <a name="view-the-deployment-alert"></a>Visualización de los registros de implementación

Una vez finalizada la implementación de actualizaciones, recibirá la alerta que especificó durante la configuración de la implementación. Por ejemplo, este es un correo electrónico de confirmación de una revisión.

![Configuración del grupo de acciones de correo electrónico](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre las configuraciones de ámbito, consulte [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).
* Si necesita buscar en los registros almacenados en el área de trabajo de Log Analytics, consulte [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/log-query-overview.md).
* Si ya ha terminado con las implementaciones, consulte [Desvinculación de un área de trabajo de una cuenta de Automation para Update Management](automation-unlink-workspace-update-management.md).
* Para eliminar máquinas virtuales de Update Management, consulte [Eliminación de una máquina virtual desde Update Management](automation-remove-vms-from-update-management.md).
* Para solucionar los errores de Update Management generales, consulte [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para solucionar problemas con el agente de Windows Update, consulte [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para solucionar problemas relativos al agente de actualización de Linux, consulte [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).
