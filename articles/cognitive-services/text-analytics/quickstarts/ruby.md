---
title: 'Guía de inicio rápido: Uso de Ruby para llamar a Text Analytics API'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido se muestra cómo usar Ruby para obtener información y ejemplos de código que le ayuden a empezar a usar rápidamente la API Text Analytics en Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/06/2020
ms.author: aahi
ms.openlocfilehash: 076276068b62ed1b7b30864e9a4227cd449c680e
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90527231"
---
# <a name="quickstart-using-ruby-to-call-the-text-analytics-cognitive-service"></a>Guía de inicio rápido: Uso de Ruby para llamar a Text Analytics de Cognitive Services
<a name="HOLTop"></a>

En este artículo se muestra cómo [detectar el idioma](#Detect), [analizar las opiniones](#SentimentAnalysis), [extraer las frases clave](#KeyPhraseExtraction) e [identificar las entidades vinculadas](#Entities) mediante  [instancias de Text Analytics API](//go.microsoft.com/fwlink/?LinkID=759711)  con Ruby.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="Detect"></a>

## <a name="detect-language"></a>Detectar idioma

Language Detection API detecta el idioma de un documento de texto con el [método Detectar idioma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7).

1. Cree un nuevo proyecto de Ruby en su IDE favorito.
1. Agregue el código que se proporciona a continuación.
1. Copie la clave y el punto de conexión de Text Analytics en el código. 
1. Ejecute el programa.

```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

subscription_key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"

path = '/text/analytics/v3.0/languages'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'text' => 'This is a document written in English.' },
    { 'id' => '2', 'text' => 'Este es un document escrito en Español.' },
    { 'id' => '3', 'text' => '这是一个用中文写的文件' }
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Respuesta de detección de idioma**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
    "documents": [
        {
            "id": "1",
            "detectedLanguage": {
                "name": "English",
                "iso6391Name": "en",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "2",
            "detectedLanguage": {
                "name": "Spanish",
                "iso6391Name": "es",
                "confidenceScore": 1.0
            },
            "warnings": []
        },
        {
            "id": "3",
            "detectedLanguage": {
                "name": "Chinese_Simplified",
                "iso6391Name": "zh_chs",
                "confidenceScore": 1.0
            },
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```
<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Análisis de opinión

Sentiment Analysis API detecta la opinión de un conjunto de registros de texto mediante el [método Sentiment](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9). En el ejemplo siguiente se puntúan dos documentos, uno en inglés y otro en español.

1. Cree un nuevo proyecto de Ruby en su IDE favorito.
1. Agregue el código que se proporciona a continuación.
1. Copie la clave y el punto de conexión de Text Analytics en el código. 
1. Ejecute el programa.

```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

subscription_key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"

path = '/text/analytics/v3.0/sentiment'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'language' => 'en', 'text' => 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id' => '2', 'language' => 'es', 'text' => 'Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico.' }
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Respuesta de análisis de sentimiento**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "confidenceScores": {
                "positive": 1.0,
                "neutral": 0.0,
                "negative": 0.0
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "confidenceScores": {
                        "positive": 1.0,
                        "neutral": 0.0,
                        "negative": 0.0
                    },
                    "offset": 0,
                    "length": 102,
                    "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                }
            ],
            "warnings": []
        },
        {
            "id": "2",
            "sentiment": "negative",
            "confidenceScores": {
                "positive": 0.02,
                "neutral": 0.05,
                "negative": 0.93
            },
            "sentences": [
                {
                    "sentiment": "negative",
                    "confidenceScores": {
                        "positive": 0.02,
                        "neutral": 0.05,
                        "negative": 0.93
                    },
                    "offset": 0,
                    "length": 92,
                    "text": "Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico."
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Extracción de frases clave

Key Phrase Extraction API extrae frases clave de un documento de texto con el [método de frases clave](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c6). En el ejemplo siguiente se extraen frases clave de documentos tanto en inglés y como en español.

1. Cree un nuevo proyecto de Ruby en su IDE favorito.
1. Agregue el código que se proporciona a continuación.
1. Copie la clave y el punto de conexión de Text Analytics en el código.
1. Ejecute el programa.


```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

subscription_key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"

path = '/text/analytics/v3.0/keyPhrases'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'language' => 'en', 'text' => 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id' => '2', 'language' => 'es', 'text' => 'Si usted quiere comunicarse con Carlos, usted debe de llamarlo a su telefono movil. Carlos es muy responsable, pero necesita recibir una notificacion si hay algun problema.' },
    { 'id' => '3', 'language' => 'en', 'text' => 'The Grand Hotel is a new hotel in the center of Seattle. It earned 5 stars in my review, and has the classiest decor I\'ve ever seen.' },
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Respuesta de extracción de frases clave**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
    "documents": [
        {
            "id": "1",
            "keyPhrases": [
                "HDR resolution",
                "new XBox",
                "clean look"
            ],
            "warnings": []
        },
        {
            "id": "2",
            "keyPhrases": [
                "Carlos",
                "notificacion",
                "algun problema",
                "telefono movil"
            ],
            "warnings": []
        },
        {
            "id": "3",
            "keyPhrases": [
                "new hotel",
                "Grand Hotel",
                "review",
                "center of Seattle",
                "classiest decor",
                "stars"
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```
<a name="Entities"></a>

## <a name="entity-recognition"></a>Reconocimiento de entidades

Entities API extrae las entidades de un documento de texto mediante el [método Entities](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634). En el ejemplo siguiente se identifican las entidades de documentos en inglés.

1. Cree un nuevo proyecto de Ruby en su IDE favorito.
1. Agregue el código que se proporciona a continuación.
1. Copie la clave y el punto de conexión de Text Analytics en el código.
1. Ejecute el programa.

```ruby
# encoding: UTF-8

require 'net/https'
require 'uri'
require 'json'

subscription_key = "<paste-your-text-analytics-key-here>"
endpoint = "<paste-your-text-analytics-endpoint-here>"

path = '/text/analytics/v3.0/entities/recognition/general'

uri = URI(endpoint + path)

documents = { 'documents': [
    { 'id' => '1', 'language' => 'en', 'text' => 'Microsoft is an It company.' }
]}

puts 'Please wait a moment for the results to appear.'

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/json"
request['Ocp-Apim-Subscription-Key'] = subscription_key
request.body = documents.to_json

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

puts JSON::pretty_generate (JSON (response.body))
```

**Respuesta de extracción de entidades**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "text": "Microsoft",
                    "category": "Organization",
                    "offset": 0,
                    "length": 9,
                    "confidenceScore": 0.86
                },
                {
                    "text": "IT",
                    "category": "Skill",
                    "offset": 16,
                    "length": 2,
                    "confidenceScore": 0.8
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Consulte también 

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes](../text-analytics-resource-faq.md)
