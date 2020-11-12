---
title: Consulta de datos en Azure Monitor mediante Azure Data Explorer (versión preliminar)
description: Use Azure Data Explorer para realizar consultas entre productos entre Azure Data Explorer, áreas de trabajo de Log Analytics y aplicaciones clásicas de Application Insights en Azure Monitor.
author: orens
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 10/13/2020
ms.openlocfilehash: 9b434c426264fcfee0dfe663a7d1b21a354badec
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491263"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Consulta de datos en Azure Monitor mediante Azure Data Explorer (versión preliminar)
El clúster de proxy de Azure Data Explorer le permite realizar consultas entre productos entre Azure Data Explorer, áreas de trabajo de Log Analytics y aplicaciones clásicas de Application Insights en Azure Monitor. Puede asignar áreas de trabajo de Log Analytics en Azure Monitor o aplicaciones clásicas de Application Insights como clústeres de proxy. A continuación, puede consultar el clúster de proxy mediante las herramientas de Azure Data Explorer y referirse a él en una consulta entre clústeres. En este artículo se muestra cómo conectarse a un clúster de proxy, agregar un clúster de proxy a la interfaz de usuario web de Azure Data Explorer y ejecutar consultas con las áreas de trabajo de Log Analytics o las aplicaciones clásicas de Application Insights desde Azure Data Explorer.

En el diagrama siguiente se muestra el flujo del proxy de Azure Data Explorer:

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-monitor-proxy-flow.png" alt-text="Flujo del proxy de Azure Data Explorer.":::


> [!NOTE]
> El proxy de Azure Data Explorer está en versión preliminar pública. [Conéctese al proxy](#connect-to-the-proxy) para habilitar la característica de proxy para los clústeres. 

## <a name="connect-to-the-proxy"></a>Conexión con el proxy
Para conectar un área de trabajo de Log Analytics o una aplicación clásica de Application Insights, abra la [interfaz de usuario web de Azure Data Explorer](https://dataexplorer.azure.com/clusters). Compruebe que el clúster nativo de Azure Data Explorer (como el clúster *help* ) aparece en el menú izquierdo antes de conectarse al clúster de Log Analytics o de Application Insights.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-web-ui-help-cluster.png" alt-text="Clúster nativo de Azure Data Explorer.":::

Haga clic en **Agregar clúster** y, luego, agregue la dirección URL del clúster de Log Analytics o de Application Insights en uno de estos formatos. 
    
* Para Log Analytics: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
* Para Application Insights: `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

Haga clic en **Agregar** para establecer la conexión.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-add-cluster.png" alt-text="Agregue el clúster.":::
 
> [!NOTE]
> Si agrega una conexión a más de un clúster de proxy, dé a cada uno un nombre diferente. En caso contrario, todos tendrán el mismo nombre en el panel izquierdo.

Una vez que se establece la conexión, el clúster de Log Analytics o de Application Insights aparecerá en el panel de la izquierda con el clúster nativo de Azure Data Explorer. 

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-azure-data-explorer-clusters.png" alt-text="Clústeres de Log Analytics y de Azure Data Explorer.":::
 
> [!NOTE]
> El número de áreas de trabajo de Azure Monitor que se pueden asignar está limitado a 100.

## <a name="create-queries-using-azure-monitor-data"></a>Creación de consultas con datos de Azure Monitor

Puede ejecutar las consultas usando herramientas de cliente que admitan consultas de Kusto, como: Kusto Explorer, interfaz de usuario web de Azure Data Explorer, Jupyter Kqlmagic, Flow, PowerQuery, PowerShell, Jarvis, Lens y API REST.

> [!NOTE]
> La característica de proxy de Azure Data Explorer se usa solo para la recuperación de datos. Para más información, consulte el apartado [Compatibilidad con funciones](#function-supportability).

> [!TIP]
> * El nombre de la base de datos debe tener el mismo nombre que el recurso especificado en el clúster de proxy. Los nombres distinguen mayúsculas de minúsculas.
> * En consultas entre clústeres, asegúrese de que la nomenclatura de las aplicaciones de Application Insights y las áreas de trabajo de Log Analytics sea correcta.
>     * Si los nombres contienen caracteres especiales, se sustituyen por codificación URL en el nombre del clúster del proxy. 
>     * Si los nombres incluyen caracteres que no cumplen las [reglas del nombre del identificador KQL](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/entity-names), se sustituyen por el carácter de guion **-** .

### <a name="direct-query-from-your-log-analytics-or-application-insights-proxy-cluster"></a>Consulta directa desde el clúster del proxy de Log Analytics o de Application Insights

Ejecute consultas en el clúster de Log Analytics o de Application Insights. Compruebe que el clúster está seleccionado en el panel izquierdo. 
 
```kusto
Perf | take 10 // Demonstrate query through the proxy on the Log Analaytics workspace
```

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-query-la.png" alt-text="Consulte el área de trabajo de Log Analytics.":::

### <a name="cross-query-of-your-log-analytics-or-application-insights-proxy-cluster-and-the-azure-data-explorer-native-cluster"></a>Consulta cruzada del clúster del proxy de Log Analytics o de Application Insights y el clúster nativo de Azure Data Explorer

Al ejecutar consultas entre clústeres desde el proxy, compruebe que el clúster nativo de Azure Data Explorer está seleccionado en el panel izquierdo. En los ejemplos siguientes se muestra cómo combinar tablas de clúster de Azure Data Explorer mediante el operador [union](/azure/data-explorer/kusto/query/unionoperator) con un área de trabajo de Log Analytics.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <Azure Data Explorer table>, cluster(CL1).database(<workspace-name>).<table name>
```
Usar el operador [`join`](/azure/data-explorer/kusto/query/joinoperator?pivots=azuremonitor), en lugar de la unión, puede requerir una [sugerencia](/azure/data-explorer/kusto/query/joinoperator?pivots=azuremonitor#join-hints) para ejecutarlo en un clúster nativo de Azure Data Explorer (y no en el proxy). 

### <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Combinación de datos desde un clúster de Azure Data Explorer de un inquilino con un recurso de Azure Monitor de otro inquilino

Las consultas entre inquilinos no son compatibles con el proxy de Azure Data Explorer. Ha iniciado sesión en un solo inquilino para ejecutar la consulta que abarca ambos recursos.

Si el recurso de Azure Data Explorer está en el inquilino "A" y el área de trabajo de Log Analytics está en el inquilino "B", use uno de los dos métodos siguientes:

- Azure Data Explorer permite agregar roles para entidades de seguridad de distintos inquilinos. Agregue su identificador de usuario en el inquilino "B" como usuario autorizado en el clúster de Azure Data Explorer. Compruebe que la propiedad *["TrustedExternalTenant"](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)* del clúster de Azure Data Explorer contiene al inquilino "B". Ejecute la consulta cruzada completamente en el inquilino "B".

- Use [Lighthouse](/azure/lighthouse/) para proyectar el recurso de Azure Monitor en el inquilino "A".

### <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Conexión a clústeres de Azure Data Explorer desde otros inquilinos

Kusto Explorer inicia sesión automáticamente en el inquilino al que pertenece originalmente la cuenta de usuario. Para acceder a los recursos de otros inquilinos con la misma cuenta de usuario, se tiene que especificar explícitamente el valor de `tenantId` en la cadena de conexión: `Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=`**TenantId**

## <a name="function-supportability"></a>Compatibilidad con funciones

El clúster de proxy de Azure Data Explorer admite funciones tanto para Log Analytics como para Application Insights. Esta funcionalidad permite que las consultas entre clústeres hagan referencia directamente a una función tabular de Azure Monitor.

El proxy admite los siguientes comandos:

* `.show functions`
* `.show function {FunctionName}`
* `.show database {DatabaseName} schema as json`

En la imagen siguiente se muestra un ejemplo de cómo consultar una función tabular desde la interfaz de usuario web de Azure Data Explorer. Para usar la función, ejecute el nombre en la ventana Consulta.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-function-query.png" alt-text="Consulta de una función tabular desde la interfaz de usuario web de Azure Data Explorer.":::
 
> [!NOTE]
> Azure Monitor solo admite funciones tabulares, las que no admiten parámetros.

## <a name="additional-syntax-examples"></a>Ejemplos de sintaxis adicionales

Las opciones de sintaxis siguientes están disponibles cuando se llama a los clústeres de Log Analytics o de Application Insights:

|Descripción de la sintaxis  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Base de datos dentro de un clúster que contiene solo el recurso definido en esta suscripción ( **recomendado para las consultas entre clústeres** ) |   clúster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | clúster (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Clúster que contiene todas las aplicaciones o áreas de trabajo en esta suscripción    |     clúster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    clúster (`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Clúster que contiene todas las aplicaciones o áreas de trabajo de la suscripción y que son miembros de este grupo de recursos    |   clúster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    clúster (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Clúster que contiene solo el recurso definido en esta suscripción      |    clúster (`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  clúster (`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

## <a name="next-steps"></a>Pasos siguientes

- Lea más sobre la [estructura de datos de las áreas de trabajo de Log Analytics y Application Insights](data-platform-logs.md).
- Aprenda a [escribir consultas en Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/write-queries).