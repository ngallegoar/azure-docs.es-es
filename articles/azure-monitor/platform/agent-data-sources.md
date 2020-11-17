---
title: Orígenes de datos de agente de Log Analytics en Azure Monitor
description: Los orígenes de datos definen los datos de registro que Azure Monitor recopila de agentes y otros orígenes conectados.  En este artículo se describe el concepto de cómo Azure Monitor usa los orígenes de datos, se explican los detalles de cómo configurarlos y se brinda un resumen de los distintos orígenes de datos disponibles.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/21/2020
ms.openlocfilehash: ad5e91a6dcdb61e09a64e61a27f12148ec28168e
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94490583"
---
# <a name="log-analytics-agent-data-sources-in-azure-monitor"></a>Orígenes de datos de agente de Log Analytics en Azure Monitor
Los datos que Azure Monitor recopila de máquinas virtuales con el agente de [Log Analytics](log-analytics-agent.md) se definen mediante los orígenes de datos que se configuran en el [área de trabajo de Log Analytics](data-platform-logs.md).   Cada origen de datos crea registros de un tipo determinado, donde cada tipo tiene su propio conjunto de propiedades.

> [!IMPORTANT]
> En este artículo se tratan los orígenes de datos con el [agente de Log Analytics](log-analytics-agent.md), que es uno de los agentes usados por Azure Monitor. Otros agentes recopilan otros datos y se configuran de forma diferente. Consulte [Información general sobre los agentes de Azure Monitor](agents-overview.md) para obtener una lista de los agentes disponibles y los datos que pueden recopilar.

![Recopilación de datos de registro](media/agent-data-sources/overview.png)

> [!IMPORTANT]
> Los orígenes de datos descritos en este artículo se aplican solo a las máquinas virtuales que ejecutan el agente de Log Analytics. 

## <a name="summary-of-data-sources"></a>Resumen de orígenes de datos
En la tabla siguiente se enumeran los orígenes de datos de agentes que actualmente se encuentran disponibles con el agente de Log Analytics.  Cada uno de ellos tiene un vínculo a un artículo independiente, donde se proporcionan detalles con respecto al origen de datos determinado.   También se proporciona información sobre el método y la frecuencia de recopilación. 


| Origen de datos | Plataforma | Agente de Log Analytics | Agente de Operations Manager | Almacenamiento de Azure | ¿Se requiere Operations Manager? | Se envían los datos del agente de Operations Manager a través del grupo de administración | Frecuencia de recopilación |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Registros personalizados](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | a la llegada |
| [Registros personalizados](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | a la llegada |
| [Registros de IIS](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |depende de la configuración de la sustitución incremental de archivos de registro |
| [Contadores de rendimiento](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |mínimo de 10 segundos, según lo programado |
| [Contadores de rendimiento](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |mínimo de 10 segundos, según lo programado |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Del almacenamiento de Azure: 10 minutos; del agente: a la llegada |
| [Registros de eventos de Windows](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | a la llegada |


## <a name="configuring-data-sources"></a>Configuración de orígenes de datos
Para configurar orígenes de datos para agentes de Log Analytics, vaya al menú **áreas de trabajo de Log Analytics** de Azure Portal y seleccione un área de trabajo. Haga clic en **Configuración avanzada** y **Datos**. Seleccione el origen de datos que quiere configurar. Puede seguir los vínculos de la tabla anterior para obtener documentación sobre cada origen de datos y detalles sobre su configuración.

Cualquier configuración se proporciona a todos los agentes conectados a esa área de trabajo.  No puede excluir ningún agente conectado de esta configuración.

![Configurar eventos de Windows](media/agent-data-sources/configure-events.png)



## <a name="data-collection"></a>datos, recopilación
Las configuraciones de orígenes de datos se entregan en cuestión de minutos a los agentes que están directamente conectados con Azure Monitor.  Los datos especificados se recopilan desde el agente y se entregan directamente a Azure Monitor a intervalos específicos a cada origen de datos.  Consulte la documentación de cada origen de datos para ver estas especificaciones.

En el caso de los agentes de System Center Operations Manager de un grupo de administración conectado, la configuración de origen de datos se convierte en módulos de administración y se proporciona al grupo de administración cada 5 minutos, de manera predeterminada.  El agente descarga el módulo de administración de la forma habitual y recopila los datos especificados. En función del origen de datos, los datos se enviarán a un servidor de administración que los reenvía a Azure Monitor o el agente los enviará a Azure Monitor sin pasar por el servidor de administración. Consulte [Detalles de la recopilación de datos para las soluciones de supervisión en Azure](../monitor-reference.md) para más información.  Puede leer detalles sobre cómo conectar Operations Manager y Azure Monitor y modificar la frecuencia con que la configuración se proporciona en [Configuración de integración con System Center Operations Manager](om-agents.md).

Si el agente no puede conectarse a Azure Monitor u Operations Manager, continuará recopilando datos para entregarlos cuando se establezca la conexión.  Si el volumen de datos alcanza el tamaño máximo de la memoria caché del cliente o si el agente no es capaz de establecer conexión en un plazo de 24 horas, los datos pueden perderse.

## <a name="log-records"></a>Entradas de registros
Todos los datos que recopila Azure Monitor se almacenan en el área de trabajo como registros.  Los registros que recopilan distintos orígenes de datos tendrán su propio conjunto de propiedades y se identificar por su propiedad **Type** .  Consulte la documentación de cada origen de datos y solución para obtener detalles sobre cada tipo de registro.

## <a name="next-steps"></a>Pasos siguientes
* Conozca las [soluciones de supervisión](../insights/solutions.md) que agregan funcionalidad a Azure Monitor y que también recopilan datos en el área de trabajo.
* Obtenga información acerca de las [consultas de registros](../log-query/log-query-overview.md) para analizar los datos recopilados de soluciones de supervisión y orígenes de datos.  
* Configure [alertas](alerts-overview.md) que le notifiquen de manera proactiva acerca de los datos críticos recopilados de soluciones de supervisión y orígenes de datos.
