---
title: Información general sobre Azure Functions
description: Obtenga información acerca de cómo Azure Functions puede ayudar a compilar aplicaciones sin servidor escalables.
author: craigshoemaker
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 11/20/2020
ms.author: cshoe
ms.custom: contperfq2
ms.openlocfilehash: 514f2e9a82a50f95f9c054c6a54e7b5af3c0af15
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96167784"
---
# <a name="introduction-to-azure-functions"></a>Introducción a Azure Functions

A menudo, se crean sistemas para que reaccionen a una serie de eventos críticos. Independientemente de si compila una API web, responde a cambios en una base de datos, procesa flujos de datos de IoT o incluso si administra colas de mensajes, cada aplicación necesita una forma de ejecutar código a medida que se producen estos eventos.

Para ello, Azure Functions ofrece proceso a petición de dos maneras significativas.

En primer lugar, Azure Functions permite implementar la lógica del sistema en bloques de código fácilmente disponibles. Estos bloques de código se denominan "funciones". Se pueden ejecutar distintas funciones cada vez que necesite responder a eventos críticos.

En segundo lugar, a medida que aumentan las solicitudes, Azure Functions satisface la demanda con tantos recursos e instancias de función como se necesiten, pero solo cuando sea necesario. A medida que disminuyan las solicitudes, todos los recursos e instancias de la aplicación adicionales se descartarán automáticamente.

¿De dónde proceden todos los recursos de proceso? Azure Functions [proporciona tantos recursos de proceso como sea necesario](./functions-scale.md) para satisfacer la demanda de la aplicación.

Proporcionar recursos de proceso a petición es la esencia de la [informática sin servidor](https://azure.microsoft.com/solutions/serverless/) en Azure Functions.

## <a name="scenarios"></a>Escenarios

En muchos casos, una función [se integra con una matriz de servicios en la nube](./functions-triggers-bindings.md) para proporcionar implementaciones con numerosas características.

A continuación se incluye un conjunto de escenarios habituales de Azure Functions, _si bien no están reflejadas todas las posibilidades_.

| Si desea... | entonces... |
| --- | --- |
| **Crear una API web** | Implemente un punto de conexión para las aplicaciones web mediante el [desencadenador HTTP](./functions-bindings-http-webhook.md) |
| **Procesar cargas de archivos** | Ejecute código cuando se cargue o se cambie un archivo en el [almacenamiento de blobs](./functions-bindings-storage-blob.md) |
| **Compilar un flujo de trabajo sin servidor** | Encadene una serie de funciones mediante [Durable Functions](./durable/durable-functions-overview.md) |
| **Responder a cambios en una base de datos** | Ejecute una lógica personalizada cuando se cree o actualice un documento en [Cosmos DB](./functions-bindings-cosmosdb-v2.md) |
| **Ejecutar tareas programadas** | Ejecute código a [horas establecidas](./functions-bindings-timer.md) |
| **Crear sistemas de cola de mensajes confiables** | Procese colas de mensajes mediante [Queue Storage](./functions-bindings-storage-queue.md), [Service Bus](./functions-bindings-service-bus.md) o [Event Hubs](./functions-bindings-event-hubs.md) |
| **Analizar flujos de datos de IoT** | Recopile y procese [datos de dispositivos IoT](./functions-bindings-event-iot.md) |
| **Procesar datos en tiempo real** | Use [Functions y Signal R](./functions-bindings-signalr-service.md) para responder a datos en el momento |

Al compilar las funciones, dispone de las siguientes opciones y recursos:

- **Usar el lenguaje que prefiera**: escriba funciones en [C#, Java, JavaScript, PowerShell o Python](./supported-languages.md), o use un [controlador personalizado](./functions-custom-handlers.md) para usar prácticamente cualquier otro lenguaje.

- **Automatizar la implementación**: desde un enfoque basado en herramientas hasta el uso de canalizaciones externas, dispone de una [enorme cantidad de opciones de implementación](./functions-deployment-technologies.md).

- **Solucionar problemas de una función**: use [herramientas de supervisión](./functions-monitoring.md) y [estrategias de pruebas](./functions-test-a-function.md) para obtener información sobre las aplicaciones.

- **Opciones de precios flexibles**: con el plan de [Consumo](./pricing.md), solo pagará mientras se ejecuten las funciones, mientras que los planes [Premium](./pricing.md) y [App Service](./pricing.md) ofrecen características para necesidades especializadas.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción con lecciones, ejemplos y tutoriales interactivos](./functions-get-started.md)