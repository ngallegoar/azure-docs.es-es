---
title: 'Inicio rápido: Biblioteca cliente de Text Analytics para C# | Microsoft Docs'
description: Introducción a la biblioteca cliente de Text Analytics para C#
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/07/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 35d5940fbc001d1806711afb14aa4a549bcb1826
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96615758"
---
<a name="HOLTop"></a>

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)

[Documentación de referencia de v3.1](/dotnet/api/azure.ai.textanalytics?preserve-view=true&view=azure-dotnet-previews) | [Código fuente de la biblioteca v3.1](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics) | [Paquete v3.1 (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics/5.1.0-beta.1) | [Ejemplos de v3.1](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)

[Documentación de referencia de v3](/dotnet/api/azure.ai.textanalytics) | [Código fuente de la biblioteca v3](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics) | [Paquete v3 (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [Ejemplos de v3](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

[Documentación de referencia de v2](/dotnet/api/overview/azure/cognitiveservices/client?view=azure-dotnet) | [Código fuente de la biblioteca v2](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [Paquete v2 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [Ejemplos de v2](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services)
* [IDE de Visual Studio](https://visualstudio.microsoft.com/vs/)
* Una vez que tenga la suscripción de Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="cree un recurso de Text Analytics"  target="_blank">cree un recurso de Text Analytics <span class="docon docon-navigate-external x-hidden-focus"></span></a> en Azure Portal para obtener la clave y el punto de conexión.  Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a API Text Analytics. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-net-core-application"></a>Creación de una aplicación de .NET Core

Utilice el IDE de Visual Studio para crear una aplicación de consola de .NET Core. Así se creará un proyecto "Hola mundo" con un solo archivo de origen de C#: *program.cs*.

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)

Instale la biblioteca cliente, para lo que debe hacer clic con el botón derecho en la solución en el **Explorador de soluciones** y seleccionar **Administrar paquetes NuGet**. En el administrador de paquetes que se abre, seleccione **Examinar** y busque `Azure.AI.TextAnalytics`. Active la casilla **Incluir versión preliminar**, seleccione la versión `5.1.0-beta.1` y, a continuación, **Instalar**. También puede usar la [Consola del Administrador de paquetes](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package).

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)

Instale la biblioteca cliente, para lo que debe hacer clic con el botón derecho en la solución en el **Explorador de soluciones** y seleccionar **Administrar paquetes NuGet**. En el administrador de paquetes que se abre, seleccione **Examinar** y busque `Azure.AI.TextAnalytics`. Seleccione la versión `5.0.0` e **Instalar**. También puede usar la [Consola del Administrador de paquetes](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package).


> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo [en GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs), que contiene los ejemplos de código de este inicio rápido. 

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Instale la biblioteca cliente, para lo que debe hacer clic con el botón derecho en la solución en el **Explorador de soluciones** y seleccionar **Administrar paquetes NuGet**. En el administrador de paquetes que se abre, seleccione **Examinar** y busque `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`. Haga clic en él y, después, en **Instalar**. También puede usar la [Consola del Administrador de paquetes](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package).

> [!TIP]
> ¿Desea ver todo el archivo de código de inicio rápido de una vez? Puede encontrarlo [en GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs), que contiene los ejemplos de código de este inicio rápido. 

---

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)

Abra el archivo *program.cs* y agregue las siguientes `using` directivas:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

En la clase `Program` de la aplicación, cree variables para el punto de conexión y la clave del recurso.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Reemplace el método de `Main` de la aplicación. Más adelante definirá los métodos a los que llama aquí.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    SentimentAnalysisWithOpinionMiningExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    RecognizePIIExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)

Abra el archivo *program.cs* y agregue las siguientes `using` directivas:

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

En la clase `Program` de la aplicación, cree variables para el punto de conexión y la clave del recurso.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Reemplace el método de `Main` de la aplicación. Más adelante definirá los métodos a los que llama aquí.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Abra el archivo *program.cs* y agregue las siguientes `using` directivas:

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

En la clase `Program` de la aplicación, cree variables para el punto de conexión y la clave del recurso. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

Reemplace el método de `Main` de la aplicación. Más adelante definirá los métodos a los que llama aquí.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>Modelo de objetos

El cliente de Text Analytics es un objeto `TextAnalyticsClient` que se autentica en Azure mediante su clave y que proporciona funciones para aceptar texto como cadenas individuales o como un lote. Puede enviar texto a la API de forma sincrónica o asincrónica. El objeto de respuesta contendrá la información del análisis de todos los documentos que envíe. 

Si va a usar la versión `3.x` del servicio, puede emplear una instancia de `TextAnalyticsClientOptions` opcional para inicializar el cliente con distintas configuraciones predeterminadas (por ejemplo, el idioma predeterminado o la sugerencia de país o región). También puede autenticarse mediante un token de Azure Active Directory. 

## <a name="code-examples"></a>Ejemplos de código

* [Análisis de opiniones](#sentiment-analysis)
* [Minería de opiniones](#opinion-mining)
* [Detección de idioma](#language-detection)
* [Reconocimiento de entidades con nombre](#named-entity-recognition-ner)
* [Vinculación de entidad](#entity-linking)
* [Extracción de frases clave](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Autenticar el cliente

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)

Asegúrese de que el método main anterior crea un objeto de cliente con el punto de conexión y las credenciales.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)

Asegúrese de que el método main anterior crea un objeto de cliente con el punto de conexión y las credenciales.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una clase `ApiKeyServiceClientCredentials` para almacenar las credenciales y agregarlas a las solicitudes del cliente. Dentro de ella, cree una invalidación para `ProcessHttpRequestAsync()` que agregue su clave al encabezado `Ocp-Apim-Subscription-Key`.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

Cree un método para crear una instancia del objeto [TextAnalyticsClient](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient) con el punto de conexión y un objeto `ApiKeyServiceClientCredentials` que contiene la clave.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>análisis de opiniones

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)

Cree una función denominada `SentimentAnalysisExample()` que tome el cliente que creó anteriormente y llame a su función `AnalyzeSentiment()`. El objeto `Response<DocumentSentiment>` devuelto contendrá la etiqueta de opinión y la puntuación del documento de entrada completo, así como un análisis de opinión de cada oración si la operación se realiza correctamente. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Output

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

### <a name="opinion-mining"></a>Minería de opiniones

Cree una función denominada `SentimentAnalysisWithOpinionMiningExample()` que tome el cliente que creó anteriormente y llame a su función `AnalyzeSentimentBatch()` con la opción `AdditionalSentimentAnalyses.OpinionMining`. El objeto devuelto `AnalyzeSentimentResultCollection` contendrá la colección de `AnalyzeSentimentResult` en la que representa `Response<DocumentSentiment>`. La diferencia entre `SentimentAnalysis()` y `SentimentAnalysisWithOpinionMiningExample()` es que el último contendrá `MinedOpinion` en cada oración, que muestra un aspecto analizado y las opiniones relacionadas. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void SentimentAnalysisWithOpinionMiningExample(TextAnalyticsClient client)
{
    var documents = new List<string>
    {
        "The food and service were unacceptable, but the concierge were nice."
    };

    AnalyzeSentimentResultCollection reviews = client.AnalyzeSentimentBatch(documents, options: new AnalyzeSentimentOptions()
    {
        AdditionalSentimentAnalyses = AdditionalSentimentAnalyses.OpinionMining
    });

    foreach (AnalyzeSentimentResult review in reviews)
    {
        Console.WriteLine($"Document sentiment: {review.DocumentSentiment.Sentiment}\n");
        Console.WriteLine($"\tPositive score: {review.DocumentSentiment.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {review.DocumentSentiment.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {review.DocumentSentiment.ConfidenceScores.Neutral:0.00}\n");
        foreach (SentenceSentiment sentence in review.DocumentSentiment.Sentences)
        {
            Console.WriteLine($"\tText: \"{sentence.Text}\"");
            Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
            Console.WriteLine($"\tSentence positive score: {sentence.ConfidenceScores.Positive:0.00}");
            Console.WriteLine($"\tSentence negative score: {sentence.ConfidenceScores.Negative:0.00}");
            Console.WriteLine($"\tSentence neutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");

            foreach (MinedOpinion minedOpinion in sentence.MinedOpinions)
            {
                Console.WriteLine($"\tAspect: {minedOpinion.Aspect.Text}, Value: {minedOpinion.Aspect.Sentiment}");
                Console.WriteLine($"\tAspect positive score: {minedOpinion.Aspect.ConfidenceScores.Positive:0.00}");
                Console.WriteLine($"\tAspect negative score: {minedOpinion.Aspect.ConfidenceScores.Negative:0.00}");
                foreach (OpinionSentiment opinion in minedOpinion.Opinions)
                {
                    Console.WriteLine($"\t\tRelated Opinion: {opinion.Text}, Value: {opinion.Sentiment}");
                    Console.WriteLine($"\t\tRelated Opinion positive score: {opinion.ConfidenceScores.Positive:0.00}");
                    Console.WriteLine($"\t\tRelated Opinion negative score: {opinion.ConfidenceScores.Negative:0.00}");
                }
            }
        }
        Console.WriteLine($"\n");
    }
}
```

### <a name="output"></a>Resultados

```console
Document sentiment: Positive

        Positive score: 0.84
        Negative score: 0.16
        Neutral score: 0.00

        Text: "The food and service were unacceptable, but the concierge were nice."
        Sentence sentiment: Positive
        Sentence positive score: 0.84
        Sentence negative score: 0.16
        Sentence neutral score: 0.00

        Aspect: food, Value: Negative
        Aspect positive score: 0.01
        Aspect negative score: 0.99
                Related Opinion: unacceptable, Value: Negative
                Related Opinion positive score: 0.01
                Related Opinion negative score: 0.99
        Aspect: service, Value: Negative
        Aspect positive score: 0.01
        Aspect negative score: 0.99
                Related Opinion: unacceptable, Value: Negative
                Related Opinion positive score: 0.01
                Related Opinion negative score: 0.99
        Aspect: concierge, Value: Positive
        Aspect positive score: 1.00
        Aspect negative score: 0.00
                Related Opinion: nice, Value: Positive
                Related Opinion positive score: 1.00
                Related Opinion negative score: 0.00


Press any key to exit.
```

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)

Cree una función denominada `SentimentAnalysisExample()` que tome el cliente que creó anteriormente y llame a su función `AnalyzeSentiment()`. El objeto `Response<DocumentSentiment>` devuelto contendrá la etiqueta de opinión y la puntuación del documento de entrada completo, así como un análisis de opinión de cada oración si la operación se realiza correctamente. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Output

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una función denominada `SentimentAnalysisExample()` que tome el cliente que creó anteriormente y llame a su función [Sentiment()](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment). El objeto [SentimentResult](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult) que se devuelve contendrá la opinión `Score` si la operación se ha realizado correctamente, y `errorMessage` si no ha sido así. 

Una puntuación cercana a 0 indica una opinión negativa, mientras que una puntuación cercana a 1 indica una opinión positiva.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>Detección de idiomas

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)


Cree una función denominada `LanguageDetectionExample()` que tome el cliente que creó anteriormente y llame a su función `DetectLanguage()`. El objeto `Response<DetectedLanguage>` devuelto contendrá el idioma detectado junto con su nombre y el código ISO-6391. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

> [!Tip]
> En algunos casos, puede ser difícil eliminar la ambigüedad de los idiomas en función de la entrada. Puede usar el parámetro `countryHint` para especificar un código de país o región de dos letras. De forma predeterminada, la API usa "US" como valor predeterminado de countryHint; para eliminar este comportamiento, puede restablecer este parámetro y configurarlo como una cadena `countryHint = ""` vacía. Para establecer un valor predeterminado diferente, establezca la propiedad `TextAnalyticsClientOptions.DefaultCountryHint` y pásela durante la inicialización del cliente.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Output

```console
Language:
        French, ISO-6391: fr
```

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)


Cree una función denominada `LanguageDetectionExample()` que tome el cliente que creó anteriormente y llame a su función `DetectLanguage()`. El objeto `Response<DetectedLanguage>` devuelto contendrá el idioma detectado junto con su nombre y el código ISO-6391. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

> [!Tip]
> En algunos casos, puede ser difícil eliminar la ambigüedad de los idiomas en función de la entrada. Puede usar el parámetro `countryHint` para especificar un código de país o región de dos letras. De forma predeterminada, la API usa "US" como valor predeterminado de countryHint; para eliminar este comportamiento, puede restablecer este parámetro y configurarlo como una cadena `countryHint = ""` vacía. Para establecer un valor predeterminado diferente, establezca la propiedad `TextAnalyticsClientOptions.DefaultCountryHint` y pásela durante la inicialización del cliente.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Output

```console
Language:
        French, ISO-6391: fr
```

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una función denominada `languageDetectionExample()` que tome el cliente que creó anteriormente y llame a su función [DetectLanguage()](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_). El objeto [LanguageResult](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult) devuelto contendrá la lista de idiomas detectados en `DetectedLanguages` si la operación se ha realizado correctamente, y `errorMessage` si no ha sido así. Imprima el primer idioma devuelto.

> [!Tip]
> En algunos casos, puede ser difícil eliminar la ambigüedad de los idiomas en función de la entrada. Puede usar el parámetro `countryHint` para especificar un código de país o región de dos letras. De forma predeterminada, la API usa "US" como valor de countryHint predeterminado; para quitar este comportamiento, puede restablecer este parámetro y configurarlo como una cadena `countryHint = ""` vacía.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>Resultados

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)


Cree una función denominada `EntityRecognitionExample()` que tome el cliente que creó anteriormente, llame a su función `RecognizeEntities()` y recorra en iteración los resultados. El objeto `Response<CategorizedEntityCollection>` devuelto contendrá la colección de las entidades detectadas `CategorizedEntity`. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2},\tLength: {entity.Length},\tOffset: {entity.Offset}\n");
    }
}
```

### <a name="output"></a>Output

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61,    Length: 4,      Offset: 18

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82,    Length: 7,      Offset: 26

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80,    Length: 9,      Offset: 34
```

### <a name="entity-linking"></a>Vinculación de entidad

Cree una función denominada `EntityLinkingExample()` que tome el cliente que creó anteriormente, llame a su función `RecognizeLinkedEntities()` y recorra en iteración los resultados. El objeto `Response<LinkedEntityCollection>` devuelto contendrá la colección de las entidades detectadas `LinkedEntity`. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`. Puesto que las entidades vinculadas se identifican de forma única, las apariciones de la misma entidad se agrupan bajo un objeto `LinkedEntity` como una lista de objetos `LinkedEntityMatch`.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}");
            Console.WriteLine($"\t\tLength: {match.Length}");
            Console.WriteLine($"\t\tOffset: {match.Offset}\n");
        }
    }
}
```

### <a name="output"></a>Output

```console
Linked Entities:
        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 0

                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 150

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63
                Length: 10
                Offset: 25

                Text: Gates
                Score: 0.63
                Length: 5
                Offset: 161

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60
                Length: 10
                Offset: 40

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32
                Length: 7
                Offset: 54

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
                Length: 5
                Offset: 89

        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88
                Length: 11
                Offset: 116
```

### <a name="personally-identifiable-information-recognition"></a>Reconocimiento de información de identificación personal

Cree una función denominada `RecognizePIIExample()` que tome el cliente que creó anteriormente, llame a su función `RecognizePiiEntities()` y recorra en iteración los resultados. El `PiiEntityCollection` devuelto representa la lista de las entidades de información de identificación personal detectadas. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void RecognizePIIExample(TextAnalyticsClient client)
{
    string document = "A developer with SSN 859-98-0987 whose phone number is 800-102-1100 is building tools with our APIs.";

    PiiEntityCollection entities = client.RecognizePiiEntities(document).Value;

    Console.WriteLine($"Redacted Text: {entities.RedactedText}");
    if (entities.Count > 0)
    {
        Console.WriteLine($"Recognized {entities.Count} PII entit{(entities.Count > 1 ? "ies" : "y")}:");
        foreach (PiiEntity entity in entities)
        {
            Console.WriteLine($"Text: {entity.Text}, Category: {entity.Category}, SubCategory: {entity.SubCategory}, Confidence score: {entity.ConfidenceScore}");
        }
    }
    else
    {
        Console.WriteLine("No entities were found.");
    }
}
```

### <a name="output"></a>Resultados

```console
Redacted Text: A developer with SSN *********** whose phone number is ************ is building tools with our APIs.
Recognized 2 PII entities:
Text: 859-98-0987, Category: U.S. Social Security Number (SSN), SubCategory: , Confidence score: 0.65
Text: 800-102-1100, Category: Phone Number, SubCategory: , Confidence score: 0.8
```

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)


> [!NOTE]
> Nuevo en la versión `3.0`:
> * La vinculación de entidad ahora es independiente del reconocimiento de entidades.


Cree una función denominada `EntityRecognitionExample()` que tome el cliente que creó anteriormente, llame a su función `RecognizeEntities()` y recorra en iteración los resultados. El objeto `Response<IReadOnlyCollection<CategorizedEntity>>` devuelto contendrá la lista de las entidades detectadas. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>Output

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80
```

### <a name="entity-linking"></a>Vinculación de entidad

Cree una función denominada `EntityLinkingExample()` que tome el cliente que creó anteriormente, llame a su función `RecognizeLinkedEntities()` y recorra en iteración los resultados. El `Response<IReadOnlyCollection<LinkedEntity>>` devuelto representa la lista de las entidades detectadas. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`. Puesto que las entidades vinculadas se identifican de forma única, las apariciones de la misma entidad se agrupan bajo un objeto `LinkedEntity` como una lista de objetos `LinkedEntityMatch`.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>Output

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63

                Text: Gates
                Score: 0.63

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55

                Text: Microsoft
                Score: 0.55

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
```

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

> [!NOTE]
> En la versión 2.1, la vinculación de entidad se incluye en la respuesta de NER.

Cree una función denominada `RecognizeEntitiesExample()` que tome el cliente que creó anteriormente y llame a su función [Entities()](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_). Recorra en iteración los resultados. El objeto [EntitiesResult](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult) devuelto contendrá la lista de entidades detectadas en `Entities` si la operación se ha realizado correctamente, y `errorMessage` si no ha sido así. Para cada identidad detectada, imprima su tipo, subtipo y nombre de Wikipedia (si existen) y las ubicaciones del texto original.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


### <a name="key-phrase-extraction"></a>Extracción de la frase clave

# <a name="version-31-preview"></a>[Versión 3.1 (versión preliminar)](#tab/version-3-1)

Cree una función denominada `KeyPhraseExtractionExample()` que tome el cliente que creó anteriormente y llame a su función `ExtractKeyPhrases()`. El objeto `<Response<KeyPhraseCollection>` devuelto contendrá la lista de las frases clave detectadas. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Resultados

```console
Key phrases:
    cat
    veterinarian
```

# <a name="version-30"></a>[Versión 3.0](#tab/version-3)

Cree una función denominada `KeyPhraseExtractionExample()` que tome el cliente que creó anteriormente y llame a su función `ExtractKeyPhrases()`. El objeto `<Response<IReadOnlyCollection<string>>` devuelto contendrá la lista de las frases clave detectadas. Si se produjeron errores, se producirá lo siguiente: `RequestFailedException`.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

# <a name="version-21"></a>[Versión 2.1](#tab/version-2)

Cree una función denominada `KeyPhraseExtractionExample()` que tome el cliente que creó anteriormente y llame a su función [KeyPhrases()](/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_). El resultado contendrá la lista de frases clave detectadas en `KeyPhrases` si la operación ha tenido éxito, o `errorMessage` si no. Imprima las frases clave detectadas.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

---