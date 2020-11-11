---
title: Guía de reintentos y control de errores de Azure Functions
description: Aprenda a controlar los errores y los eventos de reintento en Azure Functions con vínculos a errores de enlace específicos.
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/01/2020
ms.author: cshoe
ms.openlocfilehash: e52babd5b0612b4590f1422a941b25ba30c3f169
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93284461"
---
# <a name="azure-functions-error-handling-and-retries"></a>Reintentos y control de errores de Azure Functions

El control de los errores en Azure Functions es importante para evitar la pérdida de datos, eventos perdidos y para supervisar el estado de la aplicación.

En este artículo se describen las estrategias generales para el control de errores junto con vínculos a errores específicos de enlace.

## <a name="handling-errors"></a>Control de errores

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-retries.md)]

## <a name="binding-error-codes"></a>Códigos de error de enlace

Cuando se realizan integraciones con los servicios de Azure, pueden originarse errores desde las API de los servicios subyacentes. La información relacionada con los errores específicos de enlace está disponible en la sección **Excepciones y códigos de retorno** de los siguientes artículos:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs-output.md#exceptions-and-return-codes)

+ [IoT Hubs](functions-bindings-event-iot-output.md#exceptions-and-return-codes)

+ [Centros de notificaciones](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue-output.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus-output.md#exceptions-and-return-codes)

+ [Table storage](functions-bindings-storage-table-output.md#exceptions-and-return-codes)
