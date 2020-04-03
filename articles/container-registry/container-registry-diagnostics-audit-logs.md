---
title: Recopilar y analizar registros de recursos
description: Registre y analice los eventos del registro de recursos de Azure Container Registry como la autenticación, y el envío y la incorporación de cambios en imágenes.
ms.topic: article
ms.date: 01/03/2020
ms.openlocfilehash: 00f9468721126bd166051df47cec1596356e9b54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409650"
---
# <a name="azure-container-registry-logs-for-diagnostic-evaluation-and-auditing"></a>Registros de Azure Container Registry para la evaluación y auditoría de diagnóstico

En este artículo se explica cómo recopilar datos de registro de un registro de contenedor de Azure mediante las características de [Azure Monitor](../azure-monitor/overview.md). Azure Monitor recopila [registros de recursos](../azure-monitor/platform/platform-logs-overview.md) (anteriormente denominados *registros de diagnóstico*) de eventos controlados por el usuario en el registro. Recopile y consuma estos datos para satisfacer ciertas necesidades, por ejemplo:

* Auditar eventos de autenticación del registro para garantizar la seguridad y el cumplimiento 

* Proporcionar un trazo de actividad completo de los artefactos del registro, como los eventos de envío e incorporación de cambios, para que pueda diagnosticar problemas operativos con el registro 

La colección de datos de registro de recursos mediante Azure Monitor puede incurrir en costos adicionales. Consulte la página de [precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/). 

## <a name="repository-events"></a>Eventos de repositorio

Actualmente se registran los siguientes eventos de nivel de repositorio para imágenes y otros artefactos:

* **Eventos de inserción**
* **Eventos de extracción**
* **Eventos de retirada de etiqueta**
* **Eventos de eliminación** (incluidos los eventos de eliminación de repositorios)

Eventos de nivel de repositorio que no se registran actualmente: Eventos de purga.

## <a name="registry-resource-logs"></a>Registros de recursos del registro

Los registros de recurso de Azure contienen datos emitidos por los recursos de Azure que describen su funcionamiento interno. En el caso de un registro de contenedor de Azure, los registros contienen eventos de autenticación y de nivel de repositorio almacenados en las tablas siguientes. 

* **ContainerRegistryLoginEvents**: estado y eventos de autenticación del registro, incluida la dirección IP y la identidad entrantes.
* **ContainerRegistryRepositoryEvents**: operaciones tales como el envío y la incorporación de cambios en imágenes y otros artefactos de repositorios del registro.
* **AzureMetrics**: [métricas del registro de contenedor](../azure-monitor/platform/metrics-supported.md#microsoftcontainerregistryregistries), como recuentos agregados de envío e incorporación de cambios.

En el caso de las operaciones, los datos de registro incluyen lo siguiente:
  * Estado de operación correcta o errónea
  * Marcas de hora de inicio y finalización

Además de los registros de recursos, Azure proporciona un [registro de actividad](../azure-monitor/platform/platform-logs-overview.md), un único registro de nivel de suscripción de eventos de administración de Azure, como la creación o eliminación de un registro de contenedor.

## <a name="enable-collection-of-resource-logs"></a>Habilitación de la recopilación de registros de recursos

La recopilación de registros de recursos de un registro de contenedor no está habilitada de forma predeterminada. Habilite explícitamente la configuración de diagnóstico de cada registro que quiera supervisar. Si desea ver las opciones para habilitar la configuración de diagnóstico, consulte [Creación de una configuración de diagnóstico para recopilar registros de plataforma y métricas en Azure](../azure-monitor/platform/diagnostic-settings.md).

Por ejemplo, para ver los registros y las métricas de un registro de contenedor casi en tiempo real en Azure Monitor, recopile los registros de recursos en un área de trabajo de Log Analytics. Para habilitar esta configuración de diagnóstico mediante Azure Portal, siga estos pasos:

1. Si aún no tiene un área de trabajo, cree una mediante [Azure Portal](../azure-monitor/learn/quick-create-workspace.md). Para minimizar la latencia en la colección de datos, asegúrese de que el área de trabajo se encuentra en la **misma región** que el registro de contenedor.
1. En el portal, seleccione el registro y, a continuación, seleccione **Supervisión > Configuración de diagnóstico > Agregar configuración de diagnóstico**.
1. Escriba un nombre para la configuración y seleccione **Enviar a Log Analytics**.
1. Seleccione el área de trabajo para los registros de diagnóstico del registro.
1. Seleccione los datos de registro que desea recopilar y haga clic en **Guardar**.

En la imagen siguiente se muestra la creación de una configuración de diagnóstico para un registro mediante el uso del portal.

![Habilitación de la configuración de diagnóstico](media/container-registry-diagnostics-audit-logs/diagnostic-settings.png)

> [!TIP]
> Recopile solo los datos que necesita equilibrando sus necesidades en cuanto a costo y supervisión. Por ejemplo, si solo necesita auditar eventos de autenticación, seleccione solo el registro **ContainerRegistryLoginEvents**. 

## <a name="view-data-in-azure-monitor"></a>Consulta de datos en Azure Monitor

Después de habilitar la colección de registros de diagnóstico en Log Analytics, los datos pueden tardar unos minutos en aparecer en Azure Monitor. Para ver los datos en el portal, seleccione el registro y, a continuación, seleccione **Supervisión > Registros**. Seleccione una de las tablas que contiene los datos del registro. 

Ejecute consultas para ver los datos. Se proporcionan varias consultas de ejemplo, pero también puede ejecutar la suya propia. Por ejemplo, la consulta siguiente recupera las 24 horas de datos más recientes de la tabla **ContainerRegistryRepositoryEvents**:

```Kusto
ContainerRegistryRepositoryEvents
| where TimeGenerated > ago(1d) 
```

En la imagen siguiente se muestra la salida de ejemplo:

![Consulta de datos de registro](media/container-registry-diagnostics-audit-logs/azure-monitor-query.png)

Para obtener un tutorial sobre el uso de Log Analytics en Azure Portal, consulte [Introducción a Log Analytics de Azure Monitor](../azure-monitor/log-query/get-started-portal.md) o pruebe el [entorno de demostración](https://portal.loganalytics.io/demo) de Log Analytics. 

Para obtener más información sobre las consultas de registro, vea [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

### <a name="additional-query-examples"></a>Ejemplos de consultas adicionales

#### <a name="100-most-recent-registry-events"></a>100 eventos de registro más recientes

```Kusto
ContainerRegistryRepositoryEvents
| union ContainerRegistryLoginEvents
| top 100 by TimeGenerated
| project TimeGenerated, LoginServer , OperationName , Identity , Repository , DurationMs , Region , ResultType
```

## <a name="additional-log-destinations"></a>Destinos de registro adicionales

Además de enviar los registros a Log Analytics, como alternativa, un escenario común es seleccionar una cuenta de Azure Storage como destino de registro. Para archivar los registros en Azure Storage, cree una cuenta de almacenamiento antes de habilitar el archivado a través de la configuración de diagnóstico.

Puede transmitir los eventos de registro de diagnóstico a un [centro de eventos de Azure](../event-hubs/event-hubs-what-is-event-hubs.md). Event Hubs puede ingerir millones de eventos por segundo, que posteriormente se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real. 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre cómo usar [Log Analytics](../azure-monitor/log-query/get-started-portal.md) y crear [consultas de registro](../azure-monitor/log-query/get-started-queries.md).
* Consulte [Introducción a los registros de plataforma Azure](../azure-monitor/platform/platform-logs-overview.md) para obtener información sobre los registros de la plataforma que están disponibles en las diferentes capas de Azure.

