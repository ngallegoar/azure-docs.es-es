---
title: Información general sobre Azure Functions
description: Entender cómo utilizar Azure Functions para optimizar las cargas de trabajo asincrónicas en minutos.
author: mattchenderson
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 01/16/2020
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f0948cb58ba9403a34fbfd61ec43c29bed3440bc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77621001"
---
# <a name="an-introduction-to-azure-functions"></a>Introducción a Azure Functions

Azure Functions permite ejecutar pequeños fragmentos de código (denominados "funciones") sin preocuparse por la infraestructura de la aplicación. Con Azure Functions, la infraestructura de la nube proporciona todos los servidores actualizados que necesita para mantener la aplicación en ejecución a gran escala.

Una función se "desencadena" mediante un tipo específico de evento. Los [desencadenadores admitidos](./functions-triggers-bindings.md) incluyen responder a los cambios en los datos, responder a los mensajes, ejecutarse según una programación o como resultado de una solicitud HTTP.

Aunque siempre puede programar directamente en una gran cantidad de servicios, la integración con otros servicios se simplifica mediante el uso de enlaces. Los enlaces proporcionan [acceso declarativo a una gran variedad de servicios de Azure y de terceros](./functions-triggers-bindings.md).

## <a name="features"></a>Características

Algunas características clave de Azure Functions son las siguientes:

- **Aplicaciones sin servidor**: Functions permite desarrollar aplicaciones [sin servidor](https://azure.microsoft.com/solutions/serverless/) en Microsoft Azure.

- **Elección de idioma**: escriba funciones usando el lenguaje [C#, Java, JavaScript, Python y PowerShell](supported-languages.md) de su elección.

- **Modelo de precios de pago por uso**: pague solo el tiempo que haya empleado ejecutando el código. Consulte la opción del plan de hospedaje de Consumo en la [sección de precios](#pricing).  

- **Traiga sus propias dependencias**: Functions admite NuGet y NPM, lo que le proporciona acceso a sus bibliotecas favoritas.

- **Seguridad integrada** proteja las funciones desencadenadas por HTTP con los proveedores de OAuth como Azure Active Directory, Facebook, Google, Twitter y la cuenta de Microsoft.

- **Integración simplificada**: integración sencilla con los servicios de Azure y ofertas de software como servicio (SaaS).

- **Desarrollo flexible**: Configure la integración continua e implemente el código mediante [GitHub](../app-service/scripts/cli-continuous-deployment-github.md), [Azure DevOps Services](../app-service/scripts/cli-continuous-deployment-vsts.md) y otras [herramientas de desarrollo compatibles](../app-service/deploy-local-git.md).

- **Arquitectura sin servidor con estado**: organice aplicaciones sin servidor con [Durable Functions](durable/durable-functions-overview.md).

- **Código abierto** El entorno de ejecución de Functions es de código abierto y está [disponible en GitHub](https://github.com/azure/azure-webjobs-sdk-script).

## <a name="what-can-i-do-with-functions"></a>¿Qué puedo hacer con las funciones?

Azure Functions es una excelente solución para procesar datos de forma masiva, integrar sistemas, trabajar con Internet de las cosas (IoT) y generar API simples y microservicios.

Hay una variedad de plantillas que le ayudarán a empezar a trabajar con escenarios clave, como los siguientes:

- **HTTP**: ejecute código basado en [solicitudes HTTP ](functions-create-first-azure-function.md).

- **Temporizador**: programe código para que se [ejecute en horas predefinidas](./functions-create-scheduled-function.md).

- **Azure Cosmos DB**: procese [documentos Azure Cosmos DB nuevos y modificados](./functions-create-cosmos-db-triggered-function.md).

- **Blob Storage**: procese [blobs de Azure Storage nuevos y modificados](./functions-create-storage-blob-triggered-function.md).

- **Queue Storage**: responda a [mensajes de una cola de Azure Storage](./functions-create-storage-queue-triggered-function.md).

- **Event Grid**: responda a [eventos de Azure Event Grid a través de suscripciones y filtros](../event-grid/resize-images-on-storage-blob-upload-event.md).

- **Centro de eventos**: responda a [grandes volúmenes de eventos del centro de eventos de Azure](./functions-bindings-event-hubs.md).

- **Cola de Service Bus**: conéctese a otros servicios locales o de Azure mediante [la respuesta a los mensajes de cola de Service Bus](./functions-bindings-service-bus.md).

- **Tema de Service Bus**: conéctese a otros servicios locales o de Azure mediante [la respuesta a los mensajes de tema de Service Bus](./functions-bindings-service-bus.md).

## <a name="how-much-does-functions-cost"></a><a name="pricing"></a>¿Cuánto cuesta Funciones de Azure?

Azure Functions tiene tres tipos de planes de precios. Elija la que mejor se adapte a sus necesidades:

- **Plan de consumo**: Azure proporciona todos los recursos de cálculo necesarios. No tiene que preocuparse de la administración de recursos y solo paga por el tiempo que haya empleado en la ejecución del código.

- **Plan Premium**: especifique un número de instancias activadas previamente que siempre están en línea y preparadas para responder de inmediato. Cuando se ejecuta la función, Azure proporciona todos los recursos informáticos adicionales que sean necesarios. Se paga tanto por las instancias activadas previamente que se ejecutan de forma continua como por todas las instancias adicionales que se usen cuando Azure reduce y escala horizontalmente la aplicación.

- **Plan de App Service**: se ejecutan las funciones igual que aplicaciones web. Si ya usa App Service para las otras aplicaciones, las funciones pueden ejecutarse en el mismo plan sin costo adicional.

Para más información sobre planes de hospedaje, vea [Comparación de los planes de hospedaje de Azure Functions](functions-scale.md). Puede encontrar todos los detalles de precios en la [página de Precios de Funciones](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Pasos siguientes

- [Creación de su primera función de Azure](functions-create-first-function-vs-code.md)  
  Comience a usar [Visual Studio Code](functions-create-first-function-vs-code.md), la [línea de comandos](functions-create-first-azure-function-azure-cli.md) o use [Azure Portal](functions-create-first-azure-function.md).

- [Referencia para desarrolladores de Azure Functions](functions-reference.md)  
  Proporciona información técnica sobre el tiempo de ejecución de Azure Functions y una referencia para las funciones de codificación y la definición de enlaces y desencadenadores.

- [How to scale Azure Functions](functions-scale.md)  
  Trata los planes de servicio disponibles con Azure Functions, incluido el plan de hospedaje de Consumo, y cómo elegir el plan adecuado.

- [¿Qué es Azure App Service?](../app-service/overview.md)  
  Azure Functions aprovecha Azure App Service para obtener una funcionalidad básica como son las implementaciones, las variables de entorno y los diagnósticos.
