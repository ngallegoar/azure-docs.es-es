---
title: Traductor versión 2.0
titleSuffix: Azure Cognitive Services
description: Documentación de referencia para Traductor versión 2.0.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: fd0dbe5912b7c4df3c666c648dbf9a92d5398cf1
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94369518"
---
# <a name="translator-v20"></a>Traductor versión 2.0

> [!IMPORTANT]
> Esta versión de Traductor está en desuso. [Consulte la documentación de la versión 3 de Traductor](v3-0-reference.md).

La versión 2 de Traductor puede integrarse perfectamente en las aplicaciones, sitios web, herramientas u otras soluciones para proporcionar experiencias de usuario con varios idiomas. Se puede utilizar en cualquier plataforma de hardware y con cualquier sistema operativo para realizar la traducción de idiomas y otras operaciones lingüísticas relacionadas, como la detección del idioma de texto y texto a voz, según los estándares del sector. Para obtener más información, consulte [Traductor](../translator-info-overview.md).

## <a name="getting-started"></a>Introducción
Para acceder a Traductor, debe [suscribirse a Microsoft Azure](../translator-how-to-signup.md).

## <a name="authentication"></a>Authentication 
Todas las llamadas a Traductor requieren una clave de suscripción para la autenticación. La API admite tres métodos de autenticación:

- Un token de acceso. Use la clave de suscripción para crear un token de acceso mediante la realización de una solicitud POST al servicio de autenticación. Consulte la documentación de servicio de token para obtener más información. Pase el token de acceso al servicio de Translator con el encabezado de `Authorization` o el parámetro de consulta `access_token`. Cada token de acceso tiene una validez de 10 minutos. Obtenga un nuevo token de acceso cada 10 minutos y siga usando el mismo para solicitudes repetidas durante esos 10 minutos.
- Una clave de suscripción usada directamente. Pase la clave de suscripción como valor en el encabezado de `Ocp-Apim-Subscription-Key` junto con la solicitud a Traductor. Al usar la clave de suscripción directamente, no es necesario llamar al servicio de autenticación de token para crear un token de acceso.
- Una [suscripción a varios servicios de Azure Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). Este método le permite usar una única clave secreta para autenticar las solicitudes de varios servicios.
Si usa una clave secreta para varios servicios, debe incluir dos encabezados de autenticación con la solicitud. El primer encabezado pasa la clave secreta. El segundo encabezado especifica la región asociada con la suscripción:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

La región es obligatoria en la suscripción de varios servicios de Text API. La región que selecciona es la única región que puede usar para la traducción de texto al usar la clave de suscripción a varios servicios. Debe ser la misma región que seleccionó al registrarse para obtener la suscripción a varios servicios en Azure Portal.

Las regiones disponibles son `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus` y `westus2`.

La clave de suscripción y el token de acceso son secretos que deben ocultarse.

## <a name="profanity-handling"></a>Control de palabras soeces
Normalmente, el servicio de Translator conserva las palabras soeces que están presentes en el origen. El grado de blasfemia y el contexto que hace que las palabras sean soeces difieren según la cultura. Así pues, el grado de blasfemia en el idioma de destino podría ampliarse o reducirse.

Si quiere evitar palabras soeces en la traducción, incluso si se encuentran presentes en el texto de origen, puede usar la opción de filtrado de palabras soeces en los métodos que la admiten. La opción le permite elegir si quiere ver las palabras soeces eliminadas o marcadas con etiquetas adecuadas, o bien si quiere permitirlas en el destino. Los valores aceptados de `ProfanityAction` son `NoAction` (valor predeterminado), `Marked` y `Deleted`.


|ProfanityAction    |Acción |Ejemplo de origen (japonés)  |Ejemplo de traducción (español)  |
|:--|:--|:--|:--|
|NoAction   |Predeterminada. Igual que si no se configura la opción. Las palabras soeces pasarán del origen al destino.        |彼はジャッカスです。     |Es un idiota.   |
|Marked     |Las palabras soeces aparecerán rodeados por etiquetas XML \<profanity> y \</profanity>.       |彼はジャッカスです。 |Es un \<profanity>idiota\</profanity>.  |
|Deleted    |Las palabras soeces se quitarán de la salida sin reemplazo.     |彼はジャッカスです。 |Es un.   |

    
## <a name="excluding-content-from-translation"></a>Contenido excluido de la traducción
Al traducir el contenido con etiquetas, como HTML (`contentType=text/html`), a veces resulta útil para excluir contenido específico de la traducción. Puede utilizar el atributo `class=notranslate` para especificar contenido que debería permanecer en el idioma original. En el ejemplo siguiente, el contenido del primer elemento `div` no se traducirá, mientras que el del segundo elemento `div`, sí.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET /Translate

### <a name="implementation-notes"></a>Notas de implementación
Traduce una cadena de texto de un idioma a otro.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Valor devuelto:** Una cadena que representa el texto traducido.

Si anteriormente se usaba `AddTranslation` o `AddTranslationArray` para introducir una traducción con una clasificación de 5 o superior para la misma oración de origen, `Translate` devuelve solo la principal opción que está disponible para el sistema. "Misma oración de origen" significa exactamente lo mismo (coincidencia 100 %), excepto en el caso del uso de mayúsculas y minúsculas, espacio en blanco, valores de etiquetas y puntuación al final de una oración. Si no se almacena ninguna clasificación con una puntuación de 5 o superior, el resultado devuelto será la traducción automática de Microsoft Translator.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción    |Tipo de parámetro|tipo de datos|
|:--|:--|:--|:--|:--|
|appid  |(vacío)    |Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|text|(vacío)   |Necesario. Una cadena que representa el texto a traducir. El texto no puede contener más de 10 000 caracteres.|Query|string|
|desde|(vacío)   |Opcional. Una cadena que representa el código de idioma del texto que se traduce. Por ejemplo, en para inglés.|Query|string|
|to|(vacío) |Necesario. Una cadena que representa el código del idioma al que se va a traducir el texto.|Query|string|
|contentType|(vacío)    |Opcional. El formato del texto que se va a traducir. Los formatos admitidos son `text/plain` (valor predeterminado) y `text/html`. Cualquier elemento HTML debe ser un elemento completo y con formato correcto.|Query|string|
|category|(vacío)   |Opcional. una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es `general`.|Query|string|
|Authorization|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|


### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Notas de implementación
Recupera traducciones de varios textos de origen.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

Este es el formato del cuerpo de la solicitud:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

Estos elementos están en `TranslateArrayRequest`:


* `AppId`: Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `AppId`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.
* `From`: Opcional. Una cadena que representa el código de idioma del texto que se traduce. Si se deja vacío este campo, la respuesta incluirá el resultado de la detección automática de idioma.
* `Options`: Opcional. Un objeto `Options` que contiene los valores siguientes. Son todos opcionales y toman como valor predeterminado las opciones de configuración más comunes. Los elementos especificados deben incluirse en orden alfabético.
    - `Category`: una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es `general`.
    - `ContentType`: El formato del texto que se va a traducir. Los formatos admitidos son `text/plain` (valor predeterminado), `text/xml` y `text/html`. Cualquier elemento HTML debe ser un elemento completo y con formato correcto.
    - `ProfanityAction`: especifica cómo se tratan las blasfemias según se explicó anteriormente. Los valores aceptados son `NoAction` (valor predeterminado), `Marked` y `Deleted`.
    - `State`: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
    - `Uri`: Filtrar los resultados por este URI. Valor predeterminado: `all`.
    - `User`: Filtrar los resultados por este usuario. Valor predeterminado: `all`.
* `Texts`: Necesario. Una matriz que contiene el texto para la traducción. Todas las cadenas deben estar en el mismo idioma. El total de todo el texto que se debe traducir no puede superar los 10 000 caracteres. El número máximo de elementos de la matriz es 2000.
* `To`: Necesario. Una cadena que representa el código del idioma al que se va a traducir el texto.

Puede omitir los elementos opcionales. Los elementos que son elementos secundarios directos de `TranslateArrayRequest` deben aparecer en orden alfabético.

El método `TranslateArray` acepta `application/xml` o `text/xml` para `Content-Type`.

**Valor devuelto:** Una matriz `TranslateArrayResponse`. Cada `TranslateArrayResponse` tiene estos elementos:

* `Error`: indica un error si se produce. En caso contrario, se establece en null.
* `OriginalSentenceLengths`: una matriz de enteros que indica la longitud de cada oración del texto de origen. La longitud de la matriz indica el número de oraciones.
* `TranslatedText`: El texto traducido.
* `TranslatedSentenceLengths`: una matriz de enteros que indica la longitud de cada oración del texto traducido. La longitud de la matriz indica el número de oraciones.
* `State`: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Devuelve el mismo contenido que la solicitud.

Este es el formato del cuerpo de la respuesta:

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una respuesta correcta incluye una matriz de matrices `TranslateArrayResponse` en el formato descrito anteriormente.

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|Authorization|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP   |Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado. Estos son algunos de los errores comunes: <ul><li>El elemento de matriz no puede estar vacío.</li><li>Categoría no válida.</li><li>El idioma de origen no es válido.</li><li>El idioma de destino no es válido.</li><li>La solicitud contiene demasiados elementos.</li><li>El idioma de origen no se admite.</li><li>El idioma de destino no se admite.</li><li>La solicitud de traducción tiene demasiados datos.</li><li>HTML no tiene el formato correcto.</li><li>Se pasaron demasiadas cadenas en la solicitud de traducción.</li></ul>|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Notas de implementación
Recupera los nombres descriptivos de los idiomas que se pasaron como parámetro `languageCodes`, localizados en el idioma `locale` que se pasó.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

El cuerpo de la solicitud incluye una matriz de cadenas que representa los códigos de idioma ISO 639-1 para los que se van a recuperar los nombres descriptivos. Este es un ejemplo:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Valor devuelto:** una matriz de cadenas que contiene los nombres de idiomas que admite el servicio de Translator, localizados en el idioma solicitado.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de cadenas que contiene los nombres de idiomas que admite el servicio de Translator, localizados en el idioma solicitado.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|locale|(vacío) |Necesario. Una cadena que representa uno de los siguientes elementos, que se usa para localizar los nombres de idiomas: <ul><li>La combinación de un código de cultura en minúsculas de dos letras ISO 639 asociado a un idioma y un código de subcultura en mayúsculas de dos letras ISO 3166. <li>Un código de cultura en minúsculas ISO 639 por sí mismo.|Query|string|
|Authorization|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Notas de implementación
Obtiene una lista de códigos de idioma que representan los idiomas que admite el servicio de traducción.  `Translate` y `TranslateArray` puede traducir entre cualquier par de estos idiomas.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Valor devuelto:** Una matriz de cadenas que contiene los códigos de idioma que admite el servicio de Translator.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de cadenas que contiene los códigos de idioma que admite el servicio de Translator.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|Authorization|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-getlanguagesforspeak"></a>GET /GetLanguagesForSpeak

### <a name="implementation-notes"></a>Notas de implementación
Recupera los idiomas disponibles para la síntesis de voz.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Valor devuelto:** Una matriz de cadenas que contiene los códigos de idioma que admite la síntesis de voz del servicio de Translator.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de cadenas que contiene los códigos de idioma que admite la síntesis de voz del servicio de Translator.

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|
 
### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401|Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-speak"></a>GET /Speak

### <a name="implementation-notes"></a>Notas de implementación
Devuelve una secuencia con formato WAV o MP3 del texto que se ha pasado reproducido en el idioma deseado.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Valor devuelto:** una secuencia con formato WAV o MP3 del texto que se ha pasado reproducido en el idioma deseado.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

binary

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|text|(vacío)   |Necesario. Una cadena que contiene una o varias oraciones que se van a reproducir para la secuencia en el idioma especificado. El texto no debe tener más de 2000 caracteres.|Query|string|
|language|(vacío)   |Necesario. Una cadena que representa el código de idioma admitido del idioma en el que se va a reproducir el texto. El código debe ser uno de los códigos devueltos por el método `GetLanguagesForSpeak`.|Query|string|
|format|(vacío)|Opcional. Una cadena que especifica el Id. de content-type. Actualmente, `audio/wav` y `audio/mp3` están disponibles. El valor predeterminado es `audio/wav`.|Query|string|
|opciones|(vacío)    |Opcional. Una cadena que especifica propiedades de la voz sintetizada:<ul><li>`MaxQuality` y `MinSize` especifican la calidad de la señal de audio. `MaxQuality` proporciona la máxima calidad. `MinSize` proporciona el tamaño de archivo más pequeño. El valor predeterminado es `MinSize`.</li><li>`female` y `male` especifican el género de voz deseado. El valor predeterminado es `female`. Use la barra vertical (<code>\|</code>) para incluir varias opciones. Por ejemplo, `MaxQuality|Male`.</li></li></ul>  |Query|string|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-detect"></a>GET /Detect

### <a name="implementation-notes"></a>Notas de implementación
Identifica el idioma de una sección de texto.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Valor devuelto:** una cadena que contiene un código de idioma de dos caracteres para el texto.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)  |Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|text|(vacío)|Necesario. Una cadena que contiene el texto cuyo idioma se va a identificar. El texto no debe tener más de 10 000 caracteres.|Query|  string|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key  |(vacío)    |Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|


## <a name="post-detectarray"></a>POST /DetectArray

### <a name="implementation-notes"></a>Notas de implementación

Identifica los idiomas en una matriz de cadenas. Detecta de forma independiente el idioma de cada elemento de matriz individual y devuelve un resultado para cada fila de la matriz.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

Este es el formato del cuerpo de la solicitud:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

El texto no puede tener más de 10 000 caracteres.

**Valor devuelto:** una matriz de cadenas que contiene un código de idioma de dos caracteres para cada fila de la matriz de entrada.

Este es el formato del cuerpo de la respuesta:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
`DetectArray` se realizó correctamente. Devuelve una matriz de cadenas que contiene un código de idioma de dos caracteres para cada fila de la matriz de entrada.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos.  Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-addtranslation"></a>GET /AddTranslation

### <a name="implementation-notes"></a>Notas de implementación

> [!IMPORTANT]
> **Nota de desuso:** Después del 31 de enero de 2018, este método no aceptará nuevos envíos de oraciones. Obtendrá un mensaje de error. Consulte el anuncio acerca de los cambios realizados en Collaborative Translation Framework (CTF).

Agrega una traducción a la memoria de traducción.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application: xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos   |
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|originalText|(vacío)|Necesario. Una cadena que representa el texto a traducir. La longitud máxima de la cadena es de 1000 caracteres.|Query|string|
|translatedText|(vacío) |Necesario. Una cadena que contiene texto traducido en el idioma de destino. La longitud máxima de la cadena es de 2000 caracteres.|Query|string|
|desde|(vacío)   |Necesario. Una cadena que representa el código de idioma del idioma original del texto. Por ejemplo, en para inglés y de para alemán.|Query|string|
|to|(vacío)|Necesario. Una cadena que representa el código de idioma del idioma al que se va a traducir el texto.|Query|string|
|rating|(vacío) |Opcional. Un entero que representa la clasificación de calidad de la cadena. El valor se encuentra entre -10 y 10. El valor predeterminado es 1.|Query|integer|
|contentType|(vacío)    |Opcional. El formato del texto que se va a traducir. Los formatos admitidos son `text/plain` y `text/html`. Cualquier elemento HTML debe ser un elemento completo y con formato correcto.    |Query|string|
|category|(vacío)|Opcional. una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es `general`.|Query|string|
|usuario|(vacío)|Necesario. Una cadena que se usa para realizar un seguimiento del originador del envío.|Query|string|
|uri|(vacío)|Opcional. Una cadena que contiene la ubicación del contenido de la traducción.|Query|string|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos.  Token de autorización: `"Bearer" + " " + "access_token"`.  |encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|410|`AddTranslation` ya no se admite.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Notas de implementación

> [!IMPORTANT]
> **Nota de desuso:** Después del 31 de enero de 2018, este método no aceptará nuevos envíos de oraciones. Obtendrá un mensaje de error. Consulte el anuncio acerca de los cambios realizados en Collaborative Translation Framework (CTF).

Agrega una matriz de traducciones a la memoria de traducción. Este método es una versión de la matriz de `AddTranslation`.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

Este es el formato del cuerpo de la solicitud:

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

Estos elementos están en `AddtranslationsRequest`:

* `AppId`: Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `AppId`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.
* `From`: Necesario. Una cadena que contiene el código de idioma del idioma de origen. Debe ser uno de los idiomas devueltos por el método `GetLanguagesForTranslate`.
* `To`: Necesario. Una cadena que contiene el código de idioma del idioma de destino. Debe ser uno de los idiomas devueltos por el método `GetLanguagesForTranslate`.
* `Translations`: Necesario. Una matriz de traducciones para agregar memoria de traducción. Cada traducción debe contener `OriginalText`, `TranslatedText` y `Rating`. El tamaño máximo de cada `OriginalText` y `TranslatedText` es 1000 caracteres. El total de todos los elementos `OriginalText` y `TranslatedText` no puede ser superior a 10 000 caracteres. El número máximo de elementos de la matriz es 100.
* `Options`: Necesario. Un conjunto de opciones, entre las que se incluyen `Category`, `ContentType`, `Uri` y `User`. `User` es obligatorio. `Category`, `ContentType` y `Uri` son opcionales. Los elementos especificados deben incluirse en orden alfabético.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
El método `AddTranslationArray` se ha llevado a cabo de forma correcta. 

Después del 31 de enero de 2018, no se aceptarán envíos de oraciones. El servicio responderá con el código de error 410.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos.  Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|410    |`AddTranslation` ya no se admite.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-breaksentences"></a>GET /BreakSentences

### <a name="implementation-notes"></a>Notas de implementación
Divide una sección de texto en oraciones y devuelve una matriz que contiene la longitud de cada oración.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Valor devuelto:** Una matriz de enteros que representa las longitudes de las oraciones. La longitud de la matriz representa el número de oraciones. Los valores representan la longitud de cada oración.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de enteros que representa las longitudes de las oraciones. La longitud de la matriz representa el número de oraciones. Los valores representan la longitud de cada oración.

integer

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)  |Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query| string|
|text|(vacío)   |Necesario. Una cadena que representa el texto que se va a dividir en oraciones. El tamaño máximo del texto es 10 000 caracteres.|Query|string|
|language   |(vacío)    |Necesario. Una cadena que representa el código de idioma del texto de entrada.|Query|string|
|Authorization|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.   |encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401|Credenciales no válidas.|
|500|Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Notas de implementación
Recupera una matriz de traducciones de un par de idiomas determinado desde el almacén y el motor de traducción automática. `GetTranslations` es diferente de `Translate` en que devuelve todas las traducciones disponibles.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

El cuerpo de la solicitud incluye el objeto opcional `TranslationOptions`, que tiene este formato:

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

El objeto `TranslateOptions` contiene los valores en la siguiente lista. Son todos opcionales y toman como valor predeterminado las opciones de configuración más comunes. Los elementos especificados deben incluirse en orden alfabético.

* `Category`: una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es `general`.
* `ContentType`: la única opción admitida, así como la predeterminada, es `text/plain`.
* `IncludeMultipleMTAlternatives`: una marca booleana para especificar si se debe devolver más de una alternativa del motor de traducción automática. Los valores válidos son `true` y `false` (distinguen mayúsculas de minúsculas). El valor predeterminado es `false`, que devuelve solo una alternativa. El establecimiento de la marca en `true` permite crear alternativas artificiales en la traducción, totalmente integradas con Collaborative Translation Framework (CTF). La característica permite la devolución de alternativas de oraciones que no tienen ninguna traducción en CTF agregando alternativas artificiales de la lista de las *N* mejores opciones del descodificador.
    - Clasificaciones. Las clasificaciones se aplican de la siguiente manera: 
         - la mejor traducción automática tiene una clasificación de 5.
       - Las alternativas de CTF reflejan la autoridad del revisor. Varían de -10 a +10.
       - Las alternativas de traducción (las *N* mejores) generadas automáticamente tienen una clasificación de 0 y un grado de coincidencia de 100.
    - Número de alternativas. El número de alternativas devueltas puede ser tan alto como el nivel especificado en `maxTranslations`, pero puede ser más bajo.
    - Pares de idiomas. Esta funcionalidad no está disponible para las traducciones entre chino simplificado y tradicional, en ambas direcciones. Está disponible para el resto de pares de idiomas admitidos de Microsoft Translator.
* `State`: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
* `Uri`: Filtrar los resultados por este URI. Si no se establece ningún valor, el valor predeterminado es `all`.
* `User`: Filtrar los resultados por este usuario. Si no se establece ningún valor, el valor predeterminado es `all`.

La solicitud `Content-Type` debe ser `text/xml`.

**Valor devuelto:** Este es el formato de la respuesta:

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Esta respuesta incluye un elemento `GetTranslationsResponse` que contiene los valores siguientes:

* `Translations`: una matriz de las coincidencias encontradas, almacenada en los objetos `TranslationMatch` (descritos en la siguiente sección). Las traducciones pueden incluir ligeras variantes del texto original (coincidencia aproximada). Las traducciones se ordenarán: las coincidencias del 100 % en primer lugar, luego las coincidencias aproximadas.
* `From`: si el método no especifica un idioma `From`, este valor procederá de la detección de idioma automática. En caso contrario, será el idioma `From` especificado.
* `State`: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Contiene el valor proporcionado en el parámetro `TranslateOptions`.

El objeto `TranslationMatch` consta de estos valores:

* `Error`: el código de error, si se ha producido un error de una cadena de entrada específica. De lo contrario, el campo está vacío.
* `MatchDegree`: indica el grado de coincidencia del texto de entrada con el texto original encontrado en el almacén. El sistema hace coincidir las oraciones según el almacén, incluidas las coincidencias inexactas. El valor devuelto oscila entre 0 y 100, donde 0 equivale a ninguna similitud y 100 a una coincidencia exacta con distinción de mayúsculas y minúsculas.
* `MatchedOriginalText`: El texto original del que se encontraron coincidencias para este resultado. Este valor solo se devuelve si el texto original de la coincidencia era diferente del texto de entrada. Se utiliza para devolver el texto de origen de una coincidencia aproximada. Este valor no se devuelve para los resultados de Microsoft Translator.
* `Rating`: Indica la autoridad de la persona que toma la decisión de calidad. Los resultados de la traducción automática tienen una clasificación de 5. Generalmente, las traducciones proporcionadas de forma anónima tienen una clasificación de 1 a 4. Generalmente, las traducciones proporcionadas de forma autoritaria tienen una clasificación de 6 a 10.
* `Count`: El número de veces que se ha seleccionado esta traducción con esta clasificación. El valor es 0 para la respuesta traducida automáticamente.
* `TranslatedText`: El texto traducido.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Un objeto `GetTranslationsResponse` en el formato que se ha descrito anteriormente.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje vacío el campo `appid`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|Query|string|
|text|(vacío)|Necesario. Una cadena que representa el texto a traducir. El tamaño máximo del texto es 10 000 caracteres.|Query|string|
|desde|(vacío)|Necesario. Una cadena que representa el código de idioma del texto que se traduce.|Query|string|
|to |(vacío)    |Necesario. Una cadena que representa el código de idioma del idioma al que se va a traducir el texto.|Query|string|
|maxTranslations|(vacío)|Necesario. Un entero que representa el número máximo de traducciones que se van a devolver.|Query|integer|
|Authorization| (vacío)|Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos. Token de autorización: `"Bearer" + " " + "access_token"`.|string|  encabezado|
|Ocp-Apim-Subscription-Key|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

### <a name="implementation-notes"></a>Notas de implementación
Recupere varios candidatos de traducción de varios textos de origen.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

Este es el formato del cuerpo de la solicitud:

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest` incluye estos elementos:

* `AppId`: Necesario. Si se usa el encabezado `Authorization`, deje vacío el campo `AppId`. De lo contrario, incluya una cadena que contenga `"Bearer" + " " + "access_token"`.
* `From`: Necesario. Una cadena que representa el código de idioma del texto que se traduce.
* `MaxTranslations`: Necesario. Un entero que representa el número máximo de traducciones que se van a devolver.
* `Options`: Opcional. Un objeto `Options` que contiene los valores siguientes. Son todos opcionales y toman como valor predeterminado las opciones de configuración más comunes. Los elementos especificados deben incluirse en orden alfabético.
    - `Category`: una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es `general`.
    - `ContentType`: la única opción admitida, así como la predeterminada, es `text/plain`.
    - `IncludeMultipleMTAlternatives`: una marca booleana para especificar si se debe devolver más de una alternativa del motor de traducción automática. Los valores válidos son `true` y `false` (distinguen mayúsculas de minúsculas). El valor predeterminado es `false`, que devuelve solo una alternativa. El establecimiento de la marca en `true` permite generar alternativas artificiales en la traducción, totalmente integradas con Collaborative Translation Framework (CTF). La característica permite la devolución de alternativas de oraciones que no tienen ninguna alternativa en CTF agregando alternativas artificiales de la lista de las *N* mejores opciones del descodificador.
        - Clasificaciones. Las clasificaciones se aplican de la siguiente manera:
          - la mejor traducción automática tiene una clasificación de 5.
          - Las alternativas de CTF reflejan la autoridad del revisor. Varían de -10 a +10.
          - Las alternativas de traducción (las *N* mejores) generadas automáticamente tienen una clasificación de 0 y un grado de coincidencia de 100.
        - Número de alternativas. El número de alternativas devueltas puede ser tan alto como el nivel especificado en `maxTranslations`, pero puede ser más bajo.
        - Pares de idiomas. Esta funcionalidad no está disponible para las traducciones entre chino simplificado y tradicional, en ambas direcciones. Está disponible para el resto de pares de idiomas admitidos de Microsoft Translator.
* `State`: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
* `Uri`: Filtrar los resultados por este URI. Si no se establece ningún valor, el valor predeterminado es `all`.
* `User`: Filtrar los resultados por este usuario. Si no se establece ningún valor, el valor predeterminado es `all`.
* `Texts`: Necesario. Una matriz que contiene el texto para la traducción. Todas las cadenas deben estar en el mismo idioma. El total de todo el texto que se debe traducir no puede superar los 10 000 caracteres. El número máximo de elementos de la matriz es 10.
* `To`: Necesario. Una cadena que representa el código de idioma del idioma al que se va a traducir el texto.

Puede omitir los elementos opcionales. Los elementos que son elementos secundarios directos de `GetTranslationsArrayRequest` deben aparecer en orden alfabético.

La solicitud `Content-Type` debe ser `text/xml`.

**Valor devuelto:** Este es el formato de la respuesta:

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Cada elemento `GetTranslationsResponse` contiene estos valores:

* `Translations`: una matriz de las coincidencias encontradas, almacenada en los objetos `TranslationMatch` (descritos en la siguiente sección). Las traducciones pueden incluir ligeras variantes del texto original (coincidencia aproximada). Las traducciones se ordenarán: las coincidencias del 100 % en primer lugar, luego las coincidencias aproximadas.
* `From`: si el método no especifica un idioma `From`, este valor procederá de la detección de idioma automática. En caso contrario, será el idioma `From` especificado.
* `State`: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Contiene el valor proporcionado en el parámetro `TranslateOptions`.

El objeto `TranslationMatch` contiene los valores siguientes:
* `Error`: el código de error, si se ha producido un error de una cadena de entrada específica. De lo contrario, el campo está vacío.
* `MatchDegree`: indica el grado de coincidencia del texto de entrada con el texto original encontrado en el almacén. El sistema hace coincidir las oraciones según el almacén, incluidas las coincidencias inexactas. El valor devuelto oscila entre 0 y 100, donde 0 equivale a ninguna similitud y 100 a una coincidencia exacta con distinción de mayúsculas y minúsculas.
* `MatchedOriginalText`: El texto original del que se encontraron coincidencias para este resultado. Este valor solo se devuelve si el texto original de la coincidencia era diferente del texto de entrada. Se utiliza para devolver el texto de origen de una coincidencia aproximada. Este valor no se devuelve para los resultados de Microsoft Translator.
* `Rating`: Indica la autoridad de la persona que toma la decisión de calidad. Los resultados de la traducción automática tienen una clasificación de 5. Generalmente, las traducciones proporcionadas de forma anónima tienen una clasificación de 1 a 4. Generalmente, las traducciones proporcionadas de forma autoritaria tienen una clasificación de 6 a 10.
* `Count`: El número de veces que se ha seleccionado esta traducción con esta clasificación. El valor es 0 para la respuesta traducida automáticamente.
* `TranslatedText`: El texto traducido.


### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Value|Descripción|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|Authorization  |(vacío)    |Se requiere tanto si el campo `appid` como el encabezado `Ocp-Apim-Subscription-Key` se dejan vacíos.  Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Se requiere tanto si el campo `appid` como el encabezado `Authorization` se dejan vacíos.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Migración a Traductor versión 3](../migrate-to-v3.md)