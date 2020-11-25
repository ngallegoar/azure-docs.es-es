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
ms.openlocfilehash: 775075765c8c8eaa94541c0f094c1f7743fe59d9
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94886794"
---
# <a name="troubleshooting-in-azure-communication-services"></a>Solución de problemas en Azure Communication Services

Este documento le ayudará a solucionar los problemas que pueda experimentar en la solución Communication Services. Si está solucionando problemas de SMS, puede [habilitar los informes de entrega con Event Grid](../quickstarts/telephony-sms/handle-sms-events.md) para capturar los detalles de la entrega de SMS.

## <a name="getting-help"></a>Ayuda

Animamos a los desarrolladores a enviar sus preguntas, sugerir características e informar de problemas como problemas en el [repositorio de GitHub](https://github.com/Azure/communication) de Communication Services. Otros foros incluyen:

* [Preguntas y respuestas de Microsoft](/answers/questions/topics/single/101418.html)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/azure+communication)

En función del [plan de soporte técnico](https://azure.microsoft.com/support/plans/) de la suscripción a Azure, se puede enviar una incidencia de soporte técnico directamente a través de [Azure Portal](https://azure.microsoft.com/support/create-ticket/).

Para ayudarle a solucionar determinados tipos de problemas, es posible que se le pidan los siguientes datos:

* **Identificador de MS-CV**: este identificador se usa para solucionar problemas de llamadas y mensajes. 
* **Identificador de la llamada.** : este identificador se usa para identificar las llamadas a Communication Services.
* **Identificador de mensajes de texto**: este identificador se usa para identificar mensajes de texto.
* **Registros de llamada**: estos registros contienen información detallada que se puede usar para solucionar problemas de llamada y de red.


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

## <a name="enable-and-access-call-logs"></a>Habilitación y acceso a los registros de llamadas




# <a name="javascript"></a>[JavaScript](#tab/javascript)

El código siguiente se puede usar para configurar `AzureLogger` para generar registros en la consola mediante la biblioteca de cliente de JavaScript:

```javascript
import { AzureLogger } from '@azure/logger'; 

AzureLogger.verbose = (...args) => { console.info(...args); } 
AzureLogger.info = (...args) => { console.info(...args); } 
AzureLogger.warning = (...args) => { console.info(...args); } 
AzureLogger.error = (...args) => { console.info(...args); } 

callClient = new CallClient({logger: AzureLogger}); 
```

# <a name="ios"></a>[iOS](#tab/ios)

Al desarrollar para iOS, los registros se almacenan en archivos `.blog`. Tenga en cuenta que no puede ver los registros directamente porque están cifrados.

Se puede tener acceso a ellos abriendo Xcode. Vaya a Windows > Devices and Simulators > Devices (Windows > Dispositivos y simuladores > Dispositivos). Seleccione el dispositivo. En Installed Apps (Aplicaciones instaladas), seleccione la aplicación y haga clic en "Download container" (Descargar contenedor). 

Esto le proporcionará un archivo `xcappdata`. Haga clic con el botón derecho en este archivo y seleccione "Show package contents" (Mostrar contenido del paquete). Después verá los archivos `.blog` que puede adjuntar a la solicitud de soporte técnico de Azure.

# <a name="android"></a>[Android](#tab/android)

Al desarrollar para Android, los registros se almacenan en archivos `.blog`. Tenga en cuenta que no puede ver los registros directamente porque están cifrados.

En Android Studio, vaya al explorador de archivos de dispositivo; para ello, seleccione View > Tool Windows > Device File Explorer (Ver > Herramienta Windows > Explorador de archivos de dispositivo) desde el simulador y el dispositivo. El archivo `.blog` se ubicará en el directorio de la aplicación, que debe ser similar a `/data/data/[app_name_space:com.contoso.com.acsquickstartapp]/files/acs_sdk.blog`. Puede adjuntar este archivo a la solicitud de soporte técnico. 
   

---


## <a name="related-information"></a>Información relacionada
- [Registros y diagnósticos](logging-and-diagnostics.md)
- [Métricas](metrics.md)