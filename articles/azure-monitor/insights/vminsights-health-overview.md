---
title: Estado de invitado de Azure Monitor para VM (versión preliminar)
description: Información general de la característica de estado en Azure Monitor para VM, incluido cómo puede ver el estado de las máquinas virtuales y recibir alertas cuando una máquina virtual pasa a un estado incorrecto.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/27/2020
ms.openlocfilehash: 96bed9f3b04e54e2e9a5234d78f9a2660126742e
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686577"
---
# <a name="azure-monitor-for-vms-guest-health-preview"></a>Estado de invitado de Azure Monitor para VM (versión preliminar)
El estado de invitado de Azure Monitor para VM permite ver el estado de una máquina virtual según un conjunto de medidas de rendimiento que se muestrean a intervalos regulares desde el sistema operativo invitado. Puede comprobar rápidamente el estado de todas las máquinas virtuales de una suscripción o un grupo de recursos, explorar en profundidad el estado detallado de una máquina virtual concreta o incluso recibir una notificación de manera proactiva cuando una máquina virtual pasa a un estado incorrecto. 

## <a name="enable-virtual-machine-health"></a>Habilitación del estado de una máquina virtual
Consulte [Habilitación del estado de invitado de Azure Monitor para VM (versión preliminar)](vminsights-health-enable.md) para obtener más información sobre cómo habilitar la característica del estado de invitado y la incorporación de máquinas virtuales.

## <a name="pricing"></a>Precios
La característica de estado de invitado no tiene ningún costo directo, pero la ingesta y el almacenamiento de datos del estado de mantenimiento en el área de trabajo Log Analytics sí tiene un costo. Todos los datos se almacenan en la tabla *HealthStateChangeEvent*. Consulte [Administración del uso y los costos con los registros de Azure Monitor](../platform/manage-cost-storage.md) para más información sobre costos y modelos de precios.

## <a name="view-virtual-machine-health"></a>Visualización del estado de la máquina virtual
La columna **Estado de la VM invitada** de la página **Introducción** ofrece una vista rápida del estado de cada máquina virtual en una suscripción o un grupo de recursos determinado. Se muestra el estado actual de cada máquina virtual, mientras que los iconos de cada grupo muestran el número de máquinas virtuales que se encuentran actualmente en cada estado de ese grupo.

[![Página de introducción con el estado de la máquina virtual invitada](media/vminsights-health-overview/get-started-page.png)](media/vminsights-health-overview/get-started-page.png#lightbox)


## <a name="monitors"></a>Monitores
Haga clic en el estado de mantenimiento de una máquina virtual para ver el estado detallado de cada uno de sus monitores. Cada monitor mide el estado de un componente específico. El estado general de la máquina virtual viene determinado por el estado de sus monitores correspondientes. 

![Ejemplo de monitores](media/vminsights-health-overview/monitors.png)

En la tabla siguiente se enumeran los monitores agregados y de unidad disponibles actualmente para cada máquina virtual. 

| Monitor | Tipo | Descripción |
|:---|:---|:---|
| Uso de CPU | Unidad | Porcentaje de uso del procesador. |
| Sistemas de archivos | Agregado | Estado agregado de todos los sistemas de archivos en una VM Linux. |
| Sistema de archivos  | Agregado | Estado de cada sistema de archivos individual en una VM Linux. El nombre del monitor es el nombre del sistema de archivos. |
| Espacio libre | Unidad | Porcentaje de espacio disponible en el sistema de archivos. |
| Discos lógicos | Agregado | Estado agregado de todos los discos lógicos en una VM Windows. |
| Disco lógico  | Agregado | Estado de cada disco lógico individual en una VM Windows. El nombre del monitor es el nombre del disco. |
| Memoria | Agregado | Estado agregado de la memoria en la VM. |
| Memoria disponible | Unidad | Megabytes disponibles en la VM. |

## <a name="health-states"></a>Estados de mantenimiento
Cada monitor muestrea los valores del agente cada minuto y compara los valores muestreados con los criterios para cada estado de mantenimiento. Si los criterios para un estado específico son true, el monitor se establece en ese estado. Si ninguno de los criterios es true, el monitor se establece en un estado correcto. Los datos se envían desde el agente a Azure Monitor cada tres minutos o inmediatamente si se produce un cambio de estado.

Cada monitor tiene una ventana de retrospectiva y analiza las muestras recopiladas en ese momento. Por ejemplo, un monitor puede tener una ventana de retrospectiva de 240 segundos para buscar el valor máximo entre al menos dos valores muestreados, pero no más de los tres últimos. Aunque los valores se muestrean periódicamente, el número muestreado en una ventana concreta puede variar ligeramente si hay alguna interrupción en la operación del agente.

Cada monitor tiene los posibles estados de mantenimiento en la tabla siguiente y solo se establecerá en uno en todo momento. Cuando se inicializa un monitor, se inicia en un estado correcto.

| Estado de mantenimiento | Descripción |
|:---|:---|
| Healthy  | El monitor no supera actualmente el umbral crítico o de advertencia. |
| Advertencia  | El monitor superó el umbral de advertencia (si está definido). |
| Crítico | El monitor superó el umbral crítico (si está definido). |
| Desconocido  | No se han recopilado datos suficientes para determinar el estado de mantenimiento. |
| Disabled | El monitor está deshabilitado actualmente. |
| None     | El monitor se acaba de iniciar y aún no se ha evaluado, o el objeto supervisado ya no existe. |



Hay dos tipos de monitores, tal como se muestra en la siguiente tabla.

| Supervisión | Descripción |
|:---|:---|
| Monitor de unidad | Mide algún aspecto de un recurso o una aplicación. Esto podría estar comprobando un contador de rendimiento para determinar el rendimiento del recurso o su disponibilidad. |
| Monitor agregado | Agrupa varios monitores para proporcionar un estado de mantenimiento agregado único. Un monitor agregado puede contener uno o varios monitores de unidad, así como otros monitores agregados. |


  
### <a name="health-rollup-policy"></a>Directiva de acumulación de estado
El estado de mantenimiento de una máquina virtual viene determinado por la acumulación de estados de cada uno de sus monitores de unidad y de agregado. Cada monitor agregado usa una directiva de acumulación de peor estado de mantenimiento posible, en la que el estado del monitor agregado coincide con el estado del monitor secundario con el peor estado de mantenimiento.  

En el ejemplo siguiente, el monitor de **Espacio disponible** está en un estado crítico, que se transfiere a los monitores agregados de su instancia y, a continuación, a los **sistemas de archivos**. Aunque el **Uso de CPU** se encuentra en un estado de advertencia, la máquina virtual se establece en el estado crítico, ya que este es el peor estado. Si el espacio disponible tuviera volviera a un estado correcto, la máquina virtual cambiaría a un estado de advertencia, ya que el uso de CPU se convertiría en el monitor con el peor estado.

![Ejemplo de la acumulación del mantenimiento](media/vminsights-health-overview/health-rollup-example.png)


## <a name="monitor-details"></a>Detalles del monitor
Seleccione un monitor para ver sus detalles, incluidas las pestañas siguientes.

**Información general** proporciona una descripción del monitor, la última vez que se evaluó y los valores muestreados para determinar el estado de mantenimiento actual. El número de muestras puede variar en función de la definición del monitor y la volatilidad de los valores.

[![Información general de los detalles de monitor](media/vminsights-health-overview/monitor-details-overview.png)](media/vminsights-health-overview/monitor-details-overview.png#lightbox)

**Historial** muestra el historial de cambios de estado del monitor. Puede expandir cualquiera de los cambios de estado para ver los valores que se evaluaron para determinar el estado de mantenimiento. Haga clic en **Ver configuración aplicada** para ver la configuración del monitor en el momento en que se cambió el estado de mantenimiento.



[![Historial de detalles de monitor](media/vminsights-health-overview/monitor-details-history.png)](media/vminsights-health-overview/monitor-details-history.png#lightbox)

### <a name="configuration"></a>Configuración
Vea y modifique la configuración del monitor de la máquina virtual seleccionada. Consulte [Configuración de la supervisión en el estado de invitado de Azure Monitor para VM (versión preliminar)](vminsights-health-enable.md) para más información.

[![Configuración de la información del monitor](media/vminsights-health-overview/monitor-details-configuration.png)](media/vminsights-health-overview/monitor-details-configuration.png#lightbox)




## <a name="next-steps"></a>Pasos siguientes

- [Habilitar el estado de invitado en Azure Monitor para VM e incorporar a los agentes.](vminsights-health-enable.md)
- [Configurar los monitores mediante Azure Portal.](vminsights-health-configure.md)
- [Configurar monitores mediante reglas de recopilación de datos.](vminsights-health-configure-dcr.md)