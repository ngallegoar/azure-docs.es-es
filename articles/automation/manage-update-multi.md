---
title: Administración de actualizaciones para varias máquinas virtuales de Azure
description: En este artículo se describe cómo administrar actualizaciones para máquinas virtuales de Azure y que no son de Azure.
services: automation
ms.subservice: update-management
ms.date: 03/26/2020
ms.topic: conceptual
ms.openlocfilehash: c9a3c88ea0c3e656adf0f8c514b418cfc07c9590
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335766"
---
# <a name="manage-updates-for-multiple-machines"></a>Administración de actualizaciones para varias máquinas

Puede usar la solución Update Management para administrar las actualizaciones y las revisiones de las máquinas Windows y Linux. Desde la cuenta de [Azure Automation](automation-offering-get-started.md), puede:

- Incorporar máquinas virtuales
- Evaluar el estado de las actualizaciones disponibles
- Programar la instalación de las actualizaciones necesarias
- Revisar los resultados de la implementación para comprobar que se han aplicado correctamente actualizaciones a todas las máquinas virtuales para las cuales se ha habilitado Update Management

## <a name="prerequisites"></a>Prerrequisitos

Para usar Update Management, necesita lo siguiente:

- Una máquina virtual o un equipo con uno de los sistemas operativos admitidos instalado.

- Acceso a un repositorio de actualizaciones para máquinas virtuales Linux incorporadas a la solución.

Para más información sobre los requisitos del sistema de Update Management, vea [Requisitos del cliente de Update Management](automation-update-management.md#clients).

## <a name="enable-update-management-for-azure-virtual-machines"></a>Habilitación de Update Management en máquinas virtuales de Azure

En Azure Portal, abra su cuenta de Automation y seleccione **Update Management**.

Seleccione **Agregar máquina virtual de Azure**.

![Pestaña Agregar máquina virtual de Azure](./media/manage-update-multi/update-onboard-vm.png)

Seleccione la máquina virtual que desea incorporar.

En **Habilitar la administración de actualizaciones**, seleccione **Habilitar** para incorporar la máquina virtual.

![Cuadro de diálogo Habilitar la administración de actualizaciones](./media/manage-update-multi/update-enable.png)

Una vez que se completa la incorporación, Update Management está habilitado para su máquina virtual.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Habilitación de Update Management en equipos y máquinas virtuales que no son de Azure

El agente de Log Analytics para Windows y Linux debe instalarse en las máquinas virtuales que se ejecutan en la red corporativa o en otro entorno en la nube con el fin de habilitarlos con Update Management. Para obtener información sobre los requisitos del sistema y los métodos admitidos a fin de implementar el agente en máquinas hospedadas fuera de Azure, vea [Información general sobre el agente de Log Analytics](../azure-monitor/platform/log-analytics-agent.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Visualización de equipos conectados a la cuenta de Automation

Después de habilitar Update Management en las máquinas, puede ver información sobre ellas si selecciona **Equipos**. Encontrará información sobre el *nombre de la máquina*, el *estado de cumplimiento*, el *entorno*, el *tipo de sistema operativo*, las *actualizaciones críticas y de seguridad instaladas*, *otras actualizaciones instaladas* y la *preparación del agente de actualización* de los equipos.

  ![Pestaña en la que se ven los equipos](./media/manage-update-multi/update-computers-tab.png)

Es posible que los equipos que no se hayan habilitado recientemente para Update Management no se hayan evaluado aún. En este caso, el estado de cumplimiento de esos equipos es **No evaluado**. Esta es una lista de valores posibles de estado de cumplimiento:

- **Compatible**: equipos que no tienen actualizaciones críticas o de seguridad pendientes.

- **No compatible**: equipos que no tienen al menos una actualización crítica o de seguridad pendiente.

- **No evaluado**: los datos de valoración de las actualizaciones no se han recibido del equipo en el período esperado. En equipos Linux, el período de tiempo esperado está en la última hora. En equipos Windows, el período de tiempo esperado está en las 12 últimas horas.

Para ver el estado del agente, seleccione el vínculo de la columna **Preparación de actualizaciones del agente**. Al seleccionar esta opción se abre el panel **Hybrid Worker**, que muestra el estado de Hybrid Worker. En la siguiente imagen se muestra un ejemplo de un agente que no se ha conectado a Update Management durante un período de tiempo prolongado:

![Pestaña en la que se ven los equipos](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Visualización de la evaluación de la actualización

Una vez habilitado Update Management, se abre el panel **Update Management**. Puede ver una lista de las actualizaciones que faltan en la pestaña **Actualizaciones que faltan**.

## <a name="collect-data"></a>Recopilación de datos

Los agentes que están instalados en máquinas virtuales y equipos recopilan datos acerca de las actualizaciones. Los agentes envían los datos a Azure Update Management.

### <a name="supported-agents"></a>Agentes admitidos

En la tabla siguiente se describen los orígenes conectados compatibles con esta solución:

| Origen conectado | Compatible | Descripción |
| --- | --- | --- |
| Agentes de Windows |Sí |Update Management recopila información sobre las actualizaciones del sistema de los agentes de Windows e inicia la instalación de las actualizaciones necesarias. |
| Agentes de Linux |Sí |Update Management recopila información sobre las actualizaciones del sistema de los agentes de Linux e inicia la instalación de las actualizaciones necesarias en las distribuciones admitidas. |
| Grupo de administración de Operations Manager |Sí |Update Management recopila información sobre las actualizaciones del sistema de los agentes de un grupo de administración conectado. |
| Cuenta de Azure Storage |No |Azure Storage no incluye información sobre las actualizaciones del sistema. |

### <a name="collection-frequency"></a>Frecuencia de recopilación

Después de que un equipo completa un examen de cumplimiento de las actualizaciones, el agente reenvía la información de forma masiva a los registros de Azure Monitor. En un equipo Windows, el examen de cumplimiento se ejecuta cada 12 horas de forma predeterminada.

Además del examen programado, el examen de cumplimiento de las actualizaciones se inicia a los 15 minutos del reinicio de MMA, así como antes y después de la instalación de actualizaciones.

En un equipo Linux, el examen de cumplimiento se realiza cada hora de manera predeterminada. Si se reinicia el agente MMA, se inicia un examen de cumplimiento al cabo de 15 minutos.

Puede que transcurran entre 30 minutos y 6 horas antes de que se muestren los datos actualizados de los equipos administrados.

## <a name="schedule-an-update-deployment"></a>Programación de una implementación de actualizaciones

Para instalar actualizaciones, programe una implementación que esté en consonancia con su ventana de programación y servicio de versiones. Puede elegir los tipos de actualizaciones que quiere incluir en la implementación. Por ejemplo, puede incluir actualizaciones de seguridad o críticas y excluir paquetes acumulativos de actualizaciones.

>[!NOTE]
>Cuando se programa una implementación de actualizaciones, se crea un recurso de [programación](shared-resources/schedules.md) vinculado al runbook **Patch-MicrosoftOMSComputers** que controla la implementación de actualizaciones en las máquinas de destino. Si elimina el recurso de programación desde Azure Portal o mediante PowerShell después de crear la implementación, se interrumpe la implementación de actualizaciones programada y aparece un error cuando intenta volver a configurarlo desde el portal. Solo se puede eliminar el recurso de programación si se elimina la programación de implementaciones correspondiente.
>

Para programar una nueva implementación de actualizaciones de una o varias máquinas virtuales, en **Update Management**, seleccione **Programar implementación de actualizaciones**.

En el panel **Nueva implementación de actualización**, especifique la siguiente información:

- **Name**: proporcione un nombre único para identificar la implementación de actualizaciones.
- **Sistema operativo**: seleccione **Windows** o **Linux**.
- **Grupos que se deben actualizar**: Defina una consulta basada en una combinación de suscripción, grupos de recursos, ubicaciones y etiquetas para crear un grupo dinámico de VM de Azure e incluirlo en la implementación. En el caso de las máquinas virtuales que no son de Azure, las búsquedas guardadas se usan para crear un grupo dinámico que se incluirá en la implementación. Para obtener más información, vea [Grupos dinámicos](automation-update-management-groups.md).
- **Máquinas para actualizar**: seleccione una Búsqueda guardada, Grupo importado o Máquinas para elegir las máquinas que desea actualizar.

   >[!NOTE]
   >Al seleccionar la opción de búsqueda guardada no se devuelven las identidades de la máquina, solo sus nombres. Si tiene varias máquinas virtuales con el mismo nombre en varios grupos de recursos, estas se devuelven en los resultados. Se recomienda usar la opción **Grupos que se deben actualizar** para garantizar que se incluyen las máquinas virtuales únicas que cumplen los criterios.

   Si elige **Máquinas**, la preparación de la máquina se muestra en la columna **Preparación del agente de actualización**. Puede ver el estado de mantenimiento de la máquina antes de programar la implementación de actualizaciones. Para información sobre los distintos métodos de creación de grupos de equipos en los registros de Azure Monitor, consulte el artículo sobre los [Grupos de equipos en los registros de Azure Monitor](../azure-monitor/platform/computer-groups.md)

  ![Panel Nueva implementación de actualización](./media/manage-update-multi/update-select-computers.png)

- **Actualizar clasificación**: seleccione los tipos de software que se incluirán en la implementación de actualizaciones. Para ver una descripción de los tipos de clasificación, consulte [Actualización de clasificaciones](automation-view-update-assessments.md#update-classifications). Los tipos de clasificación son:
  - Actualizaciones críticas
  - Actualizaciones de seguridad
  - Paquetes acumulativos de actualizaciones
  - Feature Packs
  - Service Packs
  - Actualizaciones de definiciones
  - Herramientas
  - Actualizaciones

- **Actualizaciones para incluir/excluir**: abre la página para **incluir/excluir**. Las actualizaciones que se incluirán o excluirán están en pestañas independientes. Para más información sobre cómo se controla la inclusión, consulte [Programación de una implementación de actualizaciones](automation-tutorial-update-management.md#schedule-an-update-deployment).

> [!NOTE]
> Es importante saber que las exclusiones invalidan las inclusiones. Por ejemplo, si define una regla de exclusión de `*`, no se instalan revisiones ni paquetes, ya que se excluyen todas. Las revisiones excluidas aparecen todavía como que faltan en la máquina. Para las máquinas Linux, si se incluye un paquete, pero tiene un paquete dependiente que se ha excluido, el paquete no se instala.

> [!NOTE]
> No puede especificar actualizaciones que se hayan sustituido para incluirlas en la implementación de actualizaciones.
>

- **Configuración de la programación**: puede aceptar la fecha y hora predeterminadas, que son 30 minutos después de la hora actual. También puede especificar una hora distinta.

   También puede especificar si la implementación se produce una vez o configurar una programación periódica. Para configurar una programación periódica, en **Periodicidad**, seleccione **Periódica**.

   ![Cuadro de diálogo Configuración de programación](./media/manage-update-multi/update-set-schedule.png)

- **Scripts previos + scripts posteriores**: seleccione los scripts que se ejecutarán antes y después de la implementación. Para obtener más información, consulte [Administración de scripts previos y posteriores](pre-post-scripts.md).
- **Ventana de mantenimiento (minutos)** : especifique el período de tiempo en el que desea que se produzca la implementación de actualizaciones. Esta configuración ayuda a garantizar que los cambios se realizan en las ventanas de servicio definidas.

- **Reboot control**: esta configuración determina cómo se controlan los reinicios para la implementación de actualizaciones.

   |Opción|Descripción|
   |---|---|
   |Reiniciar si es necesario| **(Valor predeterminado)** En caso necesario, se inicia un reinicio cuando la ventana de mantenimiento lo permite.|
   |Always reboot (Reiniciar siempre)|Se inicia un reinicio, independientemente de si se necesita. |
   |Never reboot (No reiniciar nunca)|Independientemente de que se requiera un reinicio, los reinicios se suprimen.|
   |Only reboot (solo reiniciar), no se instalarán las actualizaciones|Esta opción omite la instalación de actualizaciones y solo inicia un reinicio.|

Cuando haya terminado de configurar la programación, seleccione el botón **Crear** para volver al panel de estado. En la tabla **Programada** se muestra la programación de implementaciones que ha creado.

> [!NOTE]
> Update Management admite la implementación de actualizaciones de origen y la descarga previa de revisiones. Esto requiere cambios en los sistemas a los que se aplican revisiones. Para aprender a configurar estos valores en sus sistemas, consulte [soporte técnico para aplicación de revisiones propias y para descargas previas](automation-configure-windows-update.md#pre-download-updates).

## <a name="view-results-of-an-update-deployment"></a>Visualización de los resultados de una implementación de actualizaciones

Después de que se inicie la implementación programada, puede ver su estado en la pestaña **Implementaciones de actualizaciones** en **Update Management**.

Si la implementación se está ejecutando, su estado es **En curso**. Cuando la implementación finaliza correctamente, el estado cambia a **Correcto**.

Si se produce un error en una o varias actualizaciones de la implementación, el estado es **Error parcial**.

![Estado de la implementación de actualizaciones](./media/manage-update-multi/update-view-results.png)

Para establecer el panel para una implementación de actualizaciones, seleccione la implementación completada.

En el panel **Resultados de actualización** se muestra el número total de actualizaciones y los resultados de la implementación de la máquina virtual. En la tabla de la derecha se muestra un análisis detallado de cada actualización y los resultados de la instalación. Los resultados de la instalación puede ser uno de los siguientes valores:

- **No intentado**: la actualización no se instaló porque no había tiempo disponible suficiente de acuerdo con la ventana de mantenimiento definida.
- **Correcto**: actualización realizada correctamente.
- **Error**: error en la actualización.

Seleccione **Todos los registros** para ver todas las entradas de registro que creó la implementación.

Para ver el flujo de trabajo del runbook que administra la implementación de actualizaciones en la máquina virtual de destino, seleccione el icono de salida.

Seleccione **Errores** para ver información detallada sobre los errores de la implementación.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los registros, salidas y errores de Update Management, vea [Consulta de registros de actualización para Update Management](automation-update-management-query-logs.md).
