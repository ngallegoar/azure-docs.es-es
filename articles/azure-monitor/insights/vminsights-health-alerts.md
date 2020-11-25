---
title: Alertas del estado de invitado de Azure Monitor para VM (versión preliminar)
description: Describe las alertas creadas por el estado del invitado de Azure Monitor para VM, incluido cómo habilitarlas y configurar las notificaciones.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/10/2020
ms.openlocfilehash: 9bbb0be321e78317c52586454de3d49cb2f878c1
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686536"
---
# <a name="azure-monitor-for-vms-guest-health-alerts-preview"></a>Alertas del estado de invitado de Azure Monitor para VM (versión preliminar)
El estado de invitado de Azure Monitor para VM permite ver el estado de una máquina virtual conforme a la definición de un conjunto de medidas de rendimiento que se muestrean a intervalos regulares. Se puede crear una alerta cuando una máquina virtual o un monitor cambia a un estado incorrecto. Puede ver y administrar estas alertas con [las que han creado las reglas de alertas en Azure Monitor](../platform/alerts-overview.md) y optar por recibir notificaciones proactivas cuando se crea una nueva alerta.

## <a name="configure-alerts"></a>Configurar alertas
No se puede crear una regla de alerta explícita para el estado de invitado de Azure Monitor para VM mientras esta característica está en versión preliminar. De manera predeterminada, se crearán alertas para cada máquina virtual, pero no para cada monitor.  Esto significa que si un monitor cambia a un estado que no afecta al estado actual de la máquina virtual, no se crea ninguna alerta porque el estado de la máquina virtual no ha cambiado. 

Puede deshabilitar las alertas de una máquina virtual determinada o de un monitor determinado en una máquina virtual desde la opción **Estado de alerta** en la configuración de la máquina virtual en Azure Portal. Consulte [Configuración de la supervisión en el estado de invitado de Azure Monitor para VM (versión preliminar)](vminsights-health-configure.md) para obtener detalles sobre la configuración de monitores en Azure Portal. Consulte [Configuración del estado de invitado de Azure Monitor para VM mediante las reglas de recopilación de datos (versión preliminar)](vminsights-health-configure-dcr.md) para obtener más información sobre la configuración de monitores en un conjunto de máquinas virtuales.

## <a name="alert-severity"></a>Gravedad de la alerta
La gravedad de la alerta que creó el estado del invitado se asigna directamente a la gravedad de la máquina virtual o monitor que desencadena la alerta.

| Estado del monitor | Gravedad de la alerta |
|:---|:---|
| Crítico | Sev1 |
| Advertencia  | Sev2 |
| Healthy  | Sev4 |

## <a name="alert-lifecycle"></a>Ciclo de vida de alertas
Se creará una [alerta de Azure](../platform/alerts-overview.md) para una máquina virtual siempre que cambie a un estado de **advertencia** o **crítico**. Consulte la alerta desde **Alertas** en el menú **Azure Monitor** o en el menú de máquinas virtuales en Azure Portal.

Si una alerta ya se encuentra en el estado **Desencadenada** cuando cambie el estado de la máquina virtual, no se creará una segunda alerta, sino que se cambiará la gravedad de la misma alerta para que coincida con el estado de la máquina virtual. Por ejemplo, si la máquina virtual cambia al estado **Crítico** cuando una alerta de **Advertencia** ya se encontraba en el estado **Desencadenada**, la gravedad de la alerta se cambiará a **Sev1**. Si la máquina virtual cambia al estado **Advertencia** cuando una alerta de **Sev1** ya se encontraba en el estado **Desencadenada**, la gravedad de la alerta se cambiará a **Sev2**. Si la máquina virtual vuelve a un estado **Correcto**, la alerta se resolverá con la gravedad cambiada a **Sev4**.

## <a name="viewing-alerts"></a>Visualización de alertas
Vea las alertas creadas por el estado de invitado de Azure Monitor para VM con otras [alertas en Azure Portal](../platform/alerts-overview.md#alerts-experience). Puede seleccionar **Alertas** en el menú **Azure Monitor** para ver las alertas de todos los recursos supervisados, o bien seleccionar **Alertas** en el menú de la máquina virtual para ver las alertas solo de esa máquina virtual.

## <a name="alert-properties"></a>Propiedades de alerta

### <a name="properties-in-the-azure-portal"></a>Propiedades en Azure Portal
Las propiedades de la alerta al verla en Azure Portal se describen en la tabla siguiente.

| Propiedad | Descripción |
|:---|:---|
| Estado del monitor antes de que se creara la alerta | Estado del monitor o de la máquina virtual antes de que esta alerta se desencadenara por primera vez. |
| Estado del monitor cuando se creó la alerta | Estado del monitor o de la máquina virtual cuando esta alerta se desencadenó por primera vez. Este es el estado que hizo que se desencadenara la alerta. |
| Para saber más sobre la transición de estado cuando se creó la alerta | Vínculo a la página de estado de la máquina virtual en el que puede ver la transición de estado exacta. Esta transición de estado representa a la instancia la primera vez que el monitor pasó del estado **correcto** a un estado incorrecto. |

Por ejemplo, un monitor pasa de un estado **Correcto** a **Crítico** a las t0 y se desencadena una nueva alerta con **Sev1**. Después, va del estado **Crítico** a **Advertencia** a las t1 y la gravedad de la alerta se actualiza a **Sev2**. Se pasa al estado **Correcto** a las t2 y se resuelve la alerta.

Las propiedades de la alerta tendrán estos valores durante toda la secuencia.

- Estado del monitor antes de que se creara la alerta: Healthy
- Estado del monitor cuando se creó la alerta: Crítico
- Para saber más sobre la transición de estado cuando se creó la alerta: Vínculo de navegación a la transición de estado que sucedió a las t0.


### <a name="properties-in-notifications"></a>Propiedades en notificaciones
En la tabla siguiente se describen las propiedades de la alerta incluida en las notificaciones.

| Propiedad | Descripción |
|:---|:---|
| Estado anterior del monitor | Estado del monitor o de la máquina virtual antes de cambiar de estado. Si la actualización de la gravedad de la alerta desencadena esta notificación, esta propiedad representa el estado inmediatamente anterior a la actualización de la gravedad. |
| Estado actual del monitor | Estado del monitor o de la máquina virtual cuando cambió de estado. Si la actualización de la gravedad de la alerta desencadena esta notificación, esta propiedad representa el nuevo estado. |
| Para obtener más información sobre esta transición de estado | Vínculo a la página de estado de la máquina virtual en el que puede ver la transición de estado exacta. Esta transición de estado representa a la instancia cuando el monitor cambió el estado que desencadenó esta notificación. |

Con el ejemplo anterior, las notificaciones tendrían las siguientes propiedades en cada ocasión.

Notificación recibida a las t0
- Estado anterior del monitor: Healthy
- Estado actual del monitor: Crítico
- Para obtener más información sobre esta transición de estado: Vínculo de navegación a la transición de estado que sucedió a las t0.

Notificación recibida a las t1
- Estado anterior del monitor: Crítico
- Estado actual del monitor: Advertencia
- Para obtener más información sobre esta transición de estado: Vínculo de navegación a la transición de estado que sucedió a las t1.

Notificación recibida a las t2
- Estado anterior del monitor: Advertencia
- Estado actual del monitor: Healthy
- Para obtener más información sobre esta transición de estado: Vínculo de navegación a la transición de estado que sucedió a las t2.

## <a name="configure-notifications"></a>Configuración de notificaciones
Para que se le notifique proactivamente de una alerta desencadenada por el estado de invitado, cree un [grupo de acciones](../platform/action-groups.md) para definir las distintas acciones que se deben llevar a cabo, como enviar un mensaje SMS o iniciar una aplicación lógica. A continuación, cree una [regla de acción](../platform/alerts-action-rules.md) que especifique el ámbito de los monitores y las máquinas virtuales, y use ese grupo de acciones.

En el menú **Monitor** de Azure Portal, seleccione **Alertas**.  Seleccione **Administrar acciones** y, a continuación, haga clic en **Reglas de acción (versión preliminar)** . 

![Nueva regla de acción](media/vminsights-health-alerts/action-rule-new.png)

Haga clic en **Nueva regla de acción** para crear una nueva regla. Haga clic en **Seleccionar** junto a ámbito y seleccione una suscripción, un grupo de recursos, o una o varias máquinas virtuales específicas. La notificación solo se desencadenará para las máquinas virtuales que se encuentren dentro del ámbito.

![Ámbito de la regla de acción](media/vminsights-health-alerts/action-rule-scope.png)

Haga clic en **Agregar** junto a **Filtro**. Cree un filtro en el que **el servicio de monitor sea Información de máquinas virtuales: estado**. Agregue otros filtros para especificar las alertas concretas que deben desencadenar la notificación. Por ejemplo, puede usar **Gravedad** para usar a las alertas de todos los monitores que coincidan con una gravedad determinada.

![Filtros de reglas de acción](media/vminsights-health-alerts/action-rule-filter.png)

En **Definir en este ámbito**, seleccione **Grupo de acciones** y, a continuación, seleccione el grupo de acciones que quiere asociar con el monitor. Asigne un nombre a la regla y seleccione el grupo de recursos en el que se debe guardar. Haga clic en **Crear** para crear la regla.

![Regla de acción](media/vminsights-health-alerts/action-rule.png)


## <a name="next-steps"></a>Pasos siguientes

- [Habilitar el estado de invitado en Azure Monitor para VM e incorporar a los agentes.](vminsights-health-enable.md)
- [Configurar los monitores mediante Azure Portal.](vminsights-health-configure.md)
- [Configurar monitores mediante reglas de recopilación de datos.](vminsights-health-configure-dcr.md)