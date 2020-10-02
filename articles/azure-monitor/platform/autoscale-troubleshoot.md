---
title: Solución de problemas de escalabilidad automática de Azure
description: Seguimiento de problemas con la escalabilidad automática de Azure utilizada en Service Fabric, Virtual Machines, Web Apps y Cloud Services.
ms.topic: conceptual
ms.date: 11/4/2019
ms.subservice: autoscale
ms.openlocfilehash: a29b5d11a6ea06af9d5b6a8b5120c6f0caa6601e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90979051"
---
# <a name="troubleshooting-azure-autoscale"></a>Solución de problemas de escalabilidad automática de Azure
 
Gracias a la escalabilidad automática de Azure Monitor, puede ejecutar la cantidad correcta de recursos para administrar la carga de la aplicación. Permite agregar recursos para administrar el aumento de la carga y ahorrar dinero mediante la eliminación de recursos inactivos. Puede escalar en función de una programación, una fecha y hora fijas o la métrica de recursos que elija. Para más información, consulte [Introducción a la escalabilidad automática](autoscale-overview.md).

El servicio de escalabilidad automática proporciona métricas y registros para comprender las acciones de escalado que se han producido y la evaluación de las condiciones que han dado lugar a esas acciones. Puede encontrar respuesta a preguntas como las siguientes:

- ¿Por qué mi servicio se ha escalado o reducido horizontalmente?
- ¿Por qué no se ha escalado mi servicio?
- ¿Por qué se ha producido un error en una acción de escalabilidad automática?
- ¿Por qué una acción de escalabilidad automática tarda tiempo en escalar?
  
## <a name="autoscale-metrics"></a>Métricas de escalabilidad automática

La escalabilidad automática le proporciona [cuatro métricas](metrics-supported.md#microsoftinsightsautoscalesettings) para comprender su funcionamiento. 

- **Valor de métrica observado**: el valor de la métrica en la que ha elegido llevar a cabo la acción de escalado, tal como lo ha detectado o calculado el motor de escalabilidad automática. Dado que una única configuración de escalabilidad automática puede tener varias reglas y, por lo tanto, varios orígenes de métricas, puede filtrar mediante "origen de métrica" como dimensión.
- **Umbral de métrica**: el umbral establecido para realizar la acción de escalado. Como una única configuración de escalabilidad automática puede tener varias reglas y, por lo tanto, varios orígenes de métricas, puede filtrar mediante "regla de métrica" como dimensión.
- **Capacidad observada**: el número activo de instancias del recurso de destino tal como lo ha detectado el motor de escalabilidad automática.
- **Acciones de escalado iniciadas**: el número de acciones de escalabilidad horizontal y reducción horizontal iniciadas por el motor de escalabilidad automática. Puede filtrar por acciones de escalabilidad horizontal y de reducción horizontal.

Puede utilizar el [Explorador de métricas](metrics-getting-started.md) para representar todas las métricas anteriores en un único lugar. El gráfico debería mostrar:

  - la métrica real
  - la métrica tal como la ha detectado o calculado el motor de escalabilidad automática
  - el umbral de una acción de escalado
  - el cambio en capacidad 

## <a name="example-1---analyzing-a-simple-autoscale-rule"></a>Ejemplo 1: análisis de una regla de escalabilidad automática simple 

Tenemos una configuración de escalabilidad automática simple para un conjunto de escalado de máquinas virtuales que:

- se escala horizontalmente cuando el promedio de porcentaje de CPU de un conjunto es superior al 70 % durante 10 minutos 
- se reduce horizontalmente cuando el porcentaje de CPU del conjunto es inferior al 5 % durante más de 10 minutos. 

Vamos a revisar las métricas del servicio de escalabilidad automática.
 
![Captura de pantalla que muestra un ejemplo de porcentaje de CPU del conjunto de escalado de máquinas virtuales.](media/autoscale-troubleshoot/autoscale-vmss-CPU-ex-full-1.png)

![Ejemplo de porcentaje de CPU del conjunto de escalado de máquinas virtuales](media/autoscale-troubleshoot/autoscale-vmss-CPU-ex-full-2.png)

***Figura 1a: porcentaje de métricas de CPU para el conjunto de escalado de máquinas virtuales y la métrica Valor de métrica observado para la configuración de escalabilidad automática***

![Umbral de métrica y capacidad observada](media/autoscale-troubleshoot/autoscale-metric-threshold-capacity-ex-full.png)

***Figura 1b: Umbral de métrica y Capacidad observada***

En la ilustración 1b, el **Umbral de métrica** (línea azul claro) de la regla de escalabilidad horizontal es 70.  La **Capacidad observada** (línea azul oscura) muestra el número de instancias activas, que actualmente es de 3. 

> [!NOTE]
> Tendrá que filtrar el **Umbral de métrica** por la regla de escalabilidad horizontal de la dimensión de la regla del desencadenador de métricas (aumento) para ver el umbral de escalabilidad horizontal y la escala en la regla (reducción). 

## <a name="example-2---advanced-autoscaling-for-a-virtual-machine-scale-set"></a>Ejemplo 2: escalabilidad automática avanzada para un conjunto de escalado de máquinas virtuales

Disponemos de una configuración de escalabilidad automática que permite escalar horizontalmente un recurso del conjunto de escalado de máquinas virtuales en función de su propia métrica **Flujos salientes**. Observe que la opción de **dividir la métrica por el recuento de instancias** para el umbral de métrica está activada. 

La regla de la acción de escalado es: 

Si el valor de **Outbound Flow per instance** (Flujo saliente por instancia) es mayor que 10, el servicio de escalabilidad automática se debe escalar horizontalmente en 1 instancia. 

En este caso, el valor de métrica observado del motor de escalabilidad automática se calcula como el valor de métrica real dividido por el número de instancias. Si el valor de métrica observado es inferior al umbral, no se inicia ninguna acción de escalabilidad horizontal. 
 
![Captura de pantalla que muestra la página del promedio de flujos de salida con un ejemplo de los gráficos de métricas de escalado automático de un conjunto de escalado de máquinas virtuales.](media/autoscale-troubleshoot/autoscale-vmss-metric-chart-ex-1.png)

![Ejemplo de gráficos de métricas de escalabilidad automática del conjunto de escalado de máquinas virtuales](media/autoscale-troubleshoot/autoscale-vmss-metric-chart-ex-2.png)

***Figura 2: ejemplo de gráficos de métricas de escalabilidad automática del conjunto de escalado de máquinas virtuales***

En la ilustración 2, puede ver dos gráficos de métricas. 

En el gráfico de la parte superior se muestra el valor real de la métrica **Flujos salientes**. El valor real es 6. 

En el gráfico de la parte inferior se muestran algunos valores. 
 - El **Valor de métrica observado** (azul claro) es 3 porque hay 2 instancias activas y 6 dividido entre 2 es 3. 
 - La **Capacidad observada** (púrpura) muestra el recuento de instancias que detecta el motor de escalabilidad automática. 
 - El **Umbral de métrica** (verde claro) se establece en 10. 

Si hay varias reglas de acción de escalado, puede usar la división o la opción **Agregar filtro** en el gráfico del Explorador de métricas para ver la métrica de una regla o un origen específico. Para más información sobre cómo dividir un gráfico de métricas, consulte [Características avanzadas de gráficos de métricas: división](metrics-charts.md#apply-splitting-to-a-chart)

## <a name="example-3---understanding-autoscale-events"></a>Ejemplo 3: descripción de los eventos de escalabilidad automática

En la pantalla de configuración de escalabilidad automática, vaya a la pestaña **Hist. de eje.** para ver las acciones de escalado más recientes. En la pestaña también se muestra el cambio en la **Capacidad observada** a lo largo del tiempo. Para ver más detalles sobre todas las acciones de escalabilidad automática, incluidas las operaciones como actualizar o eliminar la configuración de escalabilidad automática, consulte el registro de actividad y filtre por las operaciones de escalabilidad automática.

![Configuración de la escalabilidad automática](media/autoscale-troubleshoot/autoscale-setting-run-history-smaller.png)

## <a name="autoscale-resource-logs"></a>Escalabilidad automática de registros de recursos

Igual que cualquier otro recurso de Azure, el servicio de escalabilidad automática proporciona [registros de recursos](platform-logs-overview.md). Existen dos categorías de registros.

- **Evaluaciones de escalabilidad automática**: el motor de escalabilidad automática registra las entradas de registro para cada evaluación de condición única cada vez que realiza una comprobación.  La entrada incluye detalles sobre los valores observados de las métricas, las reglas evaluadas y si la evaluación ha dado como resultado una acción de escalado o no.

- **Acciones de escalabilidad automática**: el motor registra los eventos de acción de escalado iniciados por el servicio de escalabilidad automática y los resultados de estas acciones de escalado (correcto, error y cantidad de escalado que ha detectado el servicio de escalabilidad automática).

Al igual que con cualquier servicio compatible con Azure Monitor, puede usar [Configuración de diagnóstico](diagnostic-settings.md) para enrutar estos registros:

- al área de trabajo de Log Analytics para análisis detallados
- a Event Hubs y, a continuación, a herramientas que no son de Azure
- a la cuenta de Azure Storage para archivo  

![Configuración de diagnóstico de escalabilidad automática](media/autoscale-troubleshoot/diagnostic-settings.png)

En la imagen anterior se muestra la configuración de diagnóstico de escalabilidad automática de Azure Portal. Allí puede seleccionar la pestaña Registros de diagnóstico/recursos y habilitar la recopilación y el enrutamiento de registros. También puede realizar la misma acción mediante la API REST, CLI, PowerShell y plantillas del Administrador de recursos para la configuración de diagnóstico eligiendo el tipo de recurso como *Microsoft.Insights/AutoscaleSettings*. 

## <a name="troubleshooting-using-autoscale-logs"></a>Solución de problemas mediante registros de escalabilidad automática 

Para una mejor experiencia de solución de problemas, se recomienda enrutar los registros a Registros de Azure Monitor (Log Analytics) a través de un área de trabajo cuando crea la configuración de escalabilidad automática. Este proceso se muestra en la imagen de la sección anterior. Puede asegurarse de las acciones de evaluación y escalado mejor con Log Analytics.

Una vez que haya configurado los registros de escalabilidad automática para enviarlos al área de trabajo de Log Analytics, puede ejecutar las siguientes consultas para comprobar los registros. 

Para empezar, pruebe esta consulta para ver los registros de evaluación de escalabilidad automática más recientes:

```Kusto
AutoscaleEvaluationsLog
| limit 50
```

O bien, intente la siguiente consulta para ver los registros de acciones de escalado más recientes:

```Kusto
AutoscaleScaleActionsLog
| limit 50
```

Use las secciones siguientes para estas cuestiones. 

## <a name="a-scale-action-occurred-that-i-didnt-expect"></a>Se ha producido una acción de escalado que no esperaba

En primer lugar, ejecute la consulta para que la acción de escalado encuentre la acción de escalado que le interesa. Si es la acción de escalado más reciente, utilice la siguiente consulta:

```Kusto
AutoscaleScaleActionsLog
| take 1
```

Seleccione el campo CorrelationId en el registro de acciones de escalado. Utilice el campo CorrelationId para buscar el registro de evaluación correcto. Al ejecutar la consulta siguiente, se mostrarán todas las reglas y condiciones evaluadas que llevan a esta acción de escalado.

```Kusto
AutoscaleEvaluationsLog
| where CorrelationId = "<correliationId>"
```

## <a name="what-profile-caused-a-scale-action"></a>¿Qué perfil ha provocado una acción de escalado?

Se ha producido una acción de escalado, pero tiene reglas y perfiles superpuestos, y debe realizar un seguimiento de cuáles son los que han producido la acción. 

Busque el campo correlationId de la acción de escalado (como se explica en el ejemplo 1) y, después, ejecute la consulta en los registros de evaluación para ver más información sobre el perfil.

```Kusto
AutoscaleEvaluationsLog
| where CorrelationId = "<correliationId_Guid>"
| where ProfileSelected == true
| project ProfileEvaluationTime, Profile, ProfileSelected, EvaluationResult
```

La evaluación de todo el perfil también se puede entender mejor con la siguiente consulta

```Kusto
AutoscaleEvaluationsLog
| where TimeGenerated > ago(2h)
| where OperationName contains == "profileEvaluation"
| project OperationName, Profile, ProfileEvaluationTime, ProfileSelected, EvaluationResult
```

## <a name="a-scale-action-did-not-occur"></a>No se ha producido una acción de escalado

Esperaba una acción de escalado y no se ha producido. Puede que no haya ningún registro o evento de acción de escalado.

Revise las métricas de escalabilidad automática si usa una regla de escalado basada en métricas. Es posible que el **Valor de métrica observado** o la **Capacidad observada** no sean lo que esperaba y, por lo tanto, no se haya activado la regla de escalado. Aún seguirá viendo evaluaciones, pero no una regla de escalabilidad horizontal. También es posible que el tiempo de recuperación impida que se produzca una acción de escalado. 

 Revise los registros de evaluación de escalabilidad automática durante el período de tiempo en el que esperaba que se produjera la acción de escalado. Revise todas las evaluaciones que ha realizado y por qué decidió no desencadenar una acción de escalado.


```Kusto
AutoscaleEvaluationsLog
| where TimeGenerated > ago(2h)
| where OperationName == "MetricEvaluation" or OperationName == "ScaleRuleEvaluation"
| project OperationName, MetricData, ObservedValue, Threshold, EstimateScaleResult
```

## <a name="scale-action-failed"></a>Error durante la acción de escalado

Puede haber casos en los que el servicio de escalabilidad automática haya realizado la acción de escalado, pero el sistema ha decidido no escalar o no ha podido completar la acción de escalado. Use esta consulta para buscar las acciones de escalado con errores.

```Kusto
AutoscaleScaleActionsLog
| where ResultType == "Failed"
| project ResultDescription
```

Cree reglas de alertas para recibir notificaciones de errores o acciones de escalabilidad automática. También puede crear reglas de alertas para recibir notificaciones sobre los eventos de escalabilidad automática.

## <a name="schema-of-autoscale-resource-logs"></a>Esquema de registros de recursos de escalabilidad automática

Para más información, consulte los [registros de recursos de escalabilidad automática](autoscale-resource-log-schema.md)

## <a name="next-steps"></a>Pasos siguientes
Lea información sobre [Procedimientos recomendados de escalabilidad automática](autoscale-best-practices.md). 
