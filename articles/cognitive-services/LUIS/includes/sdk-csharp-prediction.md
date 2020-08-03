---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 4bf86c616420bb049e1d7a82ad0e942e6eb7b36f
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369290"
---
La biblioteca cliente de predicción de Language Understanding (LUIS) para .NET puede usarse con los siguientes fines:

* Obtener predicciones por espacio
* Predicción por versión

[Documentación de referencia](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.LUIS.Runtime) | [Paquete (NuGet) del entorno de ejecución de predicción](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) | [Ejemplos de C#](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/LanguageUnderstanding/predict-with-sdk-3x)

## <a name="prerequisites"></a>Prerrequisitos

* Cuenta de portal de Language Understanding (LUIS): [cree una de forma gratuita](https://www.luis.ai).
* La versión actual de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Un identificador de aplicación de LUIS: use el identificador de aplicación de IoT público de `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. La consulta de usuario que se usa en el código del inicio rápido es específica de esa aplicación.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-c-application"></a>Creación de una aplicación de C#

Cree una nueva aplicación de consola de .NET Core en el IDE o editor que prefiera.

1. En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `new` dotnet para crear una nueva aplicación de consola con el nombre `language-understanding-quickstart`. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: `Program.cs`.

    ```dotnetcli
    dotnet new console -n language-understanding-quickstart
    ```

1. Cambie el directorio a la carpeta de aplicaciones recién creada.

1. Para compilar la aplicación:

    ```dotnetcli
    dotnet build
    ```

    La salida de la compilación no debe contener advertencias ni errores.

    ```console
    ...
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ...
    ```

### <a name="install-the-sdk"></a>Instalación del SDK

En el directorio de la aplicación, instale la biblioteca cliente del entorno de ejecución de predicción de Language Understanding (LUIS) para .NET con el siguiente comando:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime --version 3.0.0
```

Si usa el IDE de Visual Studio, la biblioteca cliente estará disponible como un paquete de NuGet descargable.

## <a name="object-model"></a>Modelo de objetos

El cliente del entorno de ejecución de predicción de Language Understanding (LUIS) es un objeto [LUISRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet) que se autentica en Azure, que contiene la clave del recurso.

Una vez creado el cliente, úselo para acceder a la funcionalidad, entre la que se incluye:

* Predicción por [espacio de ensayo o de producción](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getslotpredictionasync?view=azure-dotnet)
* Predicción por [versión](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.predictionoperationsextensions.getversionpredictionasync?view=azure-dotnet)


## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente del entorno de ejecución de predicción de Language Understanding (LUIS) para .NET:

* [Predicción por espacio](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Adición de las dependencias

En el directorio del proyecto, abra el archivo *Program.cs* en el editor o IDE que prefiera. Reemplace el código existente `using` por las siguientes directivas `using`:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_using)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

1. Cree variables para la clave, el nombre del recurso, el identificador de la aplicación y el espacio de publicación. Establezca el identificador de la aplicación en la aplicación IoT pública:

    **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`**

    [!code-csharp[Create variables](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_variables)]

1. Cree un objeto [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.apikeyserviceclientcredentials?view=azure-dotnet) con la clave y úselo con el punto de conexión para crear un objeto [LUISRuntimeClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-dotnet).

    [!code-csharp[Create LUIS client object](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_create_client)]

## <a name="get-prediction-from-runtime"></a>Obtención de la predicción a partir del entorno de ejecución

Agregue el método siguiente para crear la solicitud al entorno de ejecución de predicción.

La expresión de usuario forma parte del objeto [PredictionRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-dotnet).

El método **GetSlotPredictionAsync** necesita varios parámetros como, por ejemplo, el identificador de la aplicación, el nombre del espacio y el objeto de la solicitud de predicción para completar la solicitud. Las demás opciones, como verbose, muestran todas las intenciones y el registro es opcional.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_maintask)]

## <a name="main-code-for-the-prediction"></a>Código principal de la predicción

Use el método Main siguiente para vincular las variables y los métodos para obtener la predicción.

[!code-csharp[Create method to get prediction runtime](~/cognitive-services-quickstart-code/dotnet/LanguageUnderstanding/predict-with-sdk-3x/Program.cs?name=snippet_main)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `dotnet run` desde el directorio de la aplicación.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con las predicciones, limpie el trabajo de este inicio rápido mediante la eliminación del archivo program.cs y sus subdirectorios.
