---
title: 'Creación de aplicaciones en tiempo real: Azure Functions y Azure SignalR Service'
description: Obtenga información sobre cómo desarrollar una aplicación web sin servidor en tiempo real con Azure SignalR Service con el ejemplo siguiente.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: cbb1fcf320a78f11045bf9627ffcc438af3e388a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74157609"
---
# <a name="build-real-time-apps-with-azure-functions-and-azure-signalr-service"></a>Creación de aplicaciones en tiempo real con Azure Functions y Azure SignalR Service

Dado que Azure SignalR Service y Azure Functions son servicios altamente escalables y totalmente administrados que le permiten concentrarse en crear aplicaciones en lugar de tener que administrar la infraestructura, es normal usar los dos servicios para proporcionar comunicaciones en tiempo real en un entorno [sin servidores](https://azure.microsoft.com/solutions/serverless/).

> [!NOTE]
> Aprenda a usar SignalR y Azure Functions juntos en el tutorial interactivo [Habilitar las actualizaciones automáticas en una aplicación web mediante Azure Functions y SignalR Service](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr).

## <a name="integrate-real-time-communications-with-azure-services"></a>Integrar las comunicaciones en tiempo real con los servicios de Azure

Azure Functions le permite escribir código en [varios lenguajes](../azure-functions/supported-languages.md), incluyendo JavaScript, Python, C# y Java, y se activa cada vez que se producen eventos en la nube. Los ejemplos de estos eventos incluyen:

* Solicitudes de HTTP y webhook
* Temporizadores periódicos
* Eventos de los servicios de Azure, como:
    - Event Grid
    - Event Hubs
    - Azure Service Bus
    - Fuente de cambios de Cosmos DB
    - Storage: blobs y colas
    - Conectores de Logic Apps como Salesforce y SQL Server

Al usar Azure Functions para integrar estos eventos con Azure SignalR Service, tiene la capacidad de enviar notificaciones a miles de clientes cada vez que se produce cualquier evento.

Estos son algunos escenarios comunes de la mensajería sin servidor en tiempo real que puede implementar con Azure Functions y el servicio SignalR incluyen:

* Visualizar la telemetría del dispositivo IoT en un panel o mapa en tiempo real.
* Actualizar datos en una aplicación cuando se actualicen documentos en Cosmos DB.
* Enviar notificaciones en aplicación cuando se creen nuevos pedidos en Salesforce.

## <a name="signalr-service-bindings-for-azure-functions"></a>Enlaces de SignalR Service para Azure Functions

Los enlaces de SignalR Service para Azure Functions permiten que una aplicación de Azure Functions publique mensajes en los clientes conectados a SignalR Service. Los clientes pueden conectarse al servicio mediante un SDK de cliente de SignalR que esté disponible en .NET, JavaScript y Java (tendrá más lenguajes disponibles en breve).

### <a name="an-example-scenario"></a>Escenario de ejemplo

Un ejemplo de cómo utilizar los enlaces de SignalR Service, es usar Azure Functions para integrarlo con Azure Cosmos DB y SignalR Service y así enviar mensajes en tiempo real cuando aparezcan nuevos eventos en una fuente de cambios de Cosmos DB.

![Cosmos DB, Azure Functions, SignalR Service](media/signalr-concept-azure-functions/signalr-cosmosdb-functions.png)

1. Se realiza un cambio en una colección de Cosmos DB.
2. El evento de cambio se propaga a la fuente de cambios de Cosmos DB.
3. El evento de cambios desencadena una instancia de Azure Functions mediante el desencadenador de Cosmos DB.
4. El enlace de salida de SignalR Service publica un mensaje en SignalR Service.
5. SignalR Service publica el mensaje para todos los clientes conectados.

### <a name="authentication-and-users"></a>Autenticación y usuarios

SignalR Service le permite difundir mensajes a todos los clientes o a un solo subconjunto de clientes, como los que pertenecen a un único usuario. Los enlaces de SignalR Service de Azure Functions se pueden combinar con la autenticación de App Service, y así poder autenticar a los usuarios que tengan proveedores como Azure Active Directory, Facebook y Twitter. Entonces podrá enviar mensajes directamente a estos usuarios autenticados.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, se obtuvo información general de cómo usar Azure Functions con SignalR Service para habilitar una amplia gama de escenarios de mensajes en tiempo real sin servidor.

Para más información sobre cómo usar Azure Functions y SignalR Service juntos, consulte los siguientes recursos:

* [Desarrollo y configuración de Azure Functions con SignalR Service](signalr-concept-serverless-development-config.md)
* [Habilitar las actualizaciones automáticas en una aplicación web mediante Azure Functions y SignalR Service](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr)

Siga una de estas guías de inicio rápido para obtener más información.

* [Azure SignalR Service Serverless Quickstart - C#](signalr-quickstart-azure-functions-csharp.md) (Guía de inicio rápido de Azure SignalR Service sin servidor: C#).
* [Azure SignalR Service Serverless Quickstart - JavaScript](signalr-quickstart-azure-functions-javascript.md) (Guía de inicio rápido de Azure SignalR Service sin servidor: JavaScript).
