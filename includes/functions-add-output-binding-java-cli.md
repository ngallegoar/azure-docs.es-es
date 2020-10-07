---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 2b2c043e70aac14c7fc6f0b58aae257624b05d13
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80673308"
---
En un proyecto de Java, los enlaces se definen como anotaciones de enlace en el método de función. Entonces, el archivo *function.json* se genera automáticamente en función de estas anotaciones.

Vaya a la ubicación del código de función en _src/main/java_, abra el archivo de proyecto *Function.java* y agregue el parámetro siguiente a la definición del método `run`:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

El parámetro `msg` es de tipo [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding), que representa una colección de cadenas escritas como mensajes en un enlace de salida cuando se completa la función. En este caso, la salida es una cola de almacenamiento denominada `outqueue`. La cadena de conexión de la cuenta de Storage la establece el método `connection`. En lugar de la propia cadena de conexión, el usuario se encarga de pasar la configuración de la aplicación que contiene la cadena de conexión de la cuenta de Storage.

La definición del método `run` debe ahora parecerse a la del siguiente ejemplo:  

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION)  
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    ...
}
```