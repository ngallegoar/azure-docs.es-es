---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.custom: fasttrack-edit
ms.openlocfilehash: 819eed1eb4e32469bf0572a6548f5a2a44091ef0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77279636"
---
En esta tabla se muestran los enlaces que son compatibles con las versiones principales del entorno en tiempo de ejecución de Azure Functions:


| Tipo | 1.x | 2.x y posteriores<sup>1</sup> | Desencadenador | Entrada | Output |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|
| [Event Grid](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| |✔|
| [Event Hubs](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP y webhooks](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [IoT Hub](../articles/azure-functions/functions-bindings-event-iot.md)             |✔|✔|✔| |✔|
| [Microsoft Graph<br/>Tablas de Excel](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔| |✔|✔|
| [Microsoft Graph<br/>Archivos de OneDrive](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔| |✔|✔|
| [Microsoft Graph<br/>Correo electrónico de Outlook](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔| | |✔|
| [Eventos de<br/>Microsoft Graph](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔|✔|✔|✔|
| [Microsoft Graph<br/>Tokens de autenticación](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔| |✔| |
| [Mobile Apps](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [Centros de notificaciones](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [SignalR](../articles/azure-functions/functions-bindings-signalr-service.md)             | |✔| |✔|✔|
| [Table storage](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [Temporizador](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |
| [Twilio](../articles/azure-functions/functions-bindings-twilio.md)                       |✔|✔| | |✔|

<sup>1</sup> A partir del entorno en tiempo de ejecución de la versión 2.x, se deben registrar todos los enlaces, excepto HTTP y el temporizador. Consulte [Registro de extensiones de enlace](../articles/azure-functions/functions-bindings-register.md).
