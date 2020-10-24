---
author: ggailey777
ms.author: glenga
ms.date: 7/24/2019
ms.topic: include
ms.service: azure-functions
ms.openlocfilehash: 0159ceb6e5d6d64a7a9bda383396607e4ce05b84
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92164878"
---
#### <a name="built-in-log-streaming"></a>Streaming integrado de registros

Use la opción `logstream` para empezar a recibir registros de streaming de una aplicación de funciones específica que se ejecuta en Azure, como en el ejemplo siguiente:

```bash
func azure functionapp logstream <FunctionAppName>
```

>[!NOTE]
>El streaming de registro integrado aún no se ha habilitado en Core Tools para las aplicaciones de funciones que se ejecutan en Linux en un plan de consumo. En estos planes de hospedaje, es preciso usar Live Metrics Stream para ver los registros casi en tiempo real.

#### <a name="live-metrics-stream"></a>Secuencia de métricas en directo

La información de [Live Metrics Stream](../articles/azure-monitor/app/live-stream.md) de una aplicación de funciones se puede ver en una ventana nueva del explorador. Para ello, hay que incluir la opción `--browser`, como se hace en el ejemplo siguiente:

```bash
func azure functionapp logstream <FunctionAppName> --browser
```
