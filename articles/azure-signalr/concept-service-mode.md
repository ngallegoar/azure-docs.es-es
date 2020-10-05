---
title: Modo de servicio en Azure SignalR Service
description: Información general de los distintos modos de servicio de Azure SignalR Service, explicación de sus diferencias y escenarios de usuario aplicables
author: chenkennt
ms.service: signalr
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: kenchen
ms.openlocfilehash: 2cdce64b0ff03521a5848e2b4fd6e01431b5cc16
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514750"
---
# <a name="service-mode-in-azure-signalr-service"></a>Modo de servicio en Azure SignalR Service

El modo de servicio es un concepto importante en Azure SignalR Service. Cuando cree un recurso de SignalR, se le pedirá que especifique un modo de servicio:

:::image type="content" source="media/concept-service-mode/create.png" alt-text="Selección del modo de servicio al crear":::

También puede cambiarlo más adelante en el menú de configuración:

:::image type="content" source="media/concept-service-mode/update.png" alt-text="Selección del modo de servicio al crear":::

Azure SignalR Service admite actualmente tres modos de servicio: **predeterminado**, **sin servidor** y **clásico**. El recurso de SignalR se comportará de forma diferente según el modo. En este artículo, aprenderá sus diferencias y cómo elegir el modo de servicio adecuado en función de su escenario.

## <a name="default-mode"></a>Modo predeterminado

El modo predeterminado es el modo de servicio predeterminado cuando se crea un recurso de SignalR. En este modo, la aplicación funciona como una aplicación de SignalR de ASP.NET Core (o ASP.NET) típica, donde hay un servidor web que hospeda un centro (en adelante, servidor concentrador) y los clientes pueden tener una comunicación dúplex en tiempo real con el servidor concentrador. La única diferencia es que, en lugar de conectar directamente el cliente y el servidor, ambos se conectan al servicio SignalR y usan el servicio como proxy. A continuación, se muestra un diagrama que ilustra la estructura de aplicaciones habitual en el modo predeterminado:

:::image type="content" source="media/concept-service-mode/default.png" alt-text="Selección del modo de servicio al crear":::

Por lo tanto, si tiene una aplicación SignalR y desea integrarla con el servicio SignalR, el modo predeterminado será la opción adecuada en la mayoría de los casos.

### <a name="connection-routing-in-default-mode"></a>Enrutamiento de conexión en modo predeterminado

En modo predeterminado, habrá conexiones de WebSocket (llamadas conexiones de servidor) entre el servidor concentrador y el servicio SignalR. Estas conexiones se utilizan para transferir mensajes entre el servidor y el cliente. Cuando se conecte un nuevo cliente, el servicio SignalR lo enrutará a un servidor concentrador (suponiendo que tenga más de un servidor) mediante las conexiones de servidor existentes. Luego, la conexión de cliente se adherirá al mismo servidor concentrador durante su vigencia. Cuando el cliente envía mensajes, siempre se dirigen al mismo servidor concentrador. Con este comportamiento, puede mantener de forma segura algunos estados para las conexiones individuales en el servidor concentrador. Por ejemplo, si quiere transmitir algo entre el servidor y el cliente, no es necesario considerar el caso de que los paquetes de datos vayan a servidores diferentes.

> [!IMPORTANT]
> También significa que, en el modo predeterminado, el cliente no se puede conectar sin que el servidor se conecte primero. Si todos los servidores concentradores se desconectan debido a una interrupción de la red o al reinicio del servidor, la conexión de cliente recibirá un error que indica que no hay ningún servidor conectado. Por lo tanto, es responsabilidad suya que, en cualquier momento, haya al menos un servidor concentrador conectado al servicio SignalR (por ejemplo, puede tener varios servidores concentradores y comprobar que no se desconectarán al mismo tiempo durante tareas como el mantenimiento).

Este modelo de enrutamiento también significa que, cuando un servidor concentrador se queda sin conexión, las conexiones enrutadas a ese servidor se anularán. Por lo tanto, debe prever que la conexión se anule cuando el servidor concentrador esté sin conexión durante el mantenimiento y administrar la reconexión correctamente para que no tenga un efecto negativo en la aplicación.

## <a name="serverless-mode"></a>Modo sin servidor

El modo sin servidor, como su nombre indica, es un modo que no puede tener ningún servidor concentrador. En comparación con el modo predeterminado, en este modo, el cliente no requiere que el servidor concentrador se conecte. Todas las conexiones se realizan al servicio en modo "sin servidor" y el servicio es responsable de mantener las conexiones de cliente, como la administración de los pings de cliente (en el modo predeterminado, se encargan de ello los servidores concentradores).

Además, en este modo, no hay ninguna conexión de servidor (si intenta usar el SDK de servicio para establecer la conexión de servidor, recibirá un error). Por lo tanto, tampoco hay ningún enrutamiento de conexión ni adherencia cliente-servidor (como se describe en la sección del modo predeterminado). Sin embargo, puede seguir teniendo la aplicación del lado servidor para insertar mensajes en los clientes. Para ello, tiene dos opciones: usar las [API REST](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) para el envío de una sola vez, o bien, una conexión de WebSocket para que pueda enviar varios mensajes de manera más eficaz (tenga en cuenta que esta conexión de WebSocket es diferente de la conexión de servidor).

> [!NOTE]
> Tanto la opción API REST como WebSocket se admiten en el [SDK de administración](https://github.com/Azure/azure-signalr/blob/dev/docs/management-sdk-guide.md) del servicio SignalR. Si utiliza un lenguaje distinto de .NET, también puede invocar manualmente las API REST que siguen a esta [especificación](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md).
>
> Si utiliza Azure Functions, puede usar los [enlaces del servicio SignalR para Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-bindings-signalr-service) (en adelante, enlace de función) para enviar mensajes como un enlace de salida.

También es posible que la aplicación de servidor reciba mensajes y eventos de conexión de los clientes. El servicio entregará los mensajes y los eventos de conexión en los puntos de conexión preconfigurados (llamados ascendentes) mediante webhooks. En comparación con el modo predeterminado, no hay ninguna garantía de permanencia y las solicitudes HTTP pueden ser menos eficientes que las conexiones de WebSocket.

Para más información sobre cómo configurar los puntos de conexión ascendentes, consulte este [documento](https://docs.microsoft.com/azure/azure-signalr/concept-upstream).

A continuación, se muestra un diagrama que ilustra cómo funciona el modo sin servidor:

:::image type="content" source="media/concept-service-mode/serverless.png" alt-text="Selección del modo de servicio al crear":::

> [!NOTE]
> Tenga en cuenta que, en el modo predeterminado, también puede usar el enlace de función, el SDK de administración o la API REST para enviar mensajes directamente al cliente si no quiere pasar por el servidor concentrador. Sin embargo, en el modo predeterminado, las conexiones de cliente siguen siendo administradas por servidores concentradores y el punto de conexión ascendente no funcionará en ese modo.

## <a name="classic-mode"></a>Modo clásico

El clásico es una combinación del modo predeterminado y el modo sin servidor. En este modo, el modo de conexión se decide según si hay un servidor concentrador conectado cuando se establece la conexión de cliente. Si hay un servidor concentrador, la conexión de cliente se enrutará a él. En caso contrario, entrará en modo sin servidor, donde el mensaje del cliente al servidor no se puede entregar al servidor concentrador. Como consecuencia, se producirán algunas discrepancias; por ejemplo, si todos los servidores concentradores no están disponibles durante un breve período de tiempo, todas las conexiones de cliente creadas durante ese tiempo estarán en modo sin servidor y no podrán enviar mensajes al servidor concentrador.

> [!NOTE]
> El modo clásico se usa principalmente por la compatibilidad con versiones anteriores de esas aplicaciones creadas antes de que existiera el modo predeterminado y sin servidor. Se recomienda encarecidamente no usar este modo. En el caso de nuevas aplicaciones, elija el modo predeterminado o sin servidor, según su escenario. En el caso de aplicaciones existentes, también se recomienda revisar los casos de uso y elegir un modo de servicio adecuado.

El modo clásico tampoco admite algunas características nuevas, como el punto de conexión ascendente en modo sin servidor.

## <a name="choose-the-right-service-mode"></a>Selección del modo de servicio correcto

Ahora, debería comprender las diferencias entre los modos de servicio y saber cómo elegir entre ellos. Como ya aprendió en la sección anterior, no se recomienda el modo clásico y solo debe elegir entre predeterminado y sin servidor. A continuación, se muestran algunas sugerencias que pueden ayudarle a tomar la decisión adecuada para las nuevas aplicaciones y retirar el modo clásico para las aplicaciones existentes.

* Si ya está familiarizado con el funcionamiento de la biblioteca de SignalR y quiere pasar de un servicio SignalR autohospedado a Azure SignalR Service, elija el modo predeterminado. El modo predeterminado funciona exactamente igual que el servicio SignalR autohospedado (y puede usar el mismo modelo de programación en la biblioteca de SignalR), excepto que el servicio SignalR simplemente actúa como proxy entre los clientes y los servidores concentradores.

* Si va a crear una aplicación y no quiere mantener las conexiones de servidor y de servidor concentrador, elija el modo sin servidor. Este modo suele funcionar junto con Azure Functions, por lo que no es necesario mantener ningún servidor. Aunque puede seguir teniendo comunicaciones dúplex (con la API REST, el SDK de administración o el enlace de función + punto de conexión ascendente), el modelo de programación será diferente de la biblioteca de SignalR.

* Si tiene ambos servidores concentradores para que atiendan las conexiones de cliente y la aplicación de back-end para que inserte mensajes directamente en los clientes (por ejemplo, mediante la API REST), debe elegir el modo predeterminado. Tenga en cuenta que la diferencia principal entre el modo predeterminado y sin servidor radica en si tiene servidores concentradores y en cómo se enrutan las conexiones de cliente. La API REST, el SDK de administración y el enlace de función se pueden usar en ambos modos.

* Si en realidad tiene un escenario mixto, por ejemplo, tiene dos centros diferentes en el mismo recurso de SignalR (uno se usa como centro de SignalR tradicional y el otro con Azure Functions), y no tiene un servidor concentrador, debería considerar separarlos en dos recursos de SignalR, uno en modo predeterminado y el otro en modo sin servidor.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el modo predeterminado y sin servidor, lea los artículos siguientes:

* [Aspectos internos de Azure SignalR Service](signalr-concept-internals.md)

* [Desarrollo y configuración de Azure Functions con Azure SignalR Service](signalr-concept-serverless-development-config.md)
