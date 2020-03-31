---
title: 'Inicio rápido: Revisión ortográfica con la API REST y Ruby: Bing Spell Check'
titleSuffix: Azure Cognitive Services
description: Empiece a utilizar la API REST de Bing Spell Check para la revisión ortográfica y gramatical con este inicio rápido.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: aahi
ms.openlocfilehash: 89a2a345e2a4e3ca1be31297e614e86f800e6316
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448429"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-ruby"></a>Inicio rápido: Revisión ortográfica con la API REST de Bing Spell Check y Ruby

Use este inicio rápido para realizar la primera llamada a la API REST de Bing Spell Check mediante Ruby. Esta sencilla aplicación envía una solicitud a la API y devuelve una lista de palabras que no reconoce, seguida de las correcciones sugeridas. Si bien esta aplicación está escrita en Ruby, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación. El código fuente de esta aplicación está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingSpellCheckv7.rb).

## <a name="prerequisites"></a>Prerrequisitos

* [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) o posterior.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Ruby en su editor o IDE favoritos y agregue los siguientes requisitos. 

    ```ruby
    require 'net/http'
    require 'uri'
    require 'json'
    ```

2. Cree variables para la clave de suscripción, el URI del punto de conexión y la ruta de acceso. Para crear los parámetros de solicitud, anexe el parámetro `mkt=` a su mercado y `&mode` al modo `proof`. Puede usar el punto de conexión global siguiente o el punto de conexión del [subdominio personalizado](../../../cognitive-services/cognitive-services-custom-subdomains.md) que se muestra en Azure Portal para el recurso.

    ```ruby
    key = 'ENTER YOUR KEY HERE'
    uri = 'https://api.cognitive.microsoft.com'
    path = '/bing/v7.0/spellcheck?'
    params = 'mkt=en-us&mode=proof'
    ```

## <a name="send-a-spell-check-request"></a>Envío de una solicitud de revisión ortográfica

1. Cree un URI a partir del URI de host, la ruta de acceso y la cadena de parámetros. Establezca su consulta para que contenga el texto que quiere revisar.

   ```ruby
   uri = URI(uri + path + params)
   uri.query = URI.encode_www_form({
      # Request parameters
   'text' => 'Hollo, wrld!'
   })
   ```

2. Cree una solicitud mediante el URI creado anteriormente. Agregue su clave al encabezado `Ocp-Apim-Subscription-Key`.

    ```ruby
    request = Net::HTTP::Post.new(uri)
    request['Content-Type'] = "application/x-www-form-urlencoded"
    request['Ocp-Apim-Subscription-Key'] = key
    ```

3. Envíe la solicitud.

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request(request)
    end
    ```

4. Obtenga la respuesta JSON e imprímala en la consola. 

    ```ruby
    result = JSON.pretty_generate(JSON.parse(response.body))
    puts result
    ```

## <a name="run-the-application"></a>Ejecución de la aplicación

Compile y ejecute el proyecto.

Si usa la línea de comandos, utilice los siguientes comandos para ejecutar la aplicación.

```bash
ruby <FILE_NAME>.rb
```

## <a name="example-json-response"></a>Ejemplo de respuesta JSON

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../tutorials/spellcheck.md)

- [¿Qué es Bing Spell Check API?](../overview.md)
- [Referencia de Bing Spell Check API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
