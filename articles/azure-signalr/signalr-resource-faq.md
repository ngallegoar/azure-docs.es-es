---
title: Preguntas más frecuentes sobre Azure SignalR Service
description: Acceda rápidamente a las preguntas frecuentes sobre Azure SignalR Service, acerca de la solución de problemas y de los escenarios de uso típico.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: dde11b6097dddb1568f5adfea811606214a9759e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "75891251"
---
# <a name="azure-signalr-service-faq"></a>Preguntas más frecuentes sobre Azure SignalR Service

## <a name="is-azure-signalr-service-ready-for-production-use"></a>¿Azure SignalR Service está listo para usarlo en el entorno de producción?

Sí.
Para ver el anuncio de disponibilidad general, consulte [Azure SignalR ya está disponible con carácter general](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) es totalmente compatible.

La compatibilidad con ASP.NET SignalR todavía está en *versión preliminar pública*. Este es un [ejemplo de código](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom).

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>La conexión de cliente se cierra con el mensaje de error "No hay ningún servidor disponible". ¿Qué significa?

Este error solo se produce cuando los clientes envían mensajes a SignalR Service.

Si no tiene ningún servidor de aplicaciones y solo usa la API REST de SignalR Service, este comportamiento es así **por naturaleza**.
En la arquitectura sin servidor, las conexiones cliente están en modo de **ESCUCHA** y no enviarán ningún mensaje a SignalR Service.
Lea más información en [API REST](./signalr-quickstart-rest-api.md).

Si tiene servidores de aplicaciones, este mensaje de error significa que no hay ningún servidor de aplicaciones conectado a la instancia de SignalR Service.

Las causas posibles son:
- No hay ningún servidor de aplicaciones conectado con SignalR Service. Compruebe los registros del servidor de aplicaciones para ver los posibles errores de conexión. Este caso es poco común en la configuración de alta disponibilidad con más de un servidor de aplicaciones.
- Hay problemas de conectividad con las instancias de SignalR Service. Este problema es transitorio y se recuperará automáticamente.
Si persiste durante más de una hora, [abra una incidencia en GitHub](https://github.com/Azure/azure-signalr/issues/new) o [cree una solicitud de soporte técnico en Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Cuando hay varios servidores de aplicaciones, ¿los mensajes del cliente se envían a todos los servidores o solo a uno de ellos?

Es una asignación uno a uno entre el cliente y el servidor de aplicaciones. Los mensajes de un cliente siempre se envían al mismo servidor de aplicaciones.

La asignación entre el cliente y el servidor de aplicaciones se mantendrá hasta que el cliente o el servidor de aplicaciones se desconecte.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Si uno de mis servidores de aplicación no funciona, ¿cómo puedo saberlo y recibir notificaciones?

SignalR Service supervisa los latidos de los servidores de aplicaciones.
Si no se reciben latidos durante un período de tiempo determinado, se considera que el servidor de aplicaciones está sin conexión. Se desconectarán todas las conexiones de cliente asignadas a este servidor de aplicaciones.

## <a name="why-does-my-custom-iuseridprovider-throw-exception-when-switching-from-aspnet-core-signalr--sdk-to-azure-signalr-service-sdk"></a>¿Por qué el `IUserIdProvider` personalizado genera una excepción al cambiar del SDK de ASP.NET Core SignalR al SDK de Azure SignalR Service?

El parámetro `HubConnectionContext context` es distinto entre el SDK de ASP.NET Core SignalR y el SDK de Azure SignalR Service cuando se llama a `IUserIdProvider`.

En ASP.NET Core SignalR, `HubConnectionContext context` es el contexto proveniente de la conexión de cliente física con valores válidos para todas las propiedades.

En el SDK de Azure SignalR Service, `HubConnectionContext context` es el contexto proveniente de la conexión de cliente lógica. La conexión de cliente física se conecta a la instancia de SignalR Service, por lo que solo se proporciona un número limitado de propiedades.

Por ahora, solo `HubConnectionContext.GetHttpContext()` y `HubConnectionContext.User` están disponibles para el acceso.
Puede revisar el código de origen [aquí](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/HubHost/ServiceHubConnectionContext.cs).

## <a name="can-i-configure-the-transports-available-in-signalr-service-as-configuring-it-on-server-side-with-aspnet-core-signalr-for-example-disable-websocket-transport"></a>¿Puedo configurar los transportes disponibles en SignalR Service como si lo configurará en el lado servidor con ASP.NET Core SignalR? Por ejemplo, ¿puedo deshabilitar el transporte de WebSocket?

No.

Azure SignalR Service proporciona los tres transportes que ASP.NET Core SignalR Service admite de manera predeterminada. No se puede configurar. SignalR Service controlará las conexiones y los transportes para todas las conexiones de cliente.

Puede configurar los transportes del lado cliente como se documenta [aquí](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1&tabs=dotnet#configure-allowed-transports-2).
