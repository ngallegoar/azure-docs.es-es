---
title: Prueba unitaria de Azure Durable Functions
description: Obtenga información sobre cómo ejecutar una prueba unitaria de Durable Functions.
ms.topic: conceptual
ms.date: 11/03/2019
ms.openlocfilehash: 7786a0a2e2d31086e1938b70e63fe2374e16fe7f
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96601363"
---
# <a name="durable-functions-unit-testing"></a>Prueba unitaria de Durable Functions

La prueba unitaria es parte importante de las prácticas modernas de desarrollo de software. Las pruebas unitarias comprueban el comportamiento de la lógica de negocios e impiden los cambios importantes desapercibidos en el futuro. Durable Functions puede aumentar fácilmente su complejidad, por lo que realizar pruebas unitarias ayudará a evitar los cambios importantes. En las secciones siguientes se explica cómo ejecutar una prueba unitaria de los tres tipos de función: funciones de cliente de orquestación, de orquestador y de actividad.

> [!NOTE]
> En este artículo se ofrecen instrucciones sobre pruebas unitarias para aplicaciones de Durable Functions que tengan como destino Durable Functions 1.x. Todavía no se ha actualizado para tener en cuenta los cambios introducidos en Durable Functions 2.x. Para obtener más información sobre las diferencias entre versiones, vea el artículo [Versiones de Durable Functions](durable-functions-versions.md).

## <a name="prerequisites"></a>Prerequisites

Los ejemplos que aparecen en este artículo requieren conocer los siguientes conceptos y marcos:

* Pruebas unitarias

* Funciones duraderas

* [xUnit](https://github.com/xunit/xunit): marco de pruebas

* [moq](https://github.com/moq/moq4): marco de simulación

## <a name="base-classes-for-mocking"></a>Clases base para simulación

La simulación se admite a través de tres clases abstractas en Durable Functions 1.x:

* `DurableOrchestrationClientBase`

* `DurableOrchestrationContextBase`

* `DurableActivityContextBase`

Estas clases son clases base para `DurableOrchestrationClient`, `DurableOrchestrationContext` y `DurableActivityContext` que definen los métodos de cliente de orquestación, de orquestador y de actividad. Las simulaciones establecerán el comportamiento esperado para los métodos de clase base, de manera que la prueba unitaria pueda comprobar la lógica de negocios. Hay un flujo de trabajo de dos pasos para realizar una prueba unitaria de la lógica de negocios en el cliente de Orchestration y Orchestrator:

1. Use las clases base en lugar de la implementación concreta cuando defina las firmas de las funciones de cliente de orquestación y de orquestador.
2. En las pruebas unitarias, simule el comportamiento de las clases base y compruebe la lógica de negocios.

Encuentre más detalles en los párrafos siguientes para probar las funciones que usan el enlace del cliente de Orchestration y el enlace de desencadenador de Orchestrator.

## <a name="unit-testing-trigger-functions"></a>Funciones de desencadenador de prueba unitaria

En esta sección, la prueba unitaria validará la lógica de la función de desencadenador HTTP siguiente para iniciar nuevas orquestaciones.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

La tarea de prueba unitaria será comprobar el valor del encabezado `Retry-After` proporcionado en la carga útil de la respuesta. Por lo tanto, la prueba unitaria simulará algunos de los métodos de `DurableOrchestrationClientBase` para garantizar un comportamiento predecible.

En primer lugar, se requiere una simulación de la clase base, `DurableOrchestrationClientBase`. La simulación puede ser una nueva clase que implemente `DurableOrchestrationClientBase`. Sin embargo, usar un marco de simulación como [moq](https://github.com/moq/moq4) simplifica el proceso:

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Luego, se simula el método `StartNewAsync` para devolver un identificador de instancia conocido.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

A continuación, se simula `CreateCheckStatusResponse` para devolver siempre una respuesta HTTP 200 vacía.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
            Headers =
            {
                RetryAfter = new RetryConditionHeaderValue(TimeSpan.FromSeconds(10))
            }
        });
```

También se simula `ILogger`:

```csharp
    // Mock ILogger
    var loggerMock = new Mock<ILogger>();
```  

Ahora, se llama al método `Run` desde la prueba unitaria:

```csharp
    // Call Orchestration trigger function
    var result = await HttpStart.Run(
        new HttpRequestMessage()
        {
            Content = new StringContent("{}", Encoding.UTF8, "application/json"),
            RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
        },
        durableOrchestrationClientBaseMock.Object,
        functionName,
        loggerMock.Object);
 ```

 El último paso es comparar la salida con el valor esperado:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Después de combinar todos los pasos, la prueba unitaria tendrá el código siguiente:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Funciones de Orchestrator de prueba unitaria

Las funciones de Orchestrator son incluso más interesantes para la prueba unitaria porque habitualmente tienen mucha más lógica de negocios.

En esta sección, las pruebas unitarias validarán la salida de la función `E1_HelloSequence` de Orchestrator:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

El código de la prueba unitaria comenzará con la creación de una simulación:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Luego, se simularán las llamadas al método de la actividad:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

A continuación, la prueba unitaria llamará al método `HelloSequence.Run`:

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

Finalmente, se validará la salida:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Después de combinar todos los pasos, la prueba unitaria tendrá el código siguiente:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Funciones de la actividad de prueba unitaria

Es posible realizar una prueba unitaria de las funciones de actividad del mismo modo que las funciones no duraderas.

En esta sección, la prueba unitaria validará el comportamiento de la actividad de la función `E1_SayHello`:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Y las pruebas unitarias comprobarán el formato de la salida. Las pruebas unitarias pueden utilizar los tipos de parámetro directamente o simular la clase `DurableActivityContextBase`:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre xUnit](https://xunit.net/docs/getting-started/netcore/cmdline)
> 
> [Más información sobre moq](https://github.com/Moq/moq4/wiki/Quickstart)
