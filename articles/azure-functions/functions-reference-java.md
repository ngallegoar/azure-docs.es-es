---
title: Referencia para desarrolladores de Java para Azure Functions
description: Aprenda a desarrollar funciones con Java.
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 19a290fe7717d7838e8fcd1d1f5cddb3f54eb812
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82145325"
---
# <a name="azure-functions-java-developer-guide"></a>Guía de Azure Functions para desarrolladores de Java

El tiempo de ejecución de Azure Functions admite [Java SE 8 LTS (zulu8.31.0.2-jre8.0.181-win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/). Esta guía contiene información acerca de las complejidades de la escritura de Azure Functions con Java.

Como sucede con otros lenguajes, una aplicación de funciones puede tener una o varias funciones. Una función Java es un método `public` decorado con la anotación `@FunctionName`. Este método define la entrada de una función Java, y debe ser única en un paquete determinado. Una aplicación de funciones escrita en Java puede tener varias clases con varios métodos públicos anotados con `@FunctionName`.

En este artículo se supone que ya ha leído [Referencia para desarrolladores de Azure Functions](functions-reference.md). También debe completar el inicio rápido de Functions para crear su primera función, mediante [Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java) o [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java).

## <a name="programming-model"></a>Modelo de programación 

Los conceptos de [desencadenadores y enlaces](functions-triggers-bindings.md) son fundamentales en Azure Functions. Los desencadenadores inician la ejecución del código. Los enlaces, por otro lado, proporcionan una manera de pasar y devolver datos de una función, sin tener que escribir un código de acceso a datos personalizados.

## <a name="create-java-functions"></a>Creación de funciones de Java

Para facilitar la creación de funciones de Java, hay herramientas y arquetipos basados en Maven que usan plantillas de Java predefinidas para ayudarle a crear proyectos con un desencadenador de funciones específico.    

### <a name="maven-based-tooling"></a>Herramientas basadas en Maven

Los siguientes entornos del desarrollador tienen herramientas de Azure Functions que le permiten crear proyectos de aplicación de una función de Java: 

+ [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions)
+ [Eclipse](functions-create-maven-eclipse.md)
+ [IntelliJ](functions-create-maven-intellij.md)

Los vínculos de artículo anteriores muestran cómo crear sus primeras funciones mediante el IDE que prefiera. 

### <a name="project-scaffolding"></a>Creación del scaffolding del proyecto

Si prefiere el desarrollo de la línea de comandos del terminal, la forma más sencilla de crear el scaffolding de los proyectos de aplicación de una función basados en Java es usar arquetipos `Apache Maven`. El arquetipo Maven de Java para Azure Functions está publicado en _groupId_:_artifactId_: [com.microsoft.azure:azure-functions-archetype](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-archetype/). 

El siguiente comando genera un nuevo proyecto de función de Java con este arquetipo:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

Para empezar a usar este arquetipo, consulte el [inicio rápido de Java](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java). 

## <a name="create-kotlin-functions-preview"></a>Creación de funciones de Kotlin (versión preliminar)

También existe un arquetipo Maven para generar funciones de Kotlin. Este arquetipo, actualmente en versión preliminar, se publica en _groupId_:_artifactId_: [com.microsoft.azure:azure-functions-kotlin-archetype](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-kotlin-archetype/). 

El siguiente comando genera un nuevo proyecto de función de Java con este arquetipo:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```

Para empezar a usar este arquetipo, consulte [Inicio rápido de Kotlin](functions-create-first-kotlin-maven.md).

## <a name="folder-structure"></a>Estructura de carpetas

Esta es la estructura de carpetas de un proyecto de Java de Azure Functions:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

_* El proyecto de Kotlin tiene un aspecto muy similar porque todavía sigue en Maven_.

Puede usar un archivo [host.json](functions-host-json.md) compartido para configurar la aplicación de funciones. Cada función tiene su propio archivo de código (.java) y archivo de configuración de enlace (function.json).

Puede colocar más de una función en un proyecto. Evite colocar las funciones en archivos JAR independientes. `FunctionApp` en el directorio de destino es lo que se implementa en la aplicación de funciones en Azure.

## <a name="triggers-and-annotations"></a>Desencadenadores y anotaciones

 Las funciones se invocan mediante un desencadenador, como una solicitud HTTP, un temporizador o una actualización de datos. La función debe procesar ese desencadenador y las demás entradas para generar una o más salidas.

Utilice las anotaciones de Java incluidas en el paquete [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) para enlazar las entradas y salidas a los métodos. Para más información, vea los [documentos de referencia de Java](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Debe configurar una cuenta de Azure Storage en [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) para ejecutar de manera local los desencadenadores de Azure Blob Storage, Azure Queue Storage o Azure Table Storage.

Ejemplo:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Aquí está el elemento `function.json` correspondiente que generó [azure-functions-maven-plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin):

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="jdk-runtime-availability-and-support"></a>Disponibilidad y soporte técnico del entorno de ejecución de JDK 

Para el desarrollo local de aplicaciones de funciones Java, descargue y utilice los JDK de Java 8 de [Azul Zulu Enterprise for Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) de [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/). Azure Functions usa el entorno en tiempo de ejecución del JDK de Java 8 de Azul al implementar las aplicaciones de función en la nube.

El [soporte técnico de Azure](https://azure.microsoft.com/support/) para problemas con los JDK y las aplicaciones de funciones está disponible con un [plan de soporte técnico cualificado](https://azure.microsoft.com/support/plans/).

## <a name="customize-jvm"></a>Personalización de JVM

Functions permite personalizar la Máquina virtual Java (JVM) utilizada para ejecutar funciones Java. Las [siguientes opciones de JVM](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) se usan de forma predeterminada:

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

Puede proporcionar argumentos adicionales en una configuración de la aplicación denominada `JAVA_OPTS`. Puede agregar la configuración de la aplicación a la aplicación de funciones implementada en Azure en Azure Portal o la CLI de Azure.

> [!IMPORTANT]  
> En el plan de consumo, también debe agregar el valor WEBSITE_USE_PLACEHOLDER con un valor de 0 para que funcione la personalización. Esta configuración aumenta los tiempos de arranque en frío de las funciones Java.

### <a name="azure-portal"></a>Azure Portal

En [Azure Portal](https://portal.azure.com), utilice la pestaña [Configuración de la aplicación](functions-how-to-use-azure-function-app-settings.md#settings) para agregar la configuración `JAVA_OPTS`.

### <a name="azure-cli"></a>Azure CLI

Puede usar el comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) para establecer `JAVA_OPTS`, como en el siguiente ejemplo:

#### <a name="consumption-plan"></a>[Plan de consumo](#tab/consumption)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
"WEBSITE_USE_PLACEHOLDER=0" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
#### <a name="dedicated-plan--premium-plan"></a>[Plan dedicado / plan Premium](#tab/dedicated+premium)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
---

En este ejemplo se habilita el modo "desatendido". Reemplace `<APP_NAME>` por el nombre de su aplicación de funciones y `<RESOURCE_GROUP>`, por el grupo de recursos. 

## <a name="third-party-libraries"></a>Bibliotecas de terceros 

Azure Functions admite el uso de bibliotecas de terceros. De forma predeterminada, todas las dependencias especificadas en el archivo `pom.xml` del proyecto se incluyen automáticamente durante el objetivo de [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage). Para las bibliotecas no especificadas como dependencias en el archivo `pom.xml`, colóquelas en un directorio `lib` del directorio raíz de la función. Las dependencias colocadas en el directorio `lib` se agregan al cargador de clases del sistema en tiempo de ejecución.

La dependencia `com.microsoft.azure.functions:azure-functions-java-library` se proporciona en la ruta de acceso de la clase de forma predeterminada y no debe incluirse en el directorio `lib`. Asimismo, [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) agrega a la ruta de clase las dependencias que figuran [aquí](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies).

## <a name="data-type-support"></a>Compatibilidad con tipos de datos

Puede usar objetos de Java POJO (Plain Old Java Object), tipos definidos en `azure-functions-java-library` o tipos de datos primitivos como String e Integer para enlazar con enlaces de entrada o salida.

### <a name="pojos"></a>POJO

Para convertir los datos de entrada a POJO, [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) usa la biblioteca [gson](https://github.com/google/gson). Los tipos de POJO que se usan como entradas para las funciones deben ser `public`.

### <a name="binary-data"></a>Datos binarios

Enlace entradas o salidas binarias a `byte[]`; para ello, establezca el campo `dataType` en function.json como `binary`:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Si espera valores null, use `Optional<T>`.

## <a name="bindings"></a>Enlaces

Los enlaces de entrada y de salida permiten conectarse de manera declarativa a los datos desde el código. Cada función puede tener varios enlaces de entrada y de salida.

### <a name="input-binding-example"></a>Ejemplo de enlace de entrada

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Esta función se invoca con una solicitud HTTP. 
- La carga útil de la solicitud HTTP se pasa como `String` para el argumento `inputReq`.
- Una entrada se recupera de Table Storage y se pasa como `TestInputData` al argumento `inputData`.

Para recibir un lote de entradas, puede enlazarse a `String[]`, `POJO[]`, `List<String>` o `List<POJO>`.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Esta función se desencadena cuando hay nuevos datos en el centro de eventos configurado. Como `cardinality` se establece en `MANY`, la función recibe un lote de mensajes desde el centro de eventos. El elemento `EventData` del centro de eventos se convierte a `TestEventData` para la ejecución de la función.

### <a name="output-binding-example"></a>Ejemplo de enlace de salida

Puede enlazar un enlace de salida al valor de retorno con `$return`. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Si hay varios enlaces de salida, use el valor devuelto para solo uno de ellos.

Para enviar varios valores de salida, use el tipo `OutputBinding<T>` que se define en el paquete `azure-functions-java-library`. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Esta función se invoca en una solicitud HttpRequest. Escribe varios valores en Queue Storage.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage y HttpResponseMessage

 Se definen en `azure-functions-java-library`. Son tipos de aplicación auxiliar que funcionan con funciones HttpTrigger.

| Tipo especializado      |       Destino        | Uso típico                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    Desencadenador HTTP     | Obtiene el método, encabezados o consultas. |
| `HttpResponseMessage` | Enlace de salida HTTP | Devuelve un estado distinto de 200.   |

## <a name="metadata"></a>Metadatos

Algunos desencadenadores envían [metadatos de desencadenador](/azure/azure-functions/functions-triggers-bindings) junto con datos de entrada. Puede usar la anotación `@BindingName` para enlazarla a metadatos de desencadenador.


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
En el ejemplo anterior, el elemento `queryValue` está enlazado al parámetro de cadena de consulta `name` de la dirección URL de solicitud HTTP `http://{example.host}/api/metadata?name=test`. Este es otro ejemplo que muestra cómo enlazar a `Id` desde los metadatos de desencadenador de la cola.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> El nombre que se proporciona en la anotación debe coincidir con la propiedad de metadatos.

## <a name="execution-context"></a>Contexto de ejecución

El elemento `ExecutionContext` definido en `azure-functions-java-library` contiene métodos de ayuda que le permitirán comunicarse con las funciones en tiempo de ejecución.

### <a name="logger"></a>Registrador

Use el elemento `getLogger` definido en `ExecutionContext` para escribir registros desde el código de función.

Ejemplo:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Ver registros y seguimiento

Puede usar la CLI de Azure para hacer streaming del registro stdout y stderr de Java, así como de otros registros de la aplicación. 

Aquí se explica cómo configurar la aplicación de funciones para escribir el registro de aplicación con la CLI de Azure:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Para transmitir en secuencias la salida para la aplicación de funciones con la CLI de Azure, abra una nueva sesión del símbolo del sistema, Bash o Terminal y escriba el siguiente comando:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
El comando [az webapp log tail](/cli/azure/webapp/log) tiene opciones para filtrar la salida usando la opción `--provider`. 

Para descargar los archivos de registro como un solo archivo ZIP mediante la CLI de Azure, abra una sesión nueva del símbolo del sistema, Bash o Terminal y escriba el siguiente comando:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Debe haber habilitado el registro del sistema de archivos en Azure Portal o la CLI de Azure antes de ejecutar este comando.

## <a name="environment-variables"></a>Variables de entorno

En Functions, [la configuración de la aplicación](functions-app-settings.md), como las cadenas de conexión del servicio, se exponen como variables de entorno durante la ejecución. Puede obtener acceso a estas opciones mediante `System.getenv("AzureWebJobsStorage")`.

En el siguiente ejemplo se obtiene la [configuración de la aplicación](functions-how-to-use-azure-function-app-settings.md#settings), con la clave denominada `myAppSetting`:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("My app setting value: "+ System.getenv("myAppSetting"));
        return String.format(req);
    }
}

```

> [!NOTE]
> El valor de AppSetting FUNCTIONS_EXTENSION_VERSION debería ser ~2 o ~3 para una experiencia de arranque en frío optimizada.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el desarrollo de Java de Azure Functions, vea los siguientes recursos:

* [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
* [Referencia para desarrolladores de Azure Functions](functions-reference.md)
* [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
* Desarrollo y depuración local con [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) y [Eclipse](functions-create-maven-eclipse.md)
* [Depuración remota de Azure Functions en Java con Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Complemento Maven para Azure Functions](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* Optimización de la creación de funciones a través del objetivo `azure-functions:add` y preparación de un directorio de almacenamiento provisional para la [implementación del archivo ZIP](deployment-zip-push.md).
