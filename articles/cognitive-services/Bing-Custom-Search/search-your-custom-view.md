---
title: 'Búsqueda de una vista personalizada: Bing Custom Search'
titleSuffix: Azure Cognitive Services
description: Después de configurar su experiencia de búsqueda personalizada, puede probarla desde el portal de Bing Custom Search.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: aahi
ms.openlocfilehash: f00ffee47e3eb6366d632d8b6ee9beb01f048442
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76983119"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Llamada a la instancia de Bing Custom Search desde el portal

Después de configurar su experiencia de búsqueda personalizada, puede probarla desde el [portal](https://customsearch.ai) de Bing Custom Search. 

![captura de pantalla del portal de Bing Custom Search](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Creación de una consulta de búsqueda 

Una vez que haya iniciado sesión en el [portal](https://customsearch.ai) de Bing Custom Search, seleccione la instancia de búsqueda y haga clic en la pestaña **Producción**. En **Endpoints** (Puntos de conexión), seleccione un punto de conexión de API (por ejemplo, Web API). La suscripción determina qué puntos de conexión se muestran.

Para crear una consulta de búsqueda, escriba los valores de parámetro para el punto de conexión. Tenga en cuenta que los parámetros que aparecen en el portal pueden cambiar en función del punto de conexión que elija. Para más información, consulte la [referencia de Custom Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters). Para cambiar la suscripción que usa la instancia de búsqueda, agregue la clave de suscripción adecuada y actualice los parámetros apropiados del mercado y el idioma.

A continuación, se muestran algunos parámetros importantes:


|Parámetro  |Descripción  |
|---------|---------|
|Consultar     | El término de búsqueda para buscar. Solo está disponible para los puntos de conexión Web, Image, Video y Autosuggest. |
|Custom Configuration ID (Id. de configuración personalizada) | El identificador de configuración de la instancia de Custom Search seleccionada. Este campo es de solo lectura. |
|Market     | El mercado desde el que se originan los resultados. Solo está disponible para los puntos de conexión Web, Image, Video y Hosted UI.        |
|Clave de suscripción | La clave de suscripción con la que probar. Puede seleccionar una clave en la lista desplegable o escribirla manualmente.          |

Al hacer clic en **Additional Parameters** (Parámetros adicionales) aparecen los siguientes parámetros:  

|Parámetro  |Descripción  |
|---------|---------|
|Safe Search     | Un filtro usado para filtrar las páginas web de contenido para adultos. Solo está disponible para los puntos de conexión Web, Image, Video y Hosted UI. Tenga en cuenta que Bing Custom Video Search solo admite dos valores: `moderate` y `strict`.        |
|Idioma de la interfaz de usuario    | Idioma que se usa para las cadenas de la interfaz de usuario. Por ejemplo, si habilita las imágenes y los vídeos en Hosted UI, las pestañas **Image** (Imagen) y **Video** (Vídeo) usan el lenguaje especificado.        |
|Count     | Número de resultados de búsqueda que se devolverán en la respuesta. Disponible solo para los puntos de conexión Web, Image y Video.         |
|Offset    | El número de resultados de búsqueda que se van a omitir antes de devolver los resultados. Disponible solo para los puntos de conexión Web, Image y Video.        |
    
Tras especificar todas las opciones necesarias, haga clic en **Call** (Llamar) para ver la respuesta de JSON en el panel derecho. Si selecciona el punto de conexión Hosted UI, puede probar la experiencia de búsqueda en el panel derecho.

## <a name="change-your-bing-custom-search-subscription"></a>Cambie la suscripción a Bing Custom Search

Puede cambiar la suscripción asociada con la instancia de Bing Custom Search sin necesidad de crear una nueva instancia. Para que las llamadas de API se envíen y se cobren a una nueva suscripción, cree un nuevo recurso de Bing Custom Search en Azure Portal. Use la nueva clave de suscripción en las solicitudes de API, junto con el identificador de configuración personalizada. de la instancia.

## <a name="next-steps"></a>Pasos siguientes

- [Call your custom view with C#](./call-endpoint-csharp.md) (Llamada a la vista personalizada con C#)
- [Call your custom view with Java](./call-endpoint-java.md) (Llamada a la vista personalizada con Java)
- [Call your custom view with NodeJs](./call-endpoint-nodejs.md) (Llamada a la vista personalizada con NodeJs)
- [Call your custom view with Python](./call-endpoint-python.md) (Llamada a la vista personalizada con Python)

- [Call your custom view withthe C# SDK](./sdk-csharp-quick-start.md) (Llamada a la vista personalizada con el SDK de C#)
