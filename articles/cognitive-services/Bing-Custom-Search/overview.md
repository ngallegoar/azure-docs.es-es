---
title: ¿Qué es Bing Custom Search API?
titleSuffix: Azure Cognitive Services
description: Bing Custom Search API le permite crear experiencias de búsqueda a medida de los temas que le interesan.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: e70d4d83fcd9641fa86f013688b5a3a6e3652919
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96338408"
---
# <a name="what-is-the-bing-custom-search-api"></a>¿Qué es Bing Custom Search API?

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020**, las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Custom Search API le permite crear experiencias de búsqueda a medida sin anuncios de los temas que le interesan. Puede especificar los dominios y las páginas web donde buscará Bing, así como anclar y aumentar o disminuir el nivel de contenido específico para crear una vista personalizada de la Web y ayudar a los usuarios a encontrar rápidamente los resultados de búsqueda apropiados. 

## <a name="features"></a>Características

|Característica  |Descripción  |
|---------|---------|
|[Sugerencias de búsqueda en tiempo real personalizadas](define-custom-suggestions.md)     | Proporciona sugerencias de búsqueda que pueden mostrarse como una lista desplegable a medida que los usuarios escriben.       | 
|[Experiencias personalizadas de búsqueda de imágenes](get-images-from-instance.md)     | Permita que los usuarios busquen imágenes en los dominios y los sitios web especificados en su instancia de búsqueda personalizada.        |        
|[Experiencias personalizadas de búsqueda de vídeos](get-videos-from-instance.md)     | Permita que los usuarios busquen vídeos en los dominios y los sitios especificados en su instancia de búsqueda personalizada.        |    
|[Uso compartido de la instancia de búsqueda personalizada](share-your-custom-search.md)     | Comparta su instancia de búsqueda con los miembros del equipo y colabore para editarla y probarla.        | 
|[Configuración de una interfaz de usuario para sus aplicaciones y sitios web](hosted-ui.md)     | Proporciona una UI hospedada que puede integrar fácilmente en las aplicaciones web y páginas web como un fragmento de código de JavaScript.        | 
## <a name="workflow"></a>Flujo de trabajo

Puede crear una instancia de búsqueda personalizada con el [portal de Bing Custom Search](https://customsearch.ai). El portal le permite crear una instancia de búsqueda personalizada que especifique los dominios, los sitios web y las páginas web en los que quiere que busque Bing, y aquellos en los que no quiere que busque. También puede usar el portal para mostrar una vista previa de la experiencia de búsqueda, ajustar las clasificaciones de búsqueda que la API proporciona y, opcionalmente, configurar una interfaz de usuario que permite búsquedas para mostrarla en sus aplicaciones y sitios web.

Una vez creada, puede integrar la instancia de búsqueda (y, opcionalmente, una interfaz de usuario) en su aplicación o sitio web mediante una llamada a Bing Custom Search API:

![Imagen que muestra que puede conectar con Bing Custom Search mediante la API](media/BCS-Overview.png "Funcionamiento de Bing Custom Search")


## <a name="next-steps"></a>Pasos siguientes

Para iniciar rápidamente, consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).

Para obtener más información sobre cómo personalizar su instancia de búsqueda, consulte [Define a custom search instance](define-your-custom-view.md) (Definir una instancia de búsqueda personalizada).

Consulte los [requisitos de visualización y uso de Bing](../bing-web-search/use-display-requirements.md) para usar los resultados de búsqueda en sus aplicaciones y servicios.

Visite la [página central de Bing Search API](../bing-web-search/overview.md) para explorar las otras API disponibles.

Familiarícese con el contenido de referencia de cada uno de los puntos de conexión de búsqueda personalizada. La referencia contiene los puntos de conexión, los encabezados y los parámetros de consulta que se utilizan para solicitar los resultados de la búsqueda. También incluye definiciones de los objetos de respuesta.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

- [Custom Search API](/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
- [Custom Image API](/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference)
- [Custom Video API](/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference)
- [Custom Autosuggest API](/rest/api/cognitiveservices-bingsearch/bing-custom-autosuggest-api-v7-reference)