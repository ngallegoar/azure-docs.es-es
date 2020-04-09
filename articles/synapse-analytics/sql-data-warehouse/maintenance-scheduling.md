---
title: Programaciones de mantenimiento para el grupo de SQL de Synapse
description: La programación de mantenimiento permite a los clientes planear los eventos de mantenimiento programado necesarios que Azure Synapse Analytics usa para implementar nuevas características, actualizaciones y revisiones.
services: synapse-analytics
author: antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/02/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 43fc32e910c51e8b70e15aa49584a18e5b703fca
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631608"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Uso de programaciones de mantenimiento para administrar las actualizaciones del servicio y el mantenimiento

La característica de programación de mantenimiento integra las notificaciones de mantenimiento planeado de Service Health, el monitor de comprobación de Resource Health y el servicio de programación de mantenimiento del grupo de SQL de Synapse (almacenamiento de datos) dentro de Azure Synapse Analytics.

Debe usar programación de mantenimiento para elegir un período de tiempo en el que sea conveniente recibir las nuevas características, actualizaciones y revisiones. Tendrá que elegir una ventana de mantenimiento principal y una secundaria dentro de un período de siete días, cada ventana debe estar dentro de intervalos de días independientes.

Por ejemplo, puede programar una ventana principal de sábado a las 22:00 a domingo a la 01:00, y luego una ventana secundaria el miércoles de 19:00 a 22:00. Si no se puede realizar el mantenimiento durante la ventana de mantenimiento principal, se intentará nuevamente durante la ventana de mantenimiento secundaria. El mantenimiento del servicio puede producirse en ocasiones durante las dos ventanas, la principal y la secundaria. Para garantizar una finalización rápida de las operaciones de mantenimiento, DW400c y los niveles más bajos de almacenamiento de datos pueden completar el mantenimiento fuera de una ventana de mantenimiento designada.

Todas las instancias de almacenamiento de datos recién creadas tendrán una programación de mantenimiento definida por el sistema aplicada durante la implementación. La programación se podrá editar tan pronto como se complete la implementación.

Aunque una ventana de mantenimiento puede estar comprendida entre tres y ocho horas, eso no significa que el almacenamiento de datos esté sin conexión durante ese tiempo. El mantenimiento puede producirse en cualquier momento dentro de la ventana y se debe esperar una sola desconexión durante ese período que dure unos 5-6 minutos mientras el servicio implementa nuevo código en el almacenamiento de datos. Puede que DW400c y los niveles más bajos experimenten varias pérdidas de conectividad breves en distintos momentos durante la ventana de mantenimiento. Cuando se inicie el mantenimiento, se cancelarán todas las sesiones activas y se revertirán las transacciones no confirmadas. Para minimizar el tiempo de inactividad de la instancia, debe asegurarse de que no hay transacciones de larga ejecución en el almacenamiento de datos antes del comienzo del período de mantenimiento seleccionado.

Todas las operaciones de mantenimiento deben finalizar dentro de las ventanas de mantenimiento especificadas, a menos que sea necesario implementar una actualización urgente de las horas. Si el almacenamiento de datos se pausa durante un período de mantenimiento programado, se actualizará durante la operación de reanudación. Se le notificará inmediatamente después de que se haya completado el mantenimiento en el almacenamiento de datos.

## <a name="alerts-and-monitoring"></a>Supervisión y alertas

Una integración con las notificaciones de Service Health y el monitor de comprobación de Resource Health permite a los clientes mantenerse informados de la actividad de mantenimiento inminente. Esta automatización aprovecha las ventajas de Azure Monitor. Puede decidir cómo recibir notificaciones de eventos de mantenimiento inminentes. Asimismo, puede decidir qué flujos automatizados pueden ayudarle a administrar el tiempo de inactividad y minimizar el impacto en sus operaciones.

A todos los eventos de mantenimiento, con la excepción de DWC400c y los niveles más bajos, les precede una notificación con 24 horas de adelanto.

> [!NOTE]
> En caso de que sea necesario implementar una actualización donde el tiempo es fundamental, los tiempos de las notificaciones avanzadas pueden reducirse significativamente.

Si recibió una notificación anticipada relativa a la realización de mantenimiento, pero el mantenimiento no puede realizarse durante el período de la notificación, recibirá una notificación de cancelación. Después se reanudará el mantenimiento durante el siguiente período de mantenimiento programado.

Todos los eventos de mantenimiento activos se muestran en la sección **Service Health: mantenimiento planeado**. El historial de Service Health incluye un registro completo de eventos anteriores. Puede supervisar el mantenimiento mediante el panel del portal de comprobación de Azure Service Health durante un evento activo.

### <a name="maintenance-schedule-availability"></a>Disponibilidad de la programación de mantenimiento

Aunque Programación de mantenimiento no esté aún disponible en la región seleccionada, puede ver y editar la programación de mantenimiento en cualquier momento. Cuando la programación de mantenimiento esté disponible en su región, la programación identificada pasará a estar activa inmediatamente en el grupo de SQL de Synapse.

## <a name="view-a-maintenance-schedule"></a>Vista de una programación de mantenimiento

De forma predeterminada, todas las instancias de almacenamiento de datos recién creadas tienen una ventana de mantenimiento principal y secundaria de 8 horas aplicada durante la implementación. Como se ha indicado anteriormente, puede cambiar las ventanas tan pronto como se complete la implementación. Sin notificación previa, no se llevará a cabo mantenimiento fuera de las ventanas de mantenimiento especificadas.

Para ver la programación de mantenimiento que se ha aplicado al grupo de SQL de Synapse, complete los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione el grupo de SQL de Synapse que desea ver.
3. El grupo de SQL de Synapse seleccionado se abre en la hoja de información general. La programación de mantenimiento que se aplica al almacén de datos se mostrará debajo de **Programación de mantenimiento**.

![Hoja Información general](./media/maintenance-scheduling/clear-overview-blade.PNG)

## <a name="change-a-maintenance-schedule"></a>Cambiar una programación de mantenimiento

Una programación de mantenimiento puede actualizarse o cambiarse en cualquier momento. Si la instancia seleccionada está en un ciclo de mantenimiento activo, se guardará la configuración. Se activará durante el siguiente período de mantenimiento identificado. [Obtenga más información](../../service-health/resource-health-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) sobre cómo supervisar el almacenamiento de datos durante un evento de mantenimiento activo.

## <a name="identifying-the-primary-and-secondary-windows"></a>Identificar las ventanas principales y secundarias

Las ventanas principales y secundarias deben tener intervalos de días independientes. Un ejemplo es una ventana principal de martes a jueves, y otra secundaria de sábado a domingo.

Para cambiar la programación de mantenimiento del grupo de SQL de Synapse, complete los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione el grupo de SQL de Synapse que desea actualizar. La página se abre en la hoja de información general.
Abra la página de configuración de la programación de mantenimiento seleccionando el vínculo de **resumen de la Programación de mantenimiento** en la hoja de información general. O bien, seleccione la opción **Programación de mantenimiento** en el menú de recursos del lado izquierdo.

    ![Opciones de la hoja Información general](./media/maintenance-scheduling/maintenance-change-option.png)

3. Puede identificar el intervalo de día preferido para la ventana de mantenimiento principal con las opciones situadas en la parte superior de la página. Esta selección determina si la ventana principal se producirá en un día laborable o durante el fin de semana. La selección actualizará los siguientes valores de la lista desplegable.
Durante la versión preliminar es posible que algunas regiones no admitan el conjunto completo de opciones de **día** disponibles.

   ![Hoja Configuración de mantenimiento](./media/maintenance-scheduling/maintenance-settings-page.png)

4. Elija las ventanas de mantenimiento principales y secundarias que prefiera con el cuadro de lista desplegable:
   - **Día**: día preferido para realizar el mantenimiento durante el período seleccionado.
   - **Hora de inicio**: hora de inicio preferida de la ventana de mantenimiento.
   - **Período de tiempo**: duración preferida del período de tiempo.

   El área **Resumen de la programación** de la parte inferior de la hoja se actualiza según los valores que seleccione.
  
5. Seleccione **Guardar**. Aparece un mensaje que confirma que la nueva programación está activa.

   Si va a guardar una programación en una región que aún no admite la programación de mantenimiento, aparecerá el siguiente mensaje. La configuración se guarda y se activa cuando la característica esté disponible en la región seleccionada.

   ![Mensaje sobre la disponibilidad de la región](./media/maintenance-scheduling/maintenance-not-active-toast.png)

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información](../../azure-monitor/platform/alerts-metric.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) sobre cómo crear, ver y administrar alertas mediante Azure Monitor.
- [Obtenga más información](../..//azure-monitor/platform/alerts-log-webhook.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) sobre las acciones del webhook para las reglas de alerta de registro.
- [Obtenga más información](../..//azure-monitor/platform/action-groups.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) sobre la creación y administración de grupos de acciones.
- [Obtenga más información](../../service-health/service-health-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) acerca de Azure Service Health.
