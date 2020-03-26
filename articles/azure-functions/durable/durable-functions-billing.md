---
title: 'Facturación de Durable Functions: Azure Functions'
description: Obtenga información sobre los comportamientos internos de Durable Functions y cómo afectan a la facturación de Azure Functions.
author: cgillum
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: 504ef93a0002895bc5662d95ad269c8593170ee2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "74233008"
---
# <a name="durable-functions-billing"></a>Facturación de Durable Functions

[Durable Functions](durable-functions-overview.md) se factura igual que Azure Functions. Para más información, consulte los [precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

Al ejecutar funciones de orquestador en el [plan de consumo](../functions-scale.md#consumption-plan) de Azure Functions, es preciso conocer algunos comportamientos de facturación. En las secciones siguientes se describen estos comportamientos y su efecto con más detalle.

## <a name="orchestrator-function-replay-billing"></a>Facturación de la reproducción de una función de orquestador

Las [funciones del orquestador](durable-functions-orchestrations.md) pueden reproducirse varias veces a lo largo de la duración de la orquestación. El entorno de ejecución de Azure Functions considera cada reproducción como una invocación de función distinta. Por esta razón, en el plan de consumo de Azure Functions se le facturan todas y cada una de las reproducciones de cualquier función del orquestador. Otros tipos de planes no cobran las reproducciones de las funciones del orquestador.

## <a name="awaiting-and-yielding-in-orchestrator-functions"></a>Espera y suspensión de funciones de orquestador

Cuando una función de orquestador espera a que se complete una acción asincrónica, para lo que usa **await** en C# o **yiedl**en JavaScript, el entorno de ejecución considera que se debe finalizar esa ejecución en particular. La facturación de la función del orquestador se detiene en ese punto. No se reanuda hasta la reproducción de la siguiente función del orquestador. No se le facturará por el tiempo dedicado a la espera o la suspensión de una función de orquestador.

> [!NOTE]
> Hay quienes consideran que el hecho de que unas funciones llamen a otras funciones es un antipatrón. Esto se debe a un problema conocido como _doble facturación_. Cuando una función llama directamente a otra, ambas se ejecutan al mismo tiempo. La función a la que se llama está ejecutando el código de forma activa mientras que la función que realiza la llamada está esperando una respuesta. En este caso, es preciso pagar por el tiempo que la función que realiza la llamada invierte en esperar a que se ejecute la función llamada.
>
> No hay doble facturación en las funciones del orquestador. La facturación de cualquiera de las funciones del orquestador se detiene mientras espera el resultado de una función de la actividad o de una suborquestación.

## <a name="durable-http-polling"></a>Sondeo HTTP duradero

Las funciones del orquestador pueden realizar llamadas HTTP de ejecución prolongada a puntos de conexión externos, como se describe en el [artículo acerca de las características de HTTP](durable-functions-http-features.md). Tanto el método **CallHttpAsync** de C# como el método **callHttp** de JavaScript pueden realizar el sondeo interno de un punto de conexión HTTP mientras sigue el [patrón 202 asincrónico](durable-functions-http-features.md#http-202-handling).

Actualmente no se realiza una facturación directa de las operaciones de sondeo de HTTP internas. Sin embargo, el sondeo interno puede provocar que la función del orquestado se reproduzca de forma periódica. Estas reproducciones internas de la función se facturarán de forma estándar.

## <a name="azure-storage-transactions"></a>Transacciones de Azure Storage

Durable Functions usa Azure Storage de forma predeterminada para conservar el estado, procesar los mensajes y administrar las particiones mediante concesiones de blobs. Al ser el propietario de esta cuenta de almacenamiento, los costos de las transacciones se facturan a su suscripción de Azure. Para más información acerca de los artefactos de Azure Storage que usa Durable Functions, consulte el [artículo acerca de las centrales de tareas](durable-functions-task-hubs.md).

Hay varios factores que contribuyen a los costos de Azure Storage reales en los que incurre la aplicación Durable Functions:

* Una sola aplicación de funciones está asociada a una única central de tareas, que comparte un conjunto de recursos de Azure Storage. Estos recursos los usan todas las instancias de Durable Functions de una aplicación de funciones. El número real de funciones de la aplicación de funciones no tiene ningún impacto en los costos de las transacciones de Azure Storage.
* Cada instancia de la aplicación de funciones sondea internamente varias colas de la cuenta de almacenamiento mediante un algoritmo de sondeo de retroceso exponencial. Las instancias de la aplicación inactivas sondean las colas con menor frecuencia que las aplicaciones activas, lo que reduce los costos de las transacciones. Para más información sobre el comportamiento del sondeo de colas de Durable Functions, consulte la [sección de sondeo de colas del artículo acerca del rendimiento y escalado](durable-functions-perf-and-scale.md#queue-polling).
* Si se ejecuta en el plan de consumo o en el plan Premium de Azure Functions, el [controlador de escala de Azure Functions](../functions-scale.md#how-the-consumption-and-premium-plans-work) sondea regularmente todas las colas de la central de tareas en segundo plano. Si una aplicación de funciones tiene una escala de ligera a moderada, estas colas solo las sondeará solo una instancia del controlador de escala. Si la aplicación de funciones se escala horizontalmente a un gran número de instancias, se pueden agregar más instancias del controlador de escala. Estas instancias adicionales pueden aumentar el costo total de las transacciones de colas.
* Cada instancia de la aplicación de funciones compite por un conjunto de concesiones de blobs. Estas instancias realizarán llamadas periódicas a Azure Blob service para renovar las concesiones retenidas o intentar adquirir nuevas concesiones. El número particiones configuradas en la central de tareas determina el número de concesiones de blobs. El escalado horizontal a un número mayor de instancias de la aplicación de funciones es probable que aumentará los costos de las transacciones de Azure Storage asociados a estas operaciones de concesión.

Puede encontrar más información sobre los precios de Azure Storage en el documento [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre los precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/)
