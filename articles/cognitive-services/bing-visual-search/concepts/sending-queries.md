---
title: Envío de consultas de búsqueda a Bing Visual Search API
titleSuffix: Azure Cognitive Services
description: En este artículo se describen los parámetros y atributos de las solicitudes enviadas a Bing Visual Search API, así como el objeto de respuesta.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: aahi
ms.openlocfilehash: 331b2ffde8d034ba94f5b1adcae5efa223f57594
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93095125"
---
# <a name="sending-search-queries-to-the-bing-visual-search-api"></a>Envío de consultas de búsqueda a Bing Visual Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

En este artículo se describen los parámetros y atributos de las solicitudes enviadas a Bing Visual Search API, así como el objeto de respuesta. 

Puede obtener información acerca de una imagen de tres maneras:

- Mediante un token de conclusión que obtiene de una imagen en una llamada anterior a uno de los puntos de conexión de [Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
- Enviando la dirección URL de una imagen.
- Carga de una imagen (en formato binario).

## <a name="bing-visual-search-requests"></a>Solicitudes de Bing Visual Search

Si envía a Visual Search una dirección URL o un token de imagen, el fragmento de código siguiente muestra el objeto JSON que hay que incluir en el cuerpo de POST:

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

El objeto `imageInfo` debe incluir el campo `url` o `imageInsightsToken`, pero no ambos. Defina el campo `url` con la dirección URL de una imagen que sea accesible a través de Internet. El tamaño máximo de imagen admitido es 1 MB.

`imageInsightsToken` debe establecerse en un token de conclusión. Para obtener un token de conclusión, llame a Bing Image API. La respuesta contiene una lista de objetos `Image`. Cada objeto `Image` contiene un campo `imageInsightsToken`, que contiene el token.

El campo `cropArea` es opcional. El área de recorte especifica las esquinas superior izquierda e inferior derecha de una región de interés. Especifique los valores en el intervalo de 0.0 a 1.0. Los valores son un porcentaje del ancho o alto total. Por ejemplo, en el ejemplo anterior, se marca la mitad derecha de la imagen como región de interés. Inclúyala si quiere limitar la solicitud de conclusión a la región de interés.

El objeto `filters` contiene un filtro de sitio (vea el campo `site`) que puede utilizar para restringir los resultados de imágenes y productos similares a un dominio específico. Por ejemplo, si la imagen es de un Surface Book, puede establecer `site` en `www.microsoft.com`.

Si quiere obtener conclusiones acerca de una copia local de una imagen, cargue la imagen como datos binarios.

Para obtener más información sobre cómo incluir estas opciones en el cuerpo de POST, vea [Tipos de formulario de contenido](#content-form-types).

### <a name="search-endpoint"></a>Punto de conexión de búsqueda

El punto de conexión de Visual Search es: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Las solicitudes solo se deben enviar como solicitudes HTTP POST.

### <a name="query-parameters"></a>Parámetros de consulta

Estos son los parámetros de consulta que debe especificar la solicitud. Como mínimo, debe incluir el parámetro de consulta `mkt`.

| Nombre | Valor | Tipo | Obligatorio |
| --- | --- | --- | --- |
| <a name="cc"></a>cc  | Código de país de dos caracteres que representa de dónde provienen los resultados.<br /><br /> Si establece este parámetro, debe especificar también el encabezado [Accept-Language](#acceptlanguage). Bing usa el primer idioma compatible que encuentra en la lista de idiomas y combina el idioma con el código de país que especifique para determinar el mercado del que debe devolver resultados. Si la lista de idiomas no incluye ningún idioma compatible, Bing busca el idioma y el mercado más cercanos que admiten la solicitud. También puede usar un mercado agregado o predeterminado para los resultados, en lugar del especificado.<br /><br /> Debe utilizar este parámetro de consulta y el parámetro de consulta `Accept-Language` solo si especifica varios idiomas. En caso contrario, debe utilizar los parámetros de consulta `mkt` y `setLang`.<br /><br /> Este parámetro y el parámetro de consulta [mkt](#mkt) son mutuamente excluyentes: no especifique los dos. | String | No       |
| <a name="mkt"></a>mkt   | Mercado de dónde proceden los resultados. <br /><br /> **NOTA:** Si se conoce, se recomienda especificar siempre el mercado. Especificar el mercado ayuda a Bing a enrutar la solicitud y devolver una respuesta adecuada y óptima.<br /><br /> Este parámetro y el parámetro de consulta [cc](#cc) son mutuamente excluyentes: no especifique los dos. | String | Sí      |
| <a name="safesearch"></a>safeSearch | Filtro de contenido para adultos. Estos son los posibles valores posibles de filtro (no distingue entre mayúsculas y minúsculas).<br /><ul><li>Desactivado: devuelve páginas web con texto o imágenes para adultos.<br /><br/></li><li>Moderado: devuelve páginas web con texto para adultos, pero no imágenes para adultos.<br /><br/></li><li>Estricto: no devuelve páginas web con texto o imágenes para adultos.</li></ul><br /> El valor predeterminado es Moderado.<br /><br /> **NOTA:** Si la solicitud proviene de un mercado para el que la directiva para adultos de Bing requiere que `safeSearch` se defina como Estricto, Bing ignora el valor `safeSearch` y usa Estricto.<br/><br/>**NOTA:** Si usa el operador de consulta `site:`, existe la posibilidad de que la respuesta contenga contenido para adultos, independientemente del valor asignado al parámetro de consulta `safeSearch`. Use `site:` solo si es consciente del contenido del sitio y el escenario admite la posibilidad de contenido para adultos.  | String | No       |
| <a name="setlang"></a>setLang  | Idioma que se usará para las cadenas de la interfaz de usuario. Especifique el idioma con el código ISO 639-1 de dos letras. Por ejemplo, el código de idioma de inglés es EN. El valor predeterminado es EN (inglés).<br /><br /> Aunque sea opcional, siempre debe especificar el idioma. Normalmente, se establece `setLang` como el mismo idioma especificado por `mkt`, salvo que el usuario quiera que las cadenas de la interfaz de usuario se muestren en otro idioma.<br /><br /> Este parámetro y el encabezado [Accept-Language](#acceptlanguage) son mutuamente excluyentes: no especifique los dos.<br /><br /> Una cadena de interfaz de usuario es aquella que se usa como etiqueta en una interfaz de usuario. Hay pocas cadenas de interfaz de usuario en los objetos de respuesta JSON. Además, los vínculos a las propiedades de Bing.com de los objetos de respuesta aplican el idioma especificado. | String | No   |

## <a name="headers"></a>encabezados

Estos son los encabezados que debe especificar la solicitud. Los encabezados `Content-Type` y `Ocp-Apim-Subscription-Key` son los únicos necesarios, pero también debe incluir `User-Agent`, `X-MSEdge-ClientID`, `X-MSEdge-ClientIP` y `X-Search-Location`.

| Encabezado | Descripción |
| --- | --- |
| <a name="acceptlanguage"></a>Accept-Language  | Encabezado de solicitud opcional.<br /><br /> Lista delimitada por comas de los idiomas que se usarán en las cadenas de la interfaz de usuario. La lista se muestra en orden de preferencia decreciente. Para más información, incluido el formato esperado, consulte [RFC2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este encabezado y el parámetro de consulta [setLang](#setlang) son mutuamente excluyentes, no especifique ambos.<br /><br /> Si establece este encabezado, debe especificar también el parámetro de consulta [cc](#cc). Para determinar el mercado para el que hay que devolver resultados, Bing usa el primer idioma compatible que encuentra en la lista y lo combina con el valor del parámetro `cc`. Si la lista no incluye ningún idioma compatible, Bing busca el idioma y el mercado más cercanos que admiten la solicitud, o bien usa un mercado agregado o predeterminado para los resultados. Para determinar el mercado que usó Bing, consulte el encabezado `BingAPIs-Market`.<br /><br /> Use este encabezado y el parámetro de consulta `cc` solo si especifica varios idiomas. En caso contrario, use los parámetros de consulta [mkt](#mkt) y [setLang](#setlang).<br /><br /> Una cadena de interfaz de usuario es aquella que se usa como etiqueta en una interfaz de usuario. Hay pocas cadenas de interfaz de usuario en los objetos de respuesta JSON. Todos los vínculos a las propiedades de Bing.com en los objetos de respuesta aplican el idioma especificado.  |
| <a name="contenttype"></a>Content-Type  |     |
| <a name="market"></a>BingAPIs-Market    | Encabezado de respuesta.<br /><br /> Mercado que usa la solicitud. El formato es \<languageCode\>-\<countryCode\>. Por ejemplo, en-US.  |
| <a name="traceid"></a>BingAPIs-TraceId  | Encabezado de respuesta.<br /><br /> Identificador de la entrada de registro que contiene los detalles de la solicitud. Si se produce un error, capture este identificador. Si no puede determinar y resolver el problema, incluya este identificador junto con el resto de información que proporcione al equipo de soporte técnico. |
| <a name="subscriptionkey"></a>Ocp-Apim-Subscription-Key | Encabezado de solicitud obligatorio.<br /><br /> Clave de suscripción que recibió al suscribirse a este servicio en [Cognitive Services](https://www.microsoft.com/cognitive-services/). |
| <a name="pragma"></a>Pragma |   |
| <a name="useragent"></a>User-Agent  | Encabezado de solicitud opcional.<br /><br /> El agente de usuario que origina la solicitud. Bing usa el agente de usuario para proporcionar a los usuarios de dispositivos móviles una experiencia optimizada. Aunque es opcional, se recomienda especificar siempre este encabezado.<br /><br /> El agente de usuario debe ser la misma cadena que envían los exploradores que más se usan. Para obtener información sobre los agentes de usuario, consulte [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> A continuación encontrará varios ejemplos de cadenas user-agent.<br /><ul><li>Windows Phone: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android: Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone: Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC: Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad: Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>      |
| <a name="clientid"></a>X-MSEdge-ClientID  | Encabezado de solicitud y respuesta opcional.<br /><br /> Bing usa este encabezado para proporcionar a los usuarios un comportamiento coherente en las llamadas API de Bing. Bing incorpora a menudo mejoras y características piloto nuevas, y usa el identificador de cliente como clave para asignar el tráfico en distintos paquetes piloto. Si no usa el mismo identificador de cliente para un usuario en varias solicitudes, Bing puede asignar el usuario a varios paquetes piloto en conflicto. La asignación a varios paquetes piloto en conflicto puede generar una experiencia de usuario incoherente. Por ejemplo, si la segunda solicitud tiene una asignación de paquete piloto distinta de la primera, puede que se produzca una experiencia inesperada. Además, Bing puede usar el identificador de cliente para personalizar los resultados web en el historial de búsqueda del identificador de cliente, lo que proporciona una experiencia enriquecida para el usuario.<br /><br /> Bing también utiliza este encabezado para ayudar a mejorar las clasificaciones de resultados mediante el análisis de la actividad que genera un identificador de cliente. Las mejoras de relevancia ayudan con una mayor calidad de los resultados que devuelven las API de Bing y, a su vez, permiten un mayor porcentaje de clics para el consumidor de la API.<br /><br /> **IMPORTANTE:** Aunque es opcional, considere este encabezado como obligatorio. Conservar el identificador de cliente en varias solicitudes para la misma combinación de usuario final y dispositivo permite 1) que el consumidor de API reciba una experiencia de usuario coherente y 2) un mayor porcentaje de clics a través de una mejor calidad de los resultados de las API de Bing.<br /><br /> Estas son las reglas de uso básico que se aplican a este encabezado.<br /><ul><li>Cada usuario que utiliza la aplicación en el dispositivo debe tener un identificador de cliente único generado por Bing.<br /><br/>Si no incluye este encabezado en la solicitud, Bing genera un identificador y lo devuelve en el encabezado de respuesta X-MSEdge-ClientID. La única ocasión en que NO debe incluir este encabezado en una solicitud es la primera vez que el usuario usa la aplicación en el dispositivo.<br /><br/></li><li>**ATENCIÓN:** Debe asegurarse de que este identificador de cliente no se pueda vincular a la información de cualquier cuenta de usuario autenticada.</li><li>Use el identificador de cliente para cada solicitud de API de Bing que haga la aplicación para este usuario en el dispositivo.<br /><br/></li><li>Conserve el identificador de cliente. Para conservar el identificador en una aplicación de explorador, use una cookie HTTP persistente para asegurarse de que se use ese identificador en todas las sesiones. No use una cookie de sesión. Para otras aplicaciones, como aplicaciones móviles, use el almacenamiento persistente del dispositivo para conservar el identificador.<br /><br/>La próxima vez que el usuario use la aplicación en ese dispositivo, obtenga el identificador de cliente que conservó.</li></ul><br /> **NOTA:** Las respuestas de Bing pueden incluir este encabezado o no incluirlo. Si la respuesta incluye este encabezado, capture el identificador de cliente y úselo para todas las solicitudes posteriores de Bing del usuario en dicho dispositivo.<br /><br /> **NOTA:** Si incluye X-MSEdge-ClientID, no debe incluir cookies en la solicitud. |
| <a name="clientip"></a>X-MSEdge-ClientIP   | Encabezado de solicitud opcional.<br /><br /> Dirección IPv4 o IPv6 del dispositivo cliente. La dirección IP se usa para detectar la ubicación del usuario. Bing usa la información de ubicación para determinar el comportamiento de la búsqueda segura.<br /><br /> **NOTA:** Aunque es opcional, se recomienda especificar siempre tanto este encabezado como el encabezado X-Search-Location.<br /><br /> No ofusque la dirección (por ejemplo, cambiando el último octeto a 0). Al ofuscar los resultados de dirección, la ubicación no se encuentra cerca de la ubicación real, lo que puede provocar que Bing devuelva resultados erróneos. |
| <a name="location"></a>X-Search-Location   | Encabezado de solicitud opcional.<br /><br /> Una lista delimitada por punto y coma de pares clave-valor que describen la ubicación geográfica del cliente. Bing usa la información de ubicación para determinar el comportamiento de la búsqueda segura y devolver contenido local apropiado. Especifique el par clave-valor como \<key\>:\<value\>. A continuación se incluyen las claves que usan para especificar la ubicación del usuario.<br /><br /><ul><li>lat: obligatoria. La latitud de la ubicación del cliente, en grados. La latitud debe ser mayor o igual que -90.0, y menor o igual que +90.0. Los valores negativos indican latitudes del sur, mientras que los valores positivos indican latitudes del norte.<br /><br /></li><li>long: obligatoria. La longitud de la ubicación del cliente, en grados. La longitud debe ser mayor o igual que -180.0, y menor o igual que +180.0. Los valores negativos indican longitudes del oeste, mientras que los valores positivos indican longitudes del este.<br /><br /></li><li>re: obligatorio. El radio, en metros, que especifica la precisión horizontal de las coordenadas. Pase el valor que devuelve el servicio de ubicación del dispositivo. Los valores típicos pueden ser 22 m para GPS/Wi-Fi, 380 m para la triangulación de antenas repetidoras de telefonía móvil y 18 000 m para la búsqueda inversa de IP.<br /><br /></li><li>ts: opcional. La marca de tiempo UTC UNIX del momento en que el cliente estaba en la ubicación. (la marca de tiempo UNIX es el número de segundos transcurridos desde el 1 de enero de 1970).<br /><br /></li><li>head: opcional. El encabezado relativo del cliente o la dirección de desplazamiento. Especifique la dirección de desplazamiento como grados de 0 a 360, en la dirección de las agujas del reloj con respecto al norte verdadero. Especifique esta clave solo si la clave `sp` es distinta de cero.<br /><br /></li><li>sp: opcional. La velocidad horizontal, en metros por segundo, a la que se desplaza el dispositivo de cliente.<br /><br /></li><li>alt: opcional. La altitud del dispositivo de cliente, en metros.<br /><br /></li><li>are: opcional. El radio, en metros, que especifica la precisión vertical de las coordenadas. Especifique esta clave solo si especificó la clave `alt`.<br /><br /></li></ul> **NOTA:** Aunque muchas de las claves son opcionales, cuanta más información proporcione, más precisos serán los resultados de ubicación.<br /><br /> **NOTA:** Aunque es opcional, se recomienda especificar siempre la ubicación geográfica del usuario. Proporcionar la ubicación es especialmente importante si la dirección IP del cliente no refleja con precisión la ubicación física del usuario (por ejemplo, si el cliente utiliza VPN). Para obtener resultados óptimos, debería incluir tanto este encabezado como `X-MSEdge-ClientIP` pero, como mínimo, debe incluir este.       |

> [!NOTE]
> Recuerde que los [Requisitos de uso y visualización de Bing Search API](../../bing-web-search/use-display-requirements.md) exigen el cumplimiento de todas las leyes aplicables, incluido el uso de estos encabezados. Por ejemplo, en determinados lugares, como Europa, existen requisitos para obtener el consentimiento del usuario antes de colocar determinados dispositivos de seguimiento en los dispositivos de usuario.

<a name="content-form-types"></a>

### <a name="content-form-types"></a>Tipos de formulario de contenido

La solicitud debe incluir el encabezado `Content-Type`. El encabezado debe establecerse en: `multipart/form-data; boundary=\<boundary string\>`, donde \<boundary string\> es una cadena opaca única que identifica el límite de los datos del formulario. Por ejemplo, `boundary=boundary_1234-abcd`.

Si envía a Visual Search una dirección URL o un token de imagen, el fragmento de código siguiente muestra los datos de formulario que hay que incluir en el cuerpo de POST. Los datos de formulario deben incluir el encabezado `Content-Disposition` y su parámetro `name` se debe establecer en to "knowledgeRequest". Para obtener información detallada del objeto `imageInfo`, consulte la sección dedicada a la solicitud.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

Opcionalmente, puede establecer el atributo `enableEntityData` del encabezado en `true` para obtener información detallada sobre la entidad principal de la imagen que se carga, incluidos vínculos a la información web y de atribución. Este campo es `false` de forma predeterminada.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
  "imageInfo" : {
      "url" : "https://contoso.com/2018/05/fashion/red.jpg"
  },
  "knowledgeRequest" : {
    "invokedSkillsRequestData" : {
        "enableEntityData" : "true"
    }
  }
}

--boundary_1234-abcd--
```

Si carga una imagen local, el fragmento de código siguiente muestra los datos de formulario que hay que incluir en el cuerpo de POST. Los datos de formulario deben incluir el encabezado `Content-Disposition`. Asimismo, el parámetro `name` se debe establecer en "imagen" y el parámetro `filename` se puede establecer en cualquier cadena. El encabezado `Content-Type` se puede establecer en cualquier tipo MIME de imagen común. El contenido del formulario corresponde a los datos binario de la imagen. Recuerde que el tamaño de imagen máximo que puede cargar es de 1 MB. El valor mayor del ancho o alto debe ser como máximo 1500 píxeles.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

El fragmento de código siguiente muestra cómo especificar la región de interés de una imagen cargada.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

### <a name="example-request"></a>Solicitud de ejemplo

El fragmento de código siguiente muestra una solicitud de conclusión de imagen completa que pasa un token de imagen y una región de interés. El token de conclusión se obtiene de una llamada anterior a /images/search:

```  
POST https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```

## <a name="bing-visual-search-responses"></a>Respuestas de Bing Visual Search


[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

Si hay conclusiones disponibles para la imagen, la respuesta contiene uno o varios valores `tags` que contienen las conclusiones. El campo `image` contiene el token de conclusión de la imagen de entrada:

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

El campo `tags` contiene un nombre para mostrar y una lista de acciones (conclusiones). Una de las etiquetas contiene un campo `displayName` que se establece en una cadena vacía. Esta etiqueta contiene las conclusiones predeterminadas, como las páginas web que incluyen la imagen, imágenes visualmente similares y orígenes de compra para los productos de la imagen. Dado que toda la imagen es de interés, la etiqueta de conclusión predeterminada no incluye rectángulos de selección para las áreas de interés:

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

Para obtener una lista de conclusiones predeterminadas, consulte [Conclusiones predeterminadas](../default-insights-tag.md).

Las etiquetas restantes contienen otras conclusiones que pueden ser de interés para el usuario. Por ejemplo, si la imagen contiene texto, una de las etiquetas puede incluir una conclusión TextResults, que contiene el texto reconocido. O bien, si Bing reconoce una entidad (es decir, una persona, un lugar o una cosa conocidos) en la imagen, una de las etiquetas puede identificar la entidad. Visual Search también devuelve un conjunto de términos variados (etiquetas) derivados de la imagen de entrada. Estas etiquetas permiten a los usuarios explorar los conceptos que se encuentran en la imagen. Por ejemplo, si la imagen de entrada es de un atleta famoso, una de las etiquetas podría ser deportes, que contiene vínculos a imágenes de deportes.

Cada etiqueta incluye un nombre para mostrar que puede usar para clasificar las conclusiones, el rectángulo de selección que identifica la región de interés a que se aplica la conclusión, las propias conclusiones y una miniatura de la imagen. Por ejemplo, si la imagen es de una persona con un jersey de deportes, una de las etiquetas podría incluir un rectángulo de selección que delimite el jersey e incluya conclusiones de VisualSearch y ProductVisualSearch. Por otra parte, otra etiqueta podría incluir una conclusión de ImageResults que contenga una URL para una solicitud de API /images/search para obtener imágenes cuyos temas estén relacionados o una dirección URL de búsqueda de Bing.com que lleve al usuario a los resultados de búsqueda de imágenes de Bing.com.

El resto de etiquetas, distintas a la etiqueta de conclusión predeterminada, incluyen rectángulos de selección que identifican las regiones de interés de la imagen. Por ejemplo, si la imagen incluye varias personas conocidas, las etiquetas podrían incluir rectángulos de selección para cada una de ellas. O bien, si la imagen contiene prendas de vestir conocidas, las etiquetas podrían incluir rectángulos de selección para cada prenda reconocida. Puede usar los rectángulos de selección para crear zonas activas en la imagen que, cuando se haga clic sobre ellas, proporcionen detalles sobre el contenido de dicha región de la imagen. No debe incluir zonas activas en una imagen para rectángulos de selección que identifiquen toda la imagen.

### <a name="text-recognition"></a>Reconocimiento de texto

Si la imagen contiene texto que reconoce el servicio, una de las etiquetas contendrá una conclusión (acción) de TextResults. El elemento `displayName` de la conclusión contiene el texto reconocido:

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Dado que el campo `displayName` de la etiqueta contiene ##TextRecognition, no lo use como título de categoría en la experiencia de usuario. Esto se aplica a cualquier nombre para mostrar que empiece por ##. En su lugar, use el nombre para mostrar de la acción.

El reconocimiento de texto también puede reconocer la información de contacto en tarjetas de presentación, como números de teléfono y direcciones de correo electrónico. El rectángulo delimitador identifica la ubicación de la información de contacto en la tarjeta.

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```

Si la imagen contiene una entidad reconocida, como una persona, lugar o cosa conocidos, una de las etiquetas puede incluir una conclusión de entidad. Los campos `mainEntity` y `data` solo están disponibles si el atributo `enableEntityData` del encabezado `Content-Type` está establecido en `true`.

```json
{
  "image" : {
    "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
  },
  "displayName" : "Statue of Liberty",
  "boundingBox" : {
    "queryRectangle" : {
      "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
      "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
      "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
      "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
    },
    "displayRectangle" : {
      "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
      "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
      "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
      "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
    }
  },
  "actions" : [
    {
      "_type" : "ImageEntityAction",
      "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
      "displayName" : "Statue of Liberty",
      "actionType" : "Entity",
      "mainEntity" : {
        "name" = "Statue of liberty",
        "bingId" : "..."
      },
      "data" : {
        "id" : "https://api.cognitive.microsoft.com/api/v7/entities/...",
        "readLink": "https://www.bingapis.com/api/v7/search?q=...",
        "readLinkPingSuffix": "...",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
                "name": "CC-BY-SA",
                "url": "http://creativecommons.org/licenses/by-sa/3.0/",
                "urlPingSuffix": "..."
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/...",
            "urlPingSuffix": "..."
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q=...",
        "webSearchUrlPingSuffix": "...",
        "name": "Statue of Liberty",
        "image": {
          "thumbnailUrl": "https://tse1.mm.bing.net/th?id=...",
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/...",
          "hostPageUrlPingSuffix": "...",
          "width": 50,
          "height": 50,
          "sourceWidth": 474,
          "sourceHeight": 598
        },
        "description" : "...",
        "bingId": "..."
        }
      }
  ]
}
```

## <a name="see-also"></a>Consulte también

- [¿Qué es Bing Visual Search API?](../overview.md)
- [Tutorial: Creación de una aplicación web de página única de Visual Search](../tutorial-bing-visual-search-single-page-app.md)
