---
title: Búsqueda de ubicaciones con los servicios Search de Azure Maps | Microsoft Azure Maps
description: En este artículo, aprenderá a buscar una ubicación utilizando el servicio Search de Microsoft Azure Maps para la geocodificación y la geocodificación inversa.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: cf0e5267885df1ace51271c53bb2d68ee5002f00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335425"
---
# <a name="search-for-a-location-using-azure-maps-search-services"></a>Búsqueda de ubicaciones con los servicios Search de Azure Maps

El [servicio de búsqueda](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps es un conjunto de API RESTful diseñadas para que los desarrolladores busquen direcciones, lugares o listados de empresas por nombre y categoría, y otra información geográfica. Además de permitir la geocodificación tradicional, los servicios pueden asimismo realizar la geocodificación inversa de direcciones y cruces de calles en función de la longitud y la latitud. Los valores de latitud y longitud devueltos por la búsqueda se pueden usar como parámetros en otros servicios de Azure Maps, como los servicios de [ruta](https://docs.microsoft.com/rest/api/maps/route) y [tiempo](https://docs.microsoft.com/rest/api/maps/weather).

En este artículo, aprenderá a:

* Solicitar las coordenadas de latitud y longitud de una dirección (geocodificación de la ubicación de la dirección) mediante [Search Address API]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddress)
* Buscar una dirección o punto de interés (POI) mediante [Fuzzy Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* Buscar una dirección junto con sus propiedades y coordenadas.
* Realizar una [búsqueda de dirección inversa](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) para traducir la ubicación de coordenadas a la dirección postal
* Buscar una intersección de calles mediante [Search Address Reverse Cross Street API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet).

## <a name="prerequisites"></a>Prerrequisitos

Para completar los pasos descritos en este artículo, primero debe crear una cuenta de Azure Maps y obtener la clave de suscripción de cuenta de los mapas. Siga las instrucciones que se indican en [Creación de una cuenta](quick-demo-map-app.md#create-an-account-with-azure-maps) para crear una suscripción a una cuenta de Azure Maps y siga los pasos para [obtener la clave principal](quick-demo-map-app.md#get-the-primary-key-for-your-account) de la cuenta. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).

Este artículo utiliza la [aplicación Postman](https://www.getpostman.com/apps) para generar llamadas REST. Puede usar cualquier entorno de desarrollo de API que prefiera.

## <a name="request-latitude-and-longitude-for-an-address-geocoding"></a>Solicitud de la latitud y longitud de una dirección (geocodificación)

En este ejemplo, usamos [Get Search Address API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) de Azure Maps para convertir la dirección postal en coordenadas de latitud y longitud. Puede pasar una dirección completa o parcial a la API y recibir una respuesta que incluye propiedades de la dirección detalladas, como la calle, el código postal o el país/región, así como valores posicionales de latitud y longitud.

Si tiene un conjunto de direcciones para la geocodificación, puede usar [Post Search Address Batch API](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatch) para enviar un lote de consultas en una única llamada de API.

1. En Postman, haga clic en **Nueva solicitud** | **Solicitud GET** y asígnele el nombre **Búsqueda de direcciones**.

2. En la pestaña Builder, seleccione el método HTTP **GET**, escriba la dirección URL de solicitud para el punto de conexión de API y seleccione un protocolo de autorización, si procede.

![Búsqueda de direcciones](./media/how-to-search-for-address/address_search_url.png)

| Parámetro | Valor sugerido |
|---------------|------------------------------------------------| 
| HTTP method | GET |
| URL de la solicitud | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) | 
| Authorization | Sin autenticación |

3. Haga clic en **Params** (Parámetros) y especifique los siguientes pares clave-valor para utilizar como parámetros de consulta o ruta de acceso en la dirección URL de la solicitud: 

![Búsqueda de direcciones](./media/how-to-search-for-address/address_search_params.png) 

| Clave | Value | 
|------------------|-------------------------| 
| api-version | 1.0 | 
| subscription-key | \<la clave de Azure Maps\> | 
| Query | 400 Broad St, Seattle, WA 98109 | 

4. Haga clic en **Enviar** y revise el cuerpo de la respuesta. 

En este caso, especificó una consulta de dirección completa y recibe un solo resultado en el cuerpo de respuesta. 

5. En Params (Parámetros), edite la cadena de consulta de modo que tenga el siguiente valor: 

    ```plaintext 
        400 Broad, Seattle 
    ``` 

6. Agregue la siguiente clave o par clave-valor a la sección **Params** y haga clic en **Enviar**: 

| Clave | Value | 
|-----|------------| 
| typeahead | true | 

La marca **typeahead** indica a la API de búsqueda de direcciones que trate la consulta como una entrada parcial y devuelva una matriz de valores predictivos.

## <a name="using-fuzzy-search-api"></a>Uso de Fuzzy Search API

[Fuzzy Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) de Azure Maps es el servicio recomendado que se utiliza cuando no se sabe cuáles son las entradas de usuario para una consulta de búsqueda. La API combina la búsqueda de puntos de interés (POI) y la geocodificación en una "búsqueda de una sola línea" canónica. Por ejemplo, la API puede controlar las entradas de cualquier dirección o una combinación de tokens de POI. La API también se puede ponderar con una posición contextual (par de latitud y longitud), completamente restringida por una coordenada y un radio, o se puede ejecutar de forma más general sin ningún punto de anclaje de polarización geográfica.

La mayoría de las consultas de búsqueda tienen como valor predeterminado `maxFuzzyLevel=1` para aumentar el rendimiento y reducir los resultados inusuales. Este valor predeterminado puede invalidarse según requiera la solicitud mediante la transmisión del parámetro de consulta `maxFuzzyLevel=2` o `3`.

### <a name="search-for-an-address-using-fuzzy-search"></a>Buscar una dirección mediante la búsqueda aproximada

1. Abra la aplicación Postman, haga clic en Nuevo | Crear nuevo y seleccione **Solicitud GET**. Escriba un nombre de solicitud de **búsqueda aproximada**, seleccione una colección o carpeta donde guardarlo y haga clic en **Guardar**.

2. En la pestaña Builder, seleccione el método HTTP **GET** y escriba la dirección URL de solicitud para el punto de conexión de API.

    ![Búsqueda aproximada](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parámetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | HTTP method | GET |
    | URL de la solicitud | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Authorization | Sin autenticación |

    El atributo **json** de la ruta de acceso URL determina el formato de respuesta. En este artículo se usa JSON para facilitar el uso y la legibilidad. Puede encontrar los formatos de respuesta disponibles en la definición de **obtención de búsqueda aproximada** de la [referencia de la API funcional de Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Haga clic en **Params** (Parámetros) y especifique los siguientes pares clave-valor para utilizar como parámetros de consulta o ruta de acceso en la dirección URL de la solicitud:

    ![Búsqueda aproximada](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Clave | Value |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<la clave de Azure Maps\> |
    | Query | pizza |

4. Haga clic en **Enviar** y revise el cuerpo de la respuesta.

    La cadena de consulta ambigua para "pizza" devolvió 10 [resultados de punto de interés](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (POI) en las categorías "pizza" y "restaurante". Cada resultado devuelve una dirección, valores de latitud y longitud, ventanilla y puntos de entrada para la ubicación.
  
    Los resultados son variados para esta consulta, no se asocian a ninguna ubicación de referencia determinada. Puede usar el parámetro **countrySet** para especificar solo los países o regiones para los que la aplicación necesita cobertura. El comportamiento predeterminado es buscar en todo el mundo, lo que puede devolver resultados innecesarios.

5. Agregue la siguiente clave o par clave-valor a la sección **Params** y haga clic en **Enviar**:

    | Clave | Value |
    |------------------|-------------------------|
    | countrySet | US |
  
    Ahora los resultados están limitados por el código de país y la consulta devuelve pizzerías de los Estados Unidos.
  
    Para proporcionar resultados para una ubicación, puede consultar un punto de interés y usar los valores de latitud y longitud devueltos en la llamada al servicio de búsqueda aproximada. En este caso, usó el servicio de búsqueda para devolver la ubicación de Space Needle de Seattle y utilizó los valores de latitud y longitud para orientar la búsqueda.
  
6. En Params (Parámetros), escriba los siguientes pares clave-valor y haga clic en **Enviar**:

    ![Búsqueda aproximada](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Clave | Value |
    |-----|------------|
    | lat | 47,620525 |
    | lon | -122,349274 |


## <a name="search-for-a-street-address-using-reverse-address-search"></a>Buscar una dirección mediante la búsqueda de direcciones inversa

[Get Search Address Reverse API]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) de Azure Maps ayuda a transformar una coordenada (por ejemplo: 37,786505 -122,3862) en una dirección que resulte comprensible. Por lo general, esta conversión es necesaria en las aplicaciones de seguimiento, donde recibe una fuente GPS del dispositivo o recurso y desea saber en qué dirección se encuentra en la coordenada.
Si tiene un conjunto de direcciones en forma de coordenadas para realizar la geocodificación inversa, puede usar [Post Search Address Batch API](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatch) para enviar un lote de consultas en una única llamada de API.


1. En Postman, haga clic en **Nueva solicitud** | **Solicitud GET** y asígnele el nombre **Búsqueda de direcciones inversa**.

2. En la pestaña Builder, seleccione el método HTTP **GET** y escriba la dirección URL de solicitud para el punto de conexión de API.
  
    ![Dirección URL de Búsqueda de direcciones inversa](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parámetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | HTTP method | GET |
    | URL de la solicitud | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Authorization | Sin autenticación |
  
3. Haga clic en **Params** (Parámetros) y especifique los siguientes pares clave-valor para utilizar como parámetros de consulta o ruta de acceso en la dirección URL de la solicitud:
  
    ![Parámetros de Búsqueda de direcciones inversa](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Clave | Value |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<la clave de Azure Maps\> |
    | Query | 47,591180,-122,332700 |
  
4. Haga clic en **Enviar** y revise el cuerpo de la respuesta.

    La respuesta incluye información de dirección clave sobre Safeco Field.
  
5. Agregue la siguiente clave o par clave-valor a la sección **Params** y haga clic en **Enviar**:

    | Clave | Value |
    |-----|------------|
    | number | true |

    Si el parámetro de consulta [number](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) se envía con la solicitud, la respuesta puede incluir el lado de la calle (izquierda o derecha) y también una posición de desplazamiento para ese número.
  
6. Agregue la siguiente clave o par clave-valor a la sección **Params** y haga clic en **Enviar**:

    | Clave | Value |
    |-----|------------|
    | returnSpeedLimit | true |
  
    Cuando se establece el parámetro de consulta [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), la respuesta devuelve el límite de velocidad publicado.

7. Agregue la siguiente clave o par clave-valor a la sección **Params** y haga clic en **Enviar**:

    | Clave | Value |
    |-----|------------|
    | returnRoadUse | true |

    Cuando se establece el parámetro de consulta [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), la respuesta devuelve la matriz de uso de carretera para códigos geográficos inversos a nivel de calle.

8. Agregue la siguiente clave o par clave-valor a la sección **Params** y haga clic en **Enviar**:

    | Clave | Value |
    |-----|------------|
    | roadUse | true |

    Puede restringir la consulta de código geográfico inversa a un tipo específico de uso de carretera mediante el parámetro de consulta [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse).
  
## <a name="search-for-cross-street-using-reverse-address-cross-street-search"></a>Búsqueda de la calle transversal mediante la búsqueda de calles transversales de direcciones inversa

1. En Postman, haga clic en **Nueva solicitud** | **Solicitud GET** y asígnele el nombre **Búsqueda de calles transversales de direcciones inversa**.

2. En la pestaña Builder, seleccione el método HTTP **GET** y escriba la dirección URL de solicitud para el punto de conexión de API.
  
    ![Búsqueda de calles transversales de direcciones inversa](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parámetro | Valor sugerido |
    |---------------|------------------------------------------------|
    | HTTP method | GET |
    | URL de la solicitud | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Authorization | Sin autenticación |
  
3. Haga clic en **Params** (Parámetros) y especifique los siguientes pares clave-valor para utilizar como parámetros de consulta o ruta de acceso en la dirección URL de la solicitud:
  
    | Clave | Value |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<la clave de Azure Maps\> |
    | Query | 47,591180,-122,332700 |
  
4. Haga clic en **Enviar** y revise el cuerpo de la respuesta.

## <a name="next-steps"></a>Pasos siguientes

- Explore la [documentación de la API de REST del servicio Search](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps.
- Obtenga información sobre [Procedimientos recomendados del servicio Search de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-best-practices-for-search) y cómo optimizar las consultas.
