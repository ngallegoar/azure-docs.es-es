---
title: Configuración de la supervisión en el estado de invitado de Azure Monitor para VM mediante reglas de recopilación de datos (versión preliminar)
description: Describe cómo modificar la supervisión predeterminada en el estado de invitado de Azure Monitor para VM a escala mediante las plantillas de Resource Manager.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/15/2020
ms.openlocfilehash: 55e5a587a0ad02fa1f8993027b46162a14a58832
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96448238"
---
# <a name="configure-monitoring-in-azure-monitor-for-vms-guest-health-using-data-collection-rules-preview"></a>Configuración de la supervisión en el estado de invitado de Azure Monitor para VM mediante reglas de recopilación de datos (versión preliminar)
El [estado de invitado de Azure Monitor para VM](vminsights-health-overview.md) permite ver el estado de una máquina virtual conforme a la definición de un conjunto de medidas de rendimiento que se muestrean a intervalos regulares. En este artículo se describe cómo puede modificar la supervisión predeterminada en varias máquinas virtuales mediante reglas de recopilación de datos.


## <a name="monitors"></a>Monitores
El estado de mantenimiento de una máquina virtual viene determinado por la [acumulación de estados](vminsights-health-overview.md#health-rollup-policy) de cada uno de sus monitores. Hay dos tipos de monitores en el estado de invitado de Azure Monitor para VM, tal como se muestra en la tabla siguiente.

| Supervisión | Descripción |
|:---|:---|
| Monitor de unidad | Mide algún aspecto de un recurso o una aplicación. Esto podría estar comprobando un contador de rendimiento para determinar el rendimiento del recurso o su disponibilidad. |
| Monitor agregado | Agrupa varios monitores para proporcionar un estado de mantenimiento agregado único. Un monitor agregado puede contener uno o varios monitores de unidad, así como otros monitores agregados. |

No se puede cambiar directamente el conjunto de monitores que usa el estado de invitado de Azure Monitor para VM y su configuración. Puede crear [invalidaciones](#overrides) a través de las cuales se modifica el comportamiento de la configuración predeterminada. Las invalidaciones se definen en las reglas de recopilación de datos. Puede crear varias reglas de recopilación de datos que contengan varias invalidaciones para lograr la configuración de supervisión necesaria.

## <a name="monitor-properties"></a>Propiedades de monitor
En la tabla siguiente se describen las propiedades que debe configurar en cada monitor.

| Propiedad | Monitores | Descripción |
|:---|:---|:---|
| habilitado | Agregado<br>Unidad | Si es true, se calcula el monitor de estado y se contribuye al estado de la máquina virtual. Puede desencadenar una alerta si las alertas están habilitadas. |
| Alertas | Agregado<br>Unidad | Si es true, se desencadena una alerta para el monitor cuando se pasa a un estado incorrecto. Si es false, el estado del monitor seguirá contribuyendo al estado de la máquina virtual, que podría desencadenar una alerta. |
| Advertencia | Unidad | Criterios para el estado de advertencia. Si no se especifica ninguno, el monitor nunca entrará en estado de advertencia. |
| Crítico | Unidad | Criterios para el estado crítico. Si no se especifica ninguno, el monitor nunca entrará en estado crítico. |
| Frecuencia de evaluación | Unidad | Frecuencia con la que se evalúa el estado de mantenimiento. |
| Retrospectiva | Unidad | Tamaño de la ventana de retrospectiva en segundos. Consulte [elemento monitorConfiguration](#monitorconfiguration-element) para obtener una descripción detallada. |
| Tipo de evaluación | Unidad | Define el valor que se va a usar en el conjunto de muestras. Consulte [elemento monitorConfiguration](#monitorconfiguration-element) para obtener una descripción detallada. |
| Mín. de muestras | Unidad | Número mínimo de valores que se van a usar para calcular el valor. |
| Máx. de muestras | Unidad | Número máximo de valores que se van a usar para calcular el valor. |


## <a name="default-configuration"></a>Configuración predeterminada
En la tabla siguiente se muestra la configuración predeterminada de cada monitor. Esta configuración predeterminada no se puede cambiar directamente, pero puede definir [invalidaciones](#overrides) que modificarán la configuración del monitor para determinadas máquinas virtuales.


| Supervisión | habilitado | Alertas | Advertencia | Crítico | Frecuencia de evaluación | Retrospectiva | Tipo de evaluación | Mín. de muestras | Máx. de muestras |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| Uso de CPU  | True | False | None | \> 90 %    | 60 segundos | 240 segundos | Min | 2 | 3 |
| Memoria disponible | True | False | None | \< 100 MB | 60 segundos | 240 segundos | Max | 2 | 3 |
| Sistema de archivos      | True | False | None | \< 100 MB | 60 segundos | 120 segundos | Max | 1 | 1 |


## <a name="overrides"></a>Invalidaciones
Una *invalidación* cambia una o varias propiedades de un monitor. Por ejemplo, una invalidación podría deshabilitar un monitor habilitado de manera predeterminada, definir criterios de advertencia para el monitor o modificar el umbral crítico del monitor. 

Las invalidaciones se definen en una [regla de recopilación de datos (DCR)](../platform/data-collection-rule-overview.md). Puede crear varias reglas de recopilación de datos con diferentes conjuntos de invalidaciones y aplicarlas a varias máquinas virtuales. Para aplicar una regla de recopilación de datos a una máquina virtual, cree una asociación como se describe en [Configuración de la recopilación de datos para el agente de Azure Monitor (versión preliminar)](../platform/data-collection-rule-azure-monitor-agent.md#data-collection-rule-associations).


## <a name="multiple-overrides"></a>Varias invalidaciones

Un monitor único puede tener varias invalidaciones. Si las invalidaciones definen propiedades distintas, la configuración resultante es una combinación de todas las invalidaciones.

Por ejemplo, el monitor `memory|available` no especifica un umbral de advertencia ni habilita las alertas de manera predeterminada. Tenga en cuenta las siguientes invalidaciones aplicadas a este monitor:

- La invalidación 1 define la propiedad `alertConfiguration.isEnabled` en el valor `true`.
- La invalidación 2 define la propiedad `monitorConfiguration.warningCondition` con una condición de umbral de `< 250`.

La configuración resultante sería un monitor que pasa a un estado de mantenimiento de advertencia y crea una alerta de gravedad 2 cuando hay menos de 250 MB de memoria disponible, y que también pasa al estado de mantenimiento crítico cuando hay menos de 100 MB de memoria disponible y crea una alerta de gravedad 1 (o cambia la alerta existente de gravedad 2 a 1 si ya existía).

Si dos invalidaciones definen la misma propiedad en el mismo monitor, uno de los valores tendrá prioridad. Las invalidaciones se aplicarán en función de su [ámbito](#scopes-element), del más general al más específico. Esto significa que las invalidaciones más específicas tendrán mayores probabilidades de aplicarse. El orden específico es el siguiente:

1. Global 
2. Subscription
3. Resource group
4. Una máquina virtual. 

Si varias invalidaciones en el mismo nivel de ámbito definen la misma propiedad en el mismo monitor, se aplican en el orden en que aparecen en la regla de recopilación de datos. Si las invalidaciones se encuentran en reglas de recopilación de da tos diferentes, se aplican según el orden alfabético de los id. de recurso de la regla de recopilación de datos.


## <a name="data-collection-rule-configuration"></a>Configuración de la regla de recopilación de datos
Los elementos JSON de la regla de recopilación de datos que definen las invalidaciones se describen en las secciones siguientes. Puede consultar un ejemplo completo en [Regla de recopilación de datos de ejemplo](#sample-data-collection-rule).

## <a name="extensions-structure"></a>Estructura extensions
El estado del invitado se implementa como una extensión del agente de Azure Monitor, por lo que las invalidaciones se definen en el elemento `extensions` de la regla de recopilación de datos. 

```json
"extensions": [
    {
        "name": "Microsoft-VMInsights-Health",
        "streams": [
            "Microsoft-HealthStateChange"
        ],
        "extensionName": "HealthExtension",
        "extensionSettings": {   }
    }
]
```

| Elemento | Obligatorio | Descripción |
|:---|:---|:---|
| `name` | Sí | Cadena definida por el usuario para la extensión. |
| `streams` | Sí | Lista de secuencias a las que se enviarán los datos de estado del invitado. Debe incluir **Microsoft-HealthStateChange**.  |
| `extensionName` | Sí | Nombre de la extensión. Debe ser **HealthExtension**. |
| `extensionSettings` | Sí | Matriz de elementos `healthRuleOverride` que se van a aplicar a la configuración predeterminada. |


## <a name="extensionsettings-element"></a>Elemento extensionSettings
Contiene los valores para la extensión.

```json
"extensionSettings": {
    "schemaVersion": "1.0",
    "contentVersion": "",
    "healthRuleOverrides": [ ]
}
```

| Elemento | Obligatorio | Descripción |
|:---|:---|:---|
| `schemaVersion` | Sí | Cadena definida por Microsoft para representar el esquema esperado del elemento. Actualmente debe estar establecida en 1.0. |
| `contentVersion` | No | Cadena definida por el usuario para realizar un seguimiento de las distintas versiones de la configuración de estado, si es necesario. |
| `healthRuleOverrides` | Sí | Matriz de elementos `healthRuleOverride` que se van a aplicar a la configuración predeterminada. |

## <a name="healthrulesoverrides-element"></a>Elemento healthRulesOverrides
Contiene uno o varios elementos `healthRuleOverride` que definen cada uno una invalidación.

```json
"healthRuleOverrides": [
    {
        "scopes": [ ],
        "monitors": [ ],
        "monitorConfiguration": { },
        "alertConfiguration": {  },
        "isEnabled": true|false
    }
]
```

| Elemento | Obligatorio | Descripción |
|:---|:---|:---|
| `scopes` | Sí | Lista de uno o varios ámbitos que especifican las máquinas virtuales a las que se puede aplicar esta invalidación. Aunque la regla de recopilación de datos está asociada a una máquina virtual, la máquina virtual debe estar dentro de un ámbito para que se aplique la invalidación. |
| `monitors` | Sí | Lista de una o varias cadenas que definen qué monitores recibirán esta invalidación.  |
| `monitorConfiguration` | No | Configuración para el monitor, incluidos los estados de mantenimiento y cómo se calculan. |
| `alertConfiguration` | No | Configuración de alertas para el monitor. |
| `isEnabled` | No | Controla si el monitor está habilitado o no. El monitor deshabilitado cambia a un estado de mantenimiento *deshabilitado* especial y permanece deshabilitado, a menos que se vuelva a habilitar. Si se omite, el monitor heredará su estado del monitor primario de la jerarquía. |


## <a name="scopes-element"></a>Elemento scopes
Cada invalidación tiene uno o más ámbitos que definen a qué máquinas virtuales se debe aplicar la invalidación. El ámbito puede ser una suscripción, un grupo de recursos o incluso una sola máquina virtual. Incluso si la invalidación está en una regla de recopilación de datos asociada a una máquina virtual determinada, solo se aplica a esa máquina virtual si esta se encuentra dentro de uno de los ámbitos de la invalidación. Esto permite asociar de manera general un número menor de reglas de recopilación de datos a un conjunto de máquinas virtuales, pero proporciona un control detallado sobre la asignación de cada invalidación dentro de la regla de recopilación de datos propiamente dicha. Se recomienda que cree un conjunto pequeño de reglas de recopilación de datos y lo asocie a un conjunto de máquinas virtuales mediante una directiva y que especifique las invalidaciones del monitor de estado para diferentes subconjuntos de esas máquinas virtuales mediante el elemento scopes.

La tabla siguiente muestra ejemplos de distintos ámbitos.

| Ámbito | Ejemplo |
|:---|:---|
| una sola máquina virtual | `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rg-name/providers/Microsoft.Compute/virutalMachines/my-vm` |
| Todas las máquinas virtuales en un grupo de recursos | `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rg-name` |
| Todas las máquinas virtuales de una suscripción | `/subscriptions/00000000-0000-0000-0000-000000000000/` |
| Todas las máquinas virtuales a las que está asociada la regla de recopilación de datos | `*` |


## <a name="monitors-element"></a>Elemento monitors
Lista de una o varias cadenas que definen qué monitores de la jerarquía de estados recibirán esta invalidación. Cada elemento puede ser un nombre de monitor o un nombre de tipo que coincida con uno o varios monitores que recibirán esta invalidación. 

```json
"monitors": [
    "<monitor name>"
 ],
```



En la tabla siguiente se enumeran los nombres de monitor disponibles actualmente.

| Nombre de tipo | Nombre | Descripción |
|:---|:---|:---|
| root | root | Monitor de nivel superior que representa el estado de la máquina virtual. | |
| cpu-utilization | cpu-utilization | Monitor del uso de CPU. | |
| logical-disks | logical-disks | Monitor agregado para el estado de mantenimiento de todos los discos supervisados en la máquina virtual Windows. | |
| logical-disks\|* | logical-disks\|C:<br>logical-disks\|D: | Monitor agregado que realiza el seguimiento del estado de un disco específico en una máquina virtual Windows. | 
| logical-disks\|*\|free-space | logical-disks\|C:\|free-space<br>logical-disks\|D:\|free-space | Monitor del espacio disponible en disco en la máquina virtual Windows. |
| filesystems | filesystems | Monitor agregado para el estado de todos los sistemas de archivos en una máquina virtual Linux. |
| filesystems\|* | filesystems\|/<br>filesystems\|/mnt | Monitor agregado que realiza el seguimiento del estado de un sistema de archivos en una máquina virtual Linux. | filesystems|/var/log |
| filesystems\|*\|free-space | filesystems\|/\|free-space<br>filesystems\|/mnt\|free-space | Monitor del espacio disponible en disco en el sistema de archivos de la máquina virtual Linux. | 
| memoria | memoria | Monitor agregado para el estado de la memoria de la máquina virtual. | |
| memory\|available| memory\|available | Monitor que realiza el seguimiento de la memoria disponible en la máquina virtual. | |


## <a name="alertconfiguration-element"></a>Elemento alertConfiguration
Especifica si se debe crear una alerta del monitor.

```json
"alertConfiguration": {
    "isEnabled": true|false
}
```

| Elemento | Mandatory | Descripción | 
|:---|:---|:---|
| `isEnabled` | No | Si se establece en true, el monitor generará una alerta al cambiar a un estado crítico o de advertencia y resolverá la alerta cuando vuelva a un estado correcto. Si es false o se omite, no se genera ninguna alerta.  |


## <a name="monitorconfiguration-element"></a>Elemento monitorConfiguration
Solo se aplica a los monitores de unidad. Define la configuración para el monitor, incluidos los estados de mantenimiento y cómo se calculan.

Los parámetros definen el algoritmo para calcular el valor de la métrica que se va a comparar con los umbrales. En lugar de tomar medidas según una muestra de datos de la métrica subyacente, el monitor evalúa varias muestras de métricas que se hayan recibido durante la ventana entre el momento de la evaluación y hace `lookbackSec`. Se tienen en cuenta todas las muestras recibidas durante ese período de tiempo y, si el recuento de muestras es mayor que `maxSamples`, se omiten las muestras anteriores a `maxSamples`. 

En caso de que haya menos muestras en el intervalo de retrospectiva que `minSamples`, el monitor cambiará al estado de mantenimiento *Desconocido*, que indica que no hay suficientes datos para tomar decisiones fundamentadas sobre el estado de las métricas subyacentes. Si hay disponible un número de muestras mayor que `minSamples`, se ejecuta una función de agregación especificada por el parámetro evaluationType en el conjunto para calcular un valor único.


```json
"monitorConfiguration": {
        "evaluationType" : "<type-of-evaluation>",
        "lookbackSecs": <lookback-number-of-seconds>,
        "evaluationFrequencySecs": <evaluation-frequency-number-of-seconds>,
        "minSamples": <minimum-samples>,
        "maxSamples": <maximum-samples>,
        "warningCondition": {  },
        "criticalCondition": {  }
    }
}
```

| Elemento | Mandatory | Descripción | 
|:---|:---|:---|
| `evaluationFrequencySecs` | No | Define la frecuencia de evaluación del estado de mantenimiento. Cada monitor se evalúa en el momento en que el agente se inicia y en intervalos regulares según este parámetro. |
| `lookbackSecs`   | No | Tamaño de la ventana de retrospectiva en segundos. |
| `evaluationType` | No | `min`: toma el valor mínimo del conjunto completo de muestras.<br>`max`: toma el valor máximo del conjunto completo de muestras.<br>`avg`: toma el promedio de los valores del conjunto de muestras.<br>`all`: compara cada valor único del conjunto con los umbrales. El monitor cambia de estado únicamente si todas las muestras del conjunto satisfacen la condición del umbral. |
| `minSamples`     | No | Número mínimo de valores que se van a usar para calcular el valor. |
| `maxSamples`     | No | Número máximo de valores que se van a usar para calcular el valor. |
| `warningCondition`  | No | Umbral y lógica de comparación de la condición de advertencia. |
| `criticalCondition` | No | Umbral y lógica de comparación de la condición crítica. |


## <a name="warningcondition-element"></a>Elemento warningCondition
Define el umbral y la lógica de comparación de la condición de advertencia. Si no se incluye este elemento, el monitor nunca cambiará al estado de mantenimiento de advertencia.

```json
"warningCondition": {
    "isEnabled": true|false,
    "operator": "<comparison-operator>",
    "threshold": <threshold-value>
},
```

| Propiedad | Mandatory | Descripción | 
|:---|:---|:---|
| `isEnabled` | No | Especifica si está habilitada la condición. Si se establece en **false**, se deshabilita la condición aunque estén establecidas las propiedades de umbral y de operador. |
| `threshold` | No | Define el umbral con el que se compara el valor evaluado. |
| `operator`  | No | Define el operador de comparación que se va a usar en la expresión de umbral. Valores posibles: >, <, >=, <=, ==. |


## <a name="criticalcondition-element"></a>Elemento criticalCondition
Define el umbral y la lógica de comparación de la condición crítica. Si no se incluye este elemento, el monitor nunca cambiará al estado de mantenimiento crítico.

```json
"criticalCondition": {
    "isEnabled": true|false,
    "operator": "<comparison-operator>",
    "threshold": <threshold-value>
},
```

| Propiedad | Mandatory | Descripción | 
|:---|:---|:---|
| `isEnabled` | No | Especifica si está habilitada la condición. Si se establece en **false**, se deshabilita la condición aunque estén establecidas las propiedades de umbral y de operador. |
| `threshold` | No | Define el umbral con el que se compara el valor evaluado. |
| `operator`  | No | Define el operador de comparación que se va a usar en la expresión de umbral. Valores posibles: >, <, >=, <=, ==. |

## <a name="sample-data-collection-rule"></a>Eliminación de una regla de recopilación de datos
Para ver una regla de recopilación de datos de ejemplo que habilite la supervisión de invitados, consulte [Habilitación de una máquina virtual mediante una plantilla de Resource Manager](vminsights-health-enable.md#enable-a-virtual-machine-using-resource-manager-template).


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [reglas de recopilación de datos](../platform/data-collection-rule-overview.md).