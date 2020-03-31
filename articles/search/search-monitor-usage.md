---
title: Supervisión de operaciones y actividades
titleSuffix: Azure Cognitive Search
description: Habilite el registro y obtenga métricas de actividades de consulta, uso de recursos y otros datos del sistema de un servicio de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/15/2020
ms.openlocfilehash: 353e00f902a7314e5e5b7c8ee03e8b925a510b26
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462333"
---
# <a name="monitor-operations-and-activity-of-azure-cognitive-search"></a>Supervisión de operaciones y actividades de Azure Cognitive Search

En este artículo se presenta la supervisión en el nivel de servicio (recurso), en el nivel de carga de trabajo (consultas e indexación) y se sugiere un marco para supervisar el acceso de los usuarios.

En el espectro, usará una combinación de infraestructura integrada y servicios básicos como Azure Monitor, así como las API de servicio que devuelven estadísticas, recuentos y estado. Conocer el rango de funcionalidades puede ayudarle a crear un bucle de comentarios para que pueda solucionar los problemas a medida que surjan.

## <a name="use-azure-monitor"></a>Uso de Azure Monitor

Muchos servicios, entre los que se incluye Azure Cognitive Search, aprovechan [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/) para las alertas, métricas y registro de datos de diagnóstico. En el caso de Azure Cognitive Search, la infraestructura de supervisión integrada se usa principalmente para la supervisión de nivel de recurso (estado del servicio) y la [supervisión de consultas](search-monitor-queries.md).

La siguiente captura de pantalla le ayuda a buscar características de Azure Monitor en el portal.

+ La pestaña**Supervisión**, que se encuentra en la página de información general principal, muestra las métricas clave de un vistazo.
+ **Registro de actividad**, inmediatamente debajo de Información general, informa acerca de acciones de nivel de recurso: estado del servicio y notificaciones de solicitud de clave de API.
+ En **Supervisión**, que está más abajo en la lista, proporciona alertas, métricas y registros de diagnóstico configurables. Créelos cuando los necesite. Una vez que los datos se recopilan y almacenan, puede consultar o visualizar los detalles de la información.

![Integración de Azure Monitor en un servicio de búsqueda](./media/search-monitor-usage/azure-monitor-search.png
 "Integración de Azure Monitor en un servicio de búsqueda")

### <a name="precision-of-reported-numbers"></a>Precisión de los números notificados

Las páginas del portal se actualizan cada pocos minutos. Como tales, los números que se notifican en el portal son aproximados y están pensados para proporcionarle una sensación general de la capacidad del sistema para atender las solicitudes. Las métricas actuales, como las consultas por segundo (QPS), pueden ser mayores o menores que el número que se muestra en la página.

## <a name="activity-logs-and-service-health"></a>Registros de actividad y estado del servicio

El [**registro de actividad**](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) recopila información de Azure Resource Manager e informa sobre los cambios en el estado del servicio. Puede supervisar si en el registro de actividad hay condiciones críticas, de error y de advertencia relacionadas con el estado del servicio.

En el caso de las tareas en el servicio (como las consultas, la indexación o la creación de objetos), verá notificaciones informativas genéricas como *Get Admin Key* y *Get Query keys* para cada solicitud, pero no la acción específica en sí. Para obtener información de este nivel de detalle, debe configurar el registro de diagnóstico.

Puede acceder al **registro de actividad** desde el panel de navegación izquierdo, desde las notificaciones de la barra de comandos de la ventana superior o desde la página **Diagnose and solve problems** (Diagnosticar y resolver problemas).

## <a name="monitor-storage"></a>Supervisión de almacenamiento

Las páginas con pestañas integradas en la página de información general generan informes del uso de los recursos. Esta información está disponible en cuanto comienza a usar el servicio, no es preciso realizar ningún tipo de configuración, y la página se actualiza cada pocos minutos. 

Si va a concretar las decisiones sobre [qué nivel usar para las cargas de trabajo de producción](search-sku-tier.md) o si [ajustar el número de réplicas activas y particiones](search-capacity-planning.md), estas métricas pueden ayudarle con esas decisiones al mostrarle la velocidad a la que se consumen los recursos y el modo en que la configuración actual administra la carga existente.

Las alertas relacionadas con el almacenamiento no están disponibles actualmente; el consumo de almacenamiento no se agrega ni se registra en la tabla **AzureMetrics** de Azure Monitor. Necesitaría [compilar una solución personalizada](https://docs.microsoft.com/azure/azure-monitor/insights/solutions-creating) que emita notificaciones relacionadas con los recursos, en las que el código comprueba el tamaño del almacenamiento y controla la respuesta. Para más información sobre las métricas de almacenamiento, consulte [Obtención de estadísticas del servicio](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics#response).

Para la supervisión visual en el portal, la pestaña **Uso** muestra la disponibilidad de los recursos, en relación con los [límites](search-limits-quotas-capacity.md) actuales que impone el nivel de servicio. 

La siguiente ilustración es del servicio gratuito, que está limitado a 3 objetos de cada tipo y 50 MB de almacenamiento. Un servicio básico o estándar tiene límites más altos y, si aumentan los recuentos de particiones, el almacenamiento máximo aumenta proporcionalmente.

![Estado de uso en relación con los límites del nivel](./media/search-monitor-usage/usage-tab.png
 "Estado de uso en relación con los límites del nivel")

## <a name="monitor-workloads"></a>Supervisión de cargas de trabajo

Los eventos registrados incluyen los relacionados con la indexación y las consultas. La tabla **AzureDiagnostics** de Log Analytics recopila datos operativos relacionados con consultas e indexación.

La mayoría de los datos registrados son para operaciones de solo lectura. En el caso de otras operaciones de creación-actualización-eliminación no capturadas en el registro, puede consultar en el servicio de búsqueda información del sistema.

| OperationName | Descripción |
|---------------|-------------|
| ServiceStats | Esta operación es una llamada de una rutina a [Get Service Statistics](https://docs.microsoft.com/rest/api/searchservice/get-service-statistics), que se realiza de forma directa o implícita para rellenar una página de información general del portal cuando se carga o se actualiza. |
| Query.Search |  Consulte [Supervisión de consultas](search-monitor-queries.md) para ver información acerca de las consultas registradas.|
| Indexing.Index  | Esta operación es una llamada a [Agregación, actualización o eliminación de documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents). |
| indexes.Prototype | Este es un índice que crea el Asistente para importar datos. |
| Indexers.Create | Cree un índice de forma explícita o implícita mediante el Asistente para importar datos. |
| Indexers.Get | Devuelve el nombre de un indexador siempre que este se ejecuta. |
| Indexers.Status | Devuelve el estado de un indexador siempre que este se ejecuta. |
| DataSources.Get | Devuelve el nombre del origen de datos siempre que se ejecuta un indexador.|
| Indexes.Get | Devuelve el nombre de un índice siempre que se ejecuta un indexador. |

### <a name="kusto-queries-about-workloads"></a>Consultas de Kusto acerca de las cargas de trabajo

Si ha habilitado el registro, puede consultar en **AzureDiagnostics** una lista de operaciones que se han ejecutado en el servicio y cuándo se han ejecutado. También puede poner en correlación una actividad para investigar los cambios en el rendimiento.

#### <a name="example-list-operations"></a>Ejemplo: Mostrar operaciones 

Devuelve una lista de operaciones y un recuento de cada una de ellas.

```
AzureDiagnostics
| summarize count() by OperationName
```

#### <a name="example-correlate-operations"></a>Ejemplo: Poner en correlación operaciones

Pone en correlación una solicitud de consulta con operaciones de indexación y representar los puntos de datos en un gráfico de tiempo.

```
AzureDiagnostics
| summarize OperationName, Count=count()
| where OperationName in ('Query.Search', 'Indexing.Index')
| summarize Count=count(), AvgLatency=avg(DurationMs) by bin(TimeGenerated, 1h), OperationName
| render timechart
```

### <a name="use-search-apis"></a>Uso de API de búsqueda

La API REST de Azure Cognitive Search y .NET SDK proporcionan acceso mediante programación a información de métricas de servicio, índice e indexadores y recuentos de documentos.

+ [GET Service Statistics](/rest/api/searchservice/get-service-statistics)
+ [GET Index Statistics](/rest/api/searchservice/get-index-statistics)
+ [GET Document Counts](/rest/api/searchservice/count-documents)
+ [GET Indexer Status](/rest/api/searchservice/get-indexer-status)

## <a name="monitor-user-access"></a>Supervisión del acceso de los usuarios

Dado que los índices de búsqueda son un componente de una aplicación cliente mayor, no hay ninguna metodología integrada para controlar o supervisar el acceso a un índice por usuario. Se asume que las solicitudes provienen de una aplicación cliente, tanto para las solicitudes de consulta como del administrador. Las operaciones de escritura y lectura del administrador incluyen la creación, actualización y eliminación de objetos en todo el servicio. Las operaciones de solo lectura son consultas a la colección de documentos, cuyo ámbito es un solo índice. 

De por sí, lo que verá en los registros de actividad son referencias a llamadas mediante claves de administrador o claves de consulta. La clave apropiada se incluye en las solicitudes que parten del código de cliente. El servicio no está equipado para administrar los tokens de identidad o la suplantación.

Cuando existen requisitos empresariales para la autorización por usuario, se recomienda la integración con Azure Active Directory. Puede usar $filter y las identidades del usuario para [recortar los resultados de la búsqueda](search-security-trimming-for-azure-search-with-aad.md) de documentos que un usuario no debe ver. 

No hay ninguna forma de registrar esta información por separado de la cadena de consulta que incluye el parámetro $filter. Para obtener información acerca de la generación de informes de cadenas de consulta, consulte [Administración de consultas](search-monitor-queries.md).

## <a name="next-steps"></a>Pasos siguientes

La fluidez con Azure Monitor es esencial para supervisar todos los servicios de Azure, lo que incluye recursos como Azure Cognitive Search. Si no conoce Azure Monitor, dedique un tiempo a leer los artículos relacionados con los recursos. Además de los tutoriales, el siguiente artículo es un buen punto de partida.

> [!div class="nextstepaction"]
> [Supervisión de recursos de Azure con Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource)
