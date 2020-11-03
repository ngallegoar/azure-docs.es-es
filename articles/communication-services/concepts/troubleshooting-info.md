---
title: Solución de problemas en Azure Communication Services
description: Aprenda a recopilar la información que necesita para solucionar los problemas de las soluciones de Communication Services.
author: manoskow
manager: jken
services: azure-communication-services
ms.author: manoskow
ms.date: 10/23/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 4921078e9e7b5d9de06fbbc9a97dc4a964569e04
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754680"
---
# <a name="troubleshooting-in-azure-communication-services"></a>Solución de problemas en Azure Communication Services

Este documento le ayudará a recopilar la información que necesita para solucionar los problemas de las soluciones de Communication Services.

## <a name="getting-help"></a>Ayuda

Animamos a los desarrolladores a enviar sus preguntas, sugerir características e informar de problemas como problemas en el [repositorio de GitHub](https://github.com/Azure/communication) de Communication Services. Otros foros incluyen:

* [Preguntas y respuestas de Microsoft](https://docs.microsoft.com/answers/questions/topics/single/101418.html)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/azure+communication)

En función del [plan de soporte técnico](https://azure.microsoft.com/support/plans/) de la suscripción a Azure, se puede enviar una incidencia de soporte técnico directamente a través de [Azure Portal](https://azure.microsoft.com/support/create-ticket/).

Para ayudarle a solucionar determinados tipos de problemas, es posible que se le pidan los siguientes datos:

* **Identificador de MS-CV** : este identificador se usa para solucionar problemas de llamadas y mensajes. 
* **Identificador de la llamada.** : este identificador se usa para identificar las llamadas a Communication Services.
* **Identificador de mensajes de texto** : este identificador se usa para identificar mensajes de texto.

## <a name="access-your-ms-cv-id"></a>Acceso al identificador de MS-CV

Para acceder al identificador de MS-CV, configure los diagnósticos en la instancia del objeto `clientOptions` al inicializar las bibliotecas de cliente. Se pueden configurar diagnósticos para cualquiera de las bibliotecas cliente de Azure, incluidas las de chat, administración y llamadas VoIP.

### <a name="client-options-example"></a>Ejemplo de opciones de cliente

Los fragmentos de código siguientes muestran la configuración del diagnóstico. Cuando las bibliotecas de cliente se usan con el diagnóstico habilitado, los detalles de diagnóstico se emitirán en el agente de escucha de eventos configurado:

# <a name="c"></a>[C#](#tab/csharp)
``` 
// 1. Import Azure.Core.Diagnostics
using Azure.Core.Diagnostics;

// 2. Initialize an event source listener instance
using var listener = AzureEventSourceListener.CreateConsoleLogger();
Uri endpoint = new Uri("https://<RESOURCE-NAME>.communication.azure.net");
var (token, communicationUser) = await GetCommunicationUserAndToken();
CommunicationUserCredential communicationUserCredential = new CommunicationUserCredential(token);

// 3. Setup diagnostic settings
var clientOptions = new ChatClientOptions()
{
    Diagnostics =
    {
        LoggedHeaderNames = { "*" },
        LoggedQueryParameters = { "*" },
        IsLoggingContentEnabled = true,
    }
};

// 4. Initialize the ChatClient instance with the clientOptions 
ChatClient chatClient = new ChatClient(endpoint, communicationUserCredential, clientOptions);
ChatThreadClient chatThreadClient = await chatClient.CreateChatThreadAsync("Thread Topic", new[] { new ChatThreadMember(communicationUser) });
```

# <a name="python"></a>[Python](#tab/python)
``` 
from azure.communication.chat import ChatClient, CommunicationUserCredential
endpoint = "https://communication-services-sdk-live-tests-for-python.communication.azure.com"
chat_client = ChatClient(
    endpoint,
    CommunicationUserCredential(token),
    http_logging_policy=your_logging_policy)
```
---

## <a name="access-your-call-id"></a>Acceso al identificador de llamada

Al presentar a través de Azure Portal una solicitud de soporte técnico relacionada con problemas en las llamadas, es posible que se le pida que especifique el identificador de la llamada a la que hace referencia. Para acceder a él se puede usar la biblioteca cliente de llamadas:

# <a name="javascript"></a>[JavaScript](#tab/javascript)
```javascript
// `call` is an instance of a call created by `callAgent.call` or `callAgent.join` methods 
console.log(call.id)
```

# <a name="ios"></a>[iOS](#tab/ios)
```objc
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends 
// todo: the code snippet suggests it's a property while the comment suggests it's a method call
print(call.callId) 
```

# <a name="android"></a>[Android](#tab/android)
```java
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends
// `call` is an instance of a call created by `callAgent.call(…)` or `callAgent.join(…)` methods 
Log.d(call.getCallId()) 
```
---


## <a name="access-your-sms-message-id"></a>Acceso al identificador de mensajes de texto

En caso de problemas en los mensajes de texto, puede recopilar el identificador del mensaje del objeto de respuesta.

# <a name="net"></a>[.NET](#tab/dotnet)
```
// Instantiate the SMS client
const smsClient = new SmsClient(connectionString);
async function main() {
  const result = await smsClient.send({
    from: "+18445792722",
    to: ["+1972xxxxxxx"],
    message: "Hello World 👋🏻 via Sms"
  }, {
    enableDeliveryReport: true // Optional parameter
  });
console.log(result); // your message ID will be in the result
}
```
---

## <a name="related-information"></a>Información relacionada
- [Registros y diagnósticos](logging-and-diagnostics.md)
- [Métricas](metrics.md)
