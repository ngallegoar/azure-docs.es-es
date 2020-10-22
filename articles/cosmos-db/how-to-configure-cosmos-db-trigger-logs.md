---
title: Configuración y lectura de los registros con el desencadenador de Azure Functions para Cosmos DB
description: Aprenda a exponer los registros a la canalización de registros de Azure Functions cuando se usa el desencadenar de Azure Functions para Cosmos DB.
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 69a8f44831f1af13158261bedb19a254c6a565a6
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92165305"
---
# <a name="how-to-configure-and-read-the-logs-when-using-azure-functions-trigger-for-cosmos-db"></a>Configuración y lectura de los registros cuando se usa el desencadenador de Azure Functions para Cosmos DB

En este artículo se describe cómo se puede configurar el entorno de Azure Functions para enviar los registros del desencadenador de Azure Functions para Cosmos DB a la [solución de supervisión](../azure-functions/functions-monitoring.md) configurada.

## <a name="included-logs"></a>Registros incluidos

El desencadenador de Azure Functions para Cosmos DB utiliza la [biblioteca de procesadores de fuente de cambios](./change-feed-processor.md) internamente, y esta genera un conjunto de registros de mantenimiento que se pueden usar para supervisar las operaciones internas con [fines de solución de problemas](./troubleshoot-changefeed-functions.md).

Los registros de mantenimiento describen cómo se comporta el desencadenador de Azure Functions para Cosmos DB cuando se intentan realizar operaciones durante escenarios de equilibrio de carga o de inicialización.

## <a name="enabling-logging"></a>Habilitación del registro

Para habilitar el registro cuando se usa el desencadenador de Azure Functions para Cosmos DB, busque el archivo `host.json` en el proyecto de Azure Functions o en la aplicación de Azure Functions y [configure el nivel de registro necesario](../azure-functions/configure-monitoring.md#configure-log-levels). Tendrá que habilitar el seguimiento de `Host.Triggers.CosmosDB`, tal como se muestra en el ejemplo siguiente:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Después de implementar la función de Azure con la configuración actualizada, verá los registros del desencadenador de Azure Functions para Cosmos DB como parte de los seguimientos. Puede ver los registros en el proveedor de registro configurado en la *categoría* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Consulta de los registros

Ejecute la siguiente consulta para obtener los registros generados por el desencadenador de Azure Functions para Cosmos DB en [Analytics de Azure Application Insights](../azure-monitor/app/analytics.md):

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Pasos siguientes

* [Habilite la supervisión](../azure-functions/functions-monitoring.md) en las aplicaciones de Azure Functions.
* Aprenda a [diagnosticar y solucionar problemas comunes](./troubleshoot-changefeed-functions.md) cuando se usa el desencadenador de Azure Functions para Cosmos DB.
