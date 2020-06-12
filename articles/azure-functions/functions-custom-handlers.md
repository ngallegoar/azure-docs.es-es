---
title: Controladores personalizados de Azure Functions (versión preliminar)
description: Aprenda a utilizar Azure Functions con cualquier versión de lenguaje o de tiempo de ejecución.
author: craigshoemaker
ms.author: cshoe
ms.date: 3/18/2020
ms.topic: article
ms.openlocfilehash: f0b738f394c4a544ddb31e25b4570890ccfa9235
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83995877"
---
# <a name="azure-functions-custom-handlers-preview"></a>Controladores personalizados de Azure Functions (versión preliminar)

Cada aplicación de Functions se ejecuta mediante un controlador específico de lenguaje. Aunque Azure Functions admite muchos [controladores de lenguajes](./supported-languages.md) de forma predeterminada, hay casos en los que es posible que desee tener un mayor control sobre el entorno de ejecución de la aplicación. Los controladores personalizados proporcionan este control adicional.

Los controladores personalizados son servidores web ligeros que reciben eventos del host de Functions. Cualquier lenguaje que admita primitivas de HTTP puede implementar un controlador personalizado.

Los controladores personalizados son más adecuados para las situaciones en las que desea:

- Implementar una aplicación de funciones en un lenguaje que no se admite oficialmente.
- Implementar una aplicación de funciones en una versión o un tiempo de ejecución de lenguaje no compatible de forma predeterminada.
- Ofrecer un control pormenorizado sobre el entorno de ejecución de la aplicación de funciones.

Con los controladores personalizados, todos los [desencadenadores y enlaces de entrada y salida](./functions-triggers-bindings.md) son compatibles mediante [conjuntos de extensiones](./functions-bindings-register.md).

## <a name="overview"></a>Información general

En el diagrama siguiente se muestra la relación entre el host de Functions y un servidor web implementado como controlador personalizado.

![Introducción a los controladores personalizados de Azure Functions](./media/functions-custom-handlers/azure-functions-custom-handlers-overview.png)

- Los eventos desencadenan una solicitud enviada al host de Functions. El evento lleva una carga HTTP sin formato (para las funciones desencadenadas por HTTP sin enlaces) o una carga que contiene los datos de enlace de entrada para la función.
- Después, el host de Functions envía la solicitud al servidor web como proxy mediante la emisión de una [carga de solicitud](#request-payload).
- El servidor web ejecuta la función individual y devuelve una [carga de respuesta](#response-payload) al host de Functions.
- El host de Functions envía la respuesta como proxy como, por ejemplo, una carga de enlace de salida al destino.

Una aplicación Azure Functions implementada como controlador personalizado debe configurar los archivos *host.json* y *function.json* siguiendo algunas convenciones.

## <a name="application-structure"></a>Estructura de la aplicación

Para implementar un controlador personalizado, necesita los siguientes aspectos para la aplicación:

- Un archivo *host.json* en la raíz de la aplicación
- Un archivo *function.json* para cada función (dentro de una carpeta que coincida con el nombre de función)
- Un comando, script o archivo ejecutable, que ejecuta un servidor web.

En el diagrama siguiente se muestra cómo buscan estos archivos una función denominada "order" en el sistema de archivos.

```bash
| /order
|   function.json
|
| host.json
```

### <a name="configuration"></a>Configuración

La aplicación se configura mediante el archivo *host.json*. Este archivo indica al host de Functions dónde se deben enviar las solicitudes al apuntar a un servidor web capaz de procesar eventos HTTP.

Para definir un controlador personalizado, configure el archivo *host.json* con detalles sobre cómo ejecutar el servidor web siguiendo la sección `httpWorker`.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "server.exe"
        }
    }
}
```

La sección `httpWorker` apunta a un destino tal y como se define en `defaultExecutablePath`. El destino de ejecución puede ser un comando, ejecutable o archivo en el que esté implementado el servidor web.

En el caso de aplicaciones con un script, `defaultExecutablePath` apunta al tiempo de ejecución del lenguaje del script y `defaultWorkerPath` apunta a la ubicación del archivo de script. En el ejemplo siguiente se muestra cómo se configura una aplicación de JavaScript en Node.js como un controlador personalizado.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

También puede pasar argumentos mediante la matriz `arguments`:

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js",
            "arguments": [ "--argument1", "--argument2" ]
        }
    }
}
```

Los argumentos son necesarios para muchas configuraciones de depuración. Consulte la sección [Depuración](#debugging) para más información.

> [!NOTE]
> El archivo *host.json* debe estar en el mismo nivel en la estructura de directorios que el servidor web en ejecución. Es posible que algunos lenguajes y cadenas de herramientas no puedan colocar este archivo en la raíz de la aplicación de forma predeterminada.

#### <a name="bindings-support"></a>Compatibilidad con enlaces

Los desencadenadores estándar junto con los enlaces de entrada y salida están disponibles al hacer referencia a los [conjuntos de extensión](./functions-bindings-register.md) en el archivo *host.json*.

### <a name="function-metadata"></a>Metadatos de función

Cuando se usa con un controlador personalizado, el contenido de *function.json* no es diferente a cómo se define una función en cualquier otro contexto. El único requisito es que los archivos  *function.json* deben estar en una carpeta con nombre que coincida con el nombre de la función.

### <a name="request-payload"></a>Carga de solicitud

La carga de solicitud para las funciones HTTP puras es la carga de solicitud HTTP sin formato. Las funciones HTTP puras se definen como funciones sin enlaces de entrada o de salida, que devuelven una respuesta HTTP.

Cualquier otro tipo de función que incluya enlaces de entrada, de salida o se desencadene mediante un origen de eventos distinto de HTTP tiene una carga de solicitud personalizada.

El código siguiente representa una carga de solicitud de ejemplo. La carga incluye una estructura JSON con dos miembros: `Data` y `Metadata`.

El miembro `Data` incluye claves que coinciden con los nombres de los desencadenadores y de entrada, tal y como se definen en la matriz de enlaces del archivo *function.json*.

El miembro `Metadata` incluye [metadatos generados a partir del origen del evento](./functions-bindings-expressions-patterns.md#trigger-metadata).

Dados los enlaces definidos en el siguiente archivo *function.json*:

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages-incoming",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "type": "queue",
      "direction": "out",
      "queueName": "messages-outgoing",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

Se devuelve una carga de solicitud similar a la de este ejemplo:

```json
{
    "Data": {
        "myQueueItem": "{ message: \"Message sent\" }"
    },
    "Metadata": {
        "DequeueCount": 1,
        "ExpirationTime": "2019-10-16T17:58:31+00:00",
        "Id": "800ae4b3-bdd2-4c08-badd-f08e5a34b865",
        "InsertionTime": "2019-10-09T17:58:31+00:00",
        "NextVisibleTime": "2019-10-09T18:08:32+00:00",
        "PopReceipt": "AgAAAAMAAAAAAAAAAgtnj8x+1QE=",
        "sys": {
            "MethodName": "QueueTrigger",
            "UtcNow": "2019-10-09T17:58:32.2205399Z",
            "RandGuid": "24ad4c06-24ad-4e5b-8294-3da9714877e9"
        }
    }
}
```

### <a name="response-payload"></a>Carga de respuesta

Por convención, las respuestas de función tienen el formato de pares clave-valor. Las claves admitidas son:

| <nobr>Clave de carga</nobr>   | Tipo de datos | Observaciones                                                      |
| ------------- | --------- | ------------------------------------------------------------ |
| `Outputs`     | JSON      | Contiene los valores de respuesta definidos por la matriz `bindings` del archivo *function.json*.<br /><br />Por ejemplo, si una función se configura con un enlace de salida de almacenamiento de blobs denominado "blob", entonces `Outputs` contiene una clave denominada `blob`, que se establece en el valor del blob. |
| `Logs`        | array     | Los mensajes aparecen en los registros de invocación de Functions.<br /><br />Cuando se ejecuta en Azure, los mensajes aparecen en Application Insights. |
| `ReturnValue` | string    | Se usa para proporcionar una respuesta cuando se configura una salida como `$return` en el archivo *function.json*. |

Consulte el [ejemplo para ver una carga de ejemplo](#bindings-implementation).

## <a name="examples"></a>Ejemplos

Los controladores personalizados se pueden implementar en cualquier lenguaje que admita eventos HTTP. Aunque Azure Functions [es totalmente compatible con JavaScript y Node.js](./functions-reference-node.md), en los ejemplos siguientes se muestra cómo implementar un controlador personalizado mediante JavaScript en Node.js con fines instructivos.

> [!TIP]
> Al ser una guía para aprender a implementar un controlador personalizado en otros lenguajes, los ejemplos basados en Node.js que se muestran aquí también pueden resultar útiles si desea ejecutar una aplicación de Functions en una versión no compatible de Node.js.

## <a name="http-only-function"></a>Función de solo HTTP

En el ejemplo siguiente se muestra cómo configurar una función desencadenada por HTTP sin enlaces ni salidas adicionales. El escenario implementado en este ejemplo incluye una función denominada `http` que acepta `GET` o `POST`.

El siguiente fragmento de código representa cómo está compuesta una solicitud a la función.

```http
POST http://127.0.0.1:7071/api/hello HTTP/1.1
content-type: application/json

{
  "message": "Hello World!"
}
```

<a id="hello-implementation" name="hello-implementation"></a>

### <a name="implementation"></a>Implementación

En una carpeta denominada *http*, el archivo *function.json* configura la función desencadenada por HTTP.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get", "post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

La función está configurada para aceptar las solicitudes `GET` y `POST`, y el valor del resultado se proporciona mediante un argumento denominado `res`.

En la raíz de la aplicación, el archivo *host.json* está configurado para ejecutar Node.js y apuntar al archivo `server.js`.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

El archivo *server.js* implementa un servidor web y una función HTTP.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

const PORT = process.env.FUNCTIONS_HTTPWORKER_PORT;

const server = app.listen(PORT, "localhost", () => {
  console.log(`Your port is ${PORT}`);
  const { address: host, port } = server.address();
  console.log(`Example app listening at http://${host}:${port}`);
});

app.get("/hello", (req, res) => {
  res.json("Hello World!");
});

app.post("/hello", (req, res) => {
  res.json({ value: req.body });
});
```

En este ejemplo, se utiliza Express para crear un servidor web y administrar eventos HTTP, y se configura para que escuche solicitudes mediante `FUNCTIONS_HTTPWORKER_PORT`.

La función se define en la ruta de acceso de `/hello`. Las solicitudes `GET` se administran al devolver un objeto JSON simple y las solicitudes `POST` tienen acceso al cuerpo de la solicitud mediante `req.body`.

La ruta de la función order aquí es `/hello` y no `/api/hello` porque el host de Functions envía la solicitud como proxy al controlador personalizado.

>[!NOTE]
>`FUNCTIONS_HTTPWORKER_PORT` no es el puerto de acceso público que se usa para llamar a la función. El host de Functions usa este puerto para llamar al controlador personalizado.

## <a name="function-with-bindings"></a>Función con enlaces

El escenario implementado en este ejemplo incluye una función denominada `order` que acepta `POST` con una carga que representa un pedido de producto. A medida que se publica un pedido en la función, se crea un mensaje de Queue Storage y se devuelve una respuesta HTTP.

```http
POST http://127.0.0.1:7071/api/order HTTP/1.1
content-type: application/json

{
  "id": 1005,
  "quantity": 2,
  "color": "black"
}
```

<a id="bindings-implementation" name="bindings-implementation"></a>

### <a name="implementation"></a>Implementación

En una carpeta denominada *order*, el archivo *function.json* configura la función desencadenada por HTTP.

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "authLevel": "function",
      "direction": "in",
      "name": "req",
      "methods": ["post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "queue",
      "name": "message",
      "direction": "out",
      "queueName": "orders",
      "connection": "AzureWebJobsStorage"
    }
  ]
}

```

Esta función se define como una [función desencadenada por HTTP](./functions-bindings-http-webhook-trigger.md) que devuelve una [respuesta HTTP](./functions-bindings-http-webhook-output.md) y genera un mensaje de [Queue Storage](./functions-bindings-storage-queue-output.md).

En la raíz de la aplicación, el archivo *host.json* está configurado para ejecutar Node.js y apuntar al archivo `server.js`.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js"
        }
    }
}
```

El archivo *server.js* implementa un servidor web y una función HTTP.

```javascript
const express = require("express");
const app = express();

app.use(express.json());

const PORT = process.env.FUNCTIONS_HTTPWORKER_PORT;

const server = app.listen(PORT, "localhost", () => {
  console.log(`Your port is ${PORT}`);
  const { address: host, port } = server.address();
  console.log(`Example app listening at http://${host}:${port}`);
});

app.post("/order", (req, res) => {
  const message = req.body.Data.req.Body;
  const response = {
    Outputs: {
      message: message,
      res: {
        statusCode: 200,
        body: "Order complete"
      }
    },
    Logs: ["order processed"]
  };
  res.json(response);
});
```

En este ejemplo, se utiliza Express para crear un servidor web y administrar eventos HTTP, y se configura para que escuche solicitudes mediante `FUNCTIONS_HTTPWORKER_PORT`.

La función se define en la ruta de acceso de `/order`.  La ruta de la función order aquí es `/order` y no `/api/order` porque el host de Functions envía la solicitud como proxy al controlador personalizado.

A medida que se envían solicitudes `POST` a esta función, los datos se exponen a lo largo de algunos puntos:

- El cuerpo de la solicitud está disponible mediante `req.body`
- Los datos que se publican en la función están disponibles mediante `req.body.Data.req.Body`

El formato de la respuesta de la función se aplica a un par clave-valor, donde el miembro `Outputs` contiene un valor JSON en el que las claves coinciden con las salidas definidas en el archivo *function.json*.

Al establecer `message` igual al mensaje procedente de la solicitud y `res` igual a la respuesta HTTP esperada, esta función envía un mensaje a Queue Storage y devuelve una respuesta HTTP.

## <a name="debugging"></a>Depuración

Para depurar la aplicación de controlador personalizada de Functions, debe agregar los argumentos adecuados para el lenguaje y el tiempo de ejecución a fin de habilitar la depuración.

Por ejemplo, para depurar una aplicación de Node.js, la marca `--inspect` se pasa como argumento en el archivo *host.json*.

```json
{
    "version": "2.0",
    "httpWorker": {
        "description": {
            "defaultExecutablePath": "node",
            "defaultWorkerPath": "server.js",
            "arguments": [ "--inspect" ]
        }
    }
}
```

> [!NOTE]
> La configuración de depuración forma parte del archivo *host.json*, lo que significa que es posible que tenga que quitar algunos argumentos antes de la implementación en la producción.

Con esta configuración, puede iniciar el proceso de host de la función con el siguiente comando:

```bash
func host start
```

Una vez iniciado el proceso, puede asociar un depurador y llegar a los puntos de interrupción.

### <a name="visual-studio-code"></a>Visual Studio Code

El ejemplo siguiente es una configuración de ejemplo que muestra cómo puede configurar el archivo *launch.json* para conectar la aplicación al depurador de Visual Studio Code.

Este ejemplo es para Node.js, por lo que es posible que tenga que modificarlo para otros lenguajes o tiempos de ejecución.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Attach to Node Functions",
      "type": "node",
      "request": "attach",
      "port": 9229,
      "preLaunchTask": "func: host start"
    }
  ]
}
```

## <a name="deploying"></a>Implementando

Se puede implementar un controlador personalizado en casi todas las opciones de hospedaje de Azure Functions (consulte [restricciones](#restrictions)). Si el controlador requiere dependencias personalizadas (por ejemplo, un tiempo de ejecución de lenguaje), es posible que tenga que usar un [contenedor personalizado](./functions-create-function-linux-custom-image.md).

## <a name="restrictions"></a>Restricciones

- Los controladores personalizados no se admiten en los planes de consumo de Linux.
- El servidor web se debe iniciar en un plazo de 60 segundos.

## <a name="samples"></a>Ejemplos

Consulte el [repositorio de GitHub de ejemplos del controlador personalizado](https://github.com/Azure-Samples/functions-custom-handlers) para ver ejemplos sobre cómo implementar funciones en una variedad de lenguajes distintos.
