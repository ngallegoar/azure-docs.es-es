---
title: Método Transliterate de Translator
titleSuffix: Azure Cognitive Services
description: Convierta texto en un idioma de un script a otro con el método Transliterate de Translator.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 8b811c57eb163931c39a311418ac9f1513e9393a
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592260"
---
# <a name="translator-30-transliterate"></a>Translator 3.0: Transliterar

Convierte el texto en un idioma de un script a otro.

## <a name="request-url"></a>URL de la solicitud

Envíe una solicitud `POST` a:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
```

## <a name="request-parameters"></a>Parámetros de solicitud

Los parámetros de solicitud que se pasaron en la cadena de consulta son:

<table width="100%">
  <th width="20%">Parámetro de consulta</th>
  <th>Descripción</th>
  <tr>
    <td>api-version</td>
    <td>*Parámetro obligatorio*.<br/>Versión de la API que el cliente solicitó. El valor debe ser `3.0`.</td>
  </tr>
  <tr>
    <td>language</td>
    <td>*Parámetro obligatorio*.<br/>Especifica el idioma del texto que se va a convertir de un script a otro. Los idiomas posibles se enumeran en el ámbito `transliteration` obtenido al consultar el servicio de sus [idiomas admitidos](./v3-0-languages.md).</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Parámetro obligatorio*.<br/>Especifica el script que usa el texto de entrada. Busque los [idiomas admitidos](./v3-0-languages.md) con el ámbito `transliteration`, para ver los scripts de entrada disponibles para el idioma seleccionado.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Parámetro obligatorio*.<br/>Especifica el nombre del script de salida. Busque los [idiomas admitidos](./v3-0-languages.md) con el ámbito `transliteration`, para ver los scripts de salida disponibles para la combinación de idiomas y el script de entrada seleccionados.</td>
  </tr>
</table> 

Los encabezados de solicitud incluyen lo siguiente:

<table width="100%">
  <th width="20%">encabezados</th>
  <th>Descripción</th>
  <tr>
    <td>Encabezados de autenticación</td>
    <td><em>Encabezado de solicitud obligatorio</em>.<br/>Consulte las <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">opciones disponibles para la autenticación</a>.</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Encabezado de solicitud obligatorio*.<br/>Especifica el tipo de contenido de la carga. Los valores posibles son: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Encabezado de solicitud obligatorio*.<br/>Longitud del cuerpo de la solicitud.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Opcional*.<br/>GUID generado por el cliente para identificar de forma única la solicitud. Puede omitir este encabezado si incluye el id. de seguimiento en la cadena de la consulta mediante un parámetro de consulta denominado `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud es una matriz JSON. Cada elemento de la matriz es un objeto JSON con una propiedad de cadena denominada `Text`, que representa la cadena que se va a convertir.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

Se aplican las siguientes limitaciones:

* La matriz puede tener como máximo 10 elementos.
* El valor de texto de un elemento de la matriz no puede superar los 1 000 caracteres, incluyendo los espacios.
* El texto completo incluido en la solicitud no puede superar los 5000 caracteres, incluidos los espacios.

## <a name="response-body"></a>Response body

Una respuesta correcta es una matriz JSON, que cuenta con un resultado para cada elemento en la matriz de entrada. Un objeto del resultado incluye las siguientes propiedades:

  * `text`: cadena que es el resultado de convertir la cadena de entrada en el script de salida.
  
  * `script`: cadena que especifica el script que se usa en la salida.

Un ejemplo de respuesta JSON es:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
]
```

## <a name="response-headers"></a>Encabezados de respuesta

<table width="100%">
  <th width="20%">encabezados</th>
  <th>Descripción</th>
  <tr>
    <td>X-RequestId</td>
    <td>Valor generado por el servicio para identificar la solicitud. Se usa para solucionar problemas.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Códigos de estado de respuesta

A continuación se indican los códigos de estado HTTP posibles que devuelve una solicitud. 

<table width="100%">
  <th width="20%">Código de estado</th>
  <th>Descripción</th>
  <tr>
    <td>200</td>
    <td>Correcto.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Uno de los parámetros de consulta falta o no es válido. Corrija los parámetros de la solicitud antes de volver a intentarlo.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>No pudo autenticarse la solicitud. Compruebe que las credenciales que se especificaron sean correctas.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>La solicitud no está autenticada. Compruebe los detalles del mensaje de error. Esto a menudo indica que todas las traducciones gratuitas que proporciona la suscripción de prueba se han agotado.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>El servidor rechazó la solicitud porque el cliente superó los límites de solicitudes.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Se ha producido un error inesperado. Si el error continúa, notifíquelo con: fecha y hora del error, identificador de la solicitud del encabezado de respuesta `X-RequestId` e identificador de cliente del encabezado de solicitud `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Servidor no disponible temporalmente. Vuelva a intentarlo. Si el error continúa, notifíquelo con: fecha y hora del error, identificador de la solicitud del encabezado de respuesta `X-RequestId` e identificador de cliente del encabezado de solicitud `X-ClientTraceId`.</td>
  </tr>
</table> 

Si se produce un error, la solicitud también devuelve una respuesta de error JSON. El código de error es un número de 6 dígitos que combina el código de estado HTTP de 3 dígitos y otro número de 3 dígitos que ayuda a categorizar aún más el error. En la [página de referencia de Traductor v3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) pueden encontrarse los códigos de error comunes. 

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo convertir dos cadenas escritas en japonés a japonés escrito con el alfabeto latino.

Esta es la carga de JSON para la solicitud en este ejemplo:

```json
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Si está usando cURL en una ventana de línea de comandos que no admite caracteres Unicode, use la siguiente carga JSON y guárdela en un archivo denominado `request.txt`. Asegúrese de guardar el archivo con una codificación `UTF-8`.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```
