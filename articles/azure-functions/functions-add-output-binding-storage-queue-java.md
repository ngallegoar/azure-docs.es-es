---
title: Conexión de la función de Java a Azure Storage
description: Aprenda a conectar una función de Java desencadenada por HTTP a Azure Storage mediante un enlace de salida de Queue Storage.
author: KarlErickson
ms.author: karler
ms.date: 10/14/2019
ms.topic: quickstart
zone_pivot_groups: java-build-tools-set
ms.openlocfilehash: 8ae69bfa7ed00e310205332e05c071158c5fc9a3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78272800"
---
# <a name="connect-your-java-function-to-azure-storage"></a>Conexión de la función de Java a Azure Storage

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

En este artículo se muestra cómo integrar la función que creó en el [artículo de inicio rápido anterior](functions-create-first-java-maven.md) con una cola de Azure Storage. El enlace de salida que se agrega a esta función escribe datos de una solicitud HTTP en un mensaje de la cola.

La mayoría de los enlaces requieren una cadena de conexión almacenada que se usa en Functions para acceder al servicio enlazado. Para facilitar la conexión, usará la cuenta de almacenamiento que creó con la aplicación de funciones. La conexión a esta cuenta ya está almacenada en una configuración de aplicación llamada `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Prerequisites

Antes de empezar este artículo, realice los pasos de la [parte 1 del inicio rápido de Java](functions-create-first-java-maven.md).

## <a name="download-the-function-app-settings"></a>Descarga de la configuración de la aplicación de función

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Habilitación de conjuntos de extensiones

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Ahora podrá agregar el enlace de salida de almacenamiento al proyecto.

## <a name="add-an-output-binding"></a>Adición de un enlace de salida

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

## <a name="add-code-that-uses-the-output-binding"></a>Adición de código que utilice el enlace de salida

Ahora, puede usar el nuevo parámetro `msg` para escribir en el enlace de salida desde el código de la función. Agregue la siguiente línea de código antes de la respuesta de operación correcta para agregar el valor de `name` al enlace de salida `msg`.

```java
msg.setValue(name);
```

Al usar un enlace de salida, no tiene que usar el código del SDK de Azure Storage para autenticarse, obtener una referencia de cola o escribir datos. El sistema en tiempo de ejecución de Functions y el enlace de salida de cola realizan esas tareas automáticamente.

El método `run` debe ahora parecerse al del siguiente ejemplo:

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    context.getLogger().info("Java HTTP trigger processed a request.");

    // Parse query parameter
    String query = request.getQueryParameters().get("name");
    String name = request.getBody().orElse(query);

    if (name == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
    } else {
        // Write the name to the message queue. 
        msg.setValue(name);

        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
}
```

## <a name="update-the-tests"></a>Actualización de las pruebas

Dado que el arquetipo también crea un conjunto de pruebas, debe actualizar estas pruebas para controlar el nuevo parámetro `msg` en la signatura del método `run`.  

Vaya a la ubicación del código de prueba en _src/test/java_, abra el archivo de proyecto *Function.Java* y reemplace la línea de código debajo de `//Invoke` por el código siguiente.

```java
@SuppressWarnings("unchecked")
final OutputBinding<String> msg = (OutputBinding<String>)mock(OutputBinding.class);

// Invoke
final HttpResponseMessage ret = new Function().run(req, msg, context);
``` 

Ahora ya está listo para probar el nuevo enlace de salida de forma local.

## <a name="run-the-function-locally"></a>Ejecución local de la función

Como antes, use el siguiente comando para crear el proyecto e iniciar el sistema en tiempo de ejecución de Functions localmente:

::: zone pivot="java-build-tools-maven"  
```bash
mvn clean package 
mvn azure-functions:run
```
::: zone-end

::: zone pivot="java-build-tools-gradle"  
```bash
gradle jar --info
gradle azureFunctionsRun
```
::: zone-end

> [!NOTE]  
> Como ya habilitó las agrupaciones de extensiones en el archivo host.json, la [extensión de enlace de Storage](functions-bindings-storage-blob.md#add-to-your-functions-app) se descargó e instaló automáticamente durante el inicio junto con el resto de extensiones de enlace de Microsoft.

Al igual que antes, desencadene la función desde la línea de comandos mediante cURL en una ventana de terminal nueva:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

Esta vez, el enlace de salida también crea una cola denominada `outqueue` en la cuenta de almacenamiento y agrega un mensaje con esta misma cadena.

A continuación, se usa la CLI de Azure para ver la nueva cola y comprobar que se ha agregado un mensaje. También puede ver la cola mediante el [Explorador de Microsoft Azure Storage][Azure Storage Explorer] o en [Azure Portal](https://portal.azure.com).

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Reimplementación del proyecto 

Para actualizar la aplicación publicada, vuelva a ejecutar el siguiente comando:  

::: zone pivot="java-build-tools-maven"  
```bash
mvn azure-functions:deploy
```
::: zone-end

::: zone pivot="java-build-tools-gradle"  
```bash
gradle azureFunctionsDeploy
```
::: zone-end

De nuevo, puede usar cURL para probar la función implementada. Como antes, pase el valor `AzureFunctions` del cuerpo de la solicitud POST a la dirección URL, como en este ejemplo:

```bash
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Puede [examinar el mensaje de la cola de almacenamiento](#query-the-storage-queue) otra vez para comprobar que el enlace de salida genera un nuevo mensaje en la cola según lo previsto.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha actualizado la función desencadenada por HTTP para escribir datos en una cola de almacenamiento. Para más información sobre el desarrollo de Azure Functions con Java, consulte la [Guía de Azure Functions para desarrolladores de Java](functions-reference-java.md) y [Desencadenadores y enlaces en Azure Functions](functions-triggers-bindings.md). Para obtener ejemplos de proyectos de Functions completos en Java, consulte los [ejemplos de Functions para Java](/samples/browse/?products=azure-functions&languages=Java). 

A continuación, debe habilitar la supervisión de Application Insights para su aplicación de función:

> [!div class="nextstepaction"]
> [Habilitación de la integración de Application Insights](functions-monitoring.md#manually-connect-an-app-insights-resource)


[Azure Storage Explorer]: https://storageexplorer.com/
