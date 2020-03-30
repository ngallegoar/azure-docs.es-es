---
title: 'Definición de sugerencias de Custom Autosuggest: Bing Custom Search'
titleSuffix: Azure Cognitive Services
description: Custom Autosuggest devuelve una lista de cadenas de consulta de búsqueda sugeridas que son pertinentes para su experiencia de búsqueda.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: maheshb
ms.openlocfilehash: 5b2b8871d868e827532f23c7ef4f14fb00afb5bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072803"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Configuración de experiencia de Custom Autosuggest

Custom Autosuggest devuelve una lista de cadenas de consulta de búsqueda sugeridas que son pertinentes para su experiencia de búsqueda. Las cadenas de consulta sugeridas se basan en una cadena de consulta parcial que proporciona el usuario en el cuadro de búsqueda. La lista contendrá un máximo de 10 sugerencias. 

Especifique si desea devolver solo sugerencias personalizadas o también incluir sugerencias de Bing. Si incluye sugerencias de Bing, las sugerencias personalizadas aparecen antes de las sugerencias de Bing. Si proporciona suficientes sugerencias pertinentes, es posible que la lista de sugerencias devuelta no incluya sugerencias de Bing. Las sugerencias de Bing se encuentran siempre en el contexto de la instancia de Custom Search. 

Para configurar las sugerencias de consulta de búsqueda para la instancia, haga clic en la pestaña **Sugerencias automáticas**.  

> [!NOTE]
> Para usar esta característica, debe suscribirse a Custom Search en el nivel adecuado (consulte [precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)).

Es posible que transcurran hasta 24 horas para que se muestren sugerencias en el punto de conexión de servicio (API o interfaz de usuario hospedada).

## <a name="enable-bing-suggestions"></a>Habilitación de las sugerencias de Bing

Para habilitar las sugerencias de Bing, ponga el control deslizante **Automatic Bing suggestions** (Sugerencias automáticas de Bing) en la posición de activado. El control deslizante se pone azul.

## <a name="add-your-own-suggestions"></a>Adición de sus propias sugerencias

Para agregar sus propias sugerencias de cadena de consulta, agréguelas a la lista de **User-defined suggestions** (Sugerencias definidas por el usuario). Después de agregar una sugerencia a la lista, presione la tecla ENTRAR o haga clic en el icono **+** . Puede especificar la sugerencia en cualquier lenguaje. Puede agregar un máximo de 5.000 sugerencias de cadena de consulta.

## <a name="upload-suggestions"></a>Carga de sugerencias

Opcionalmente, puede cargar una lista de sugerencias de un archivo. El archivo debe contener una cadena de consulta de búsqueda por línea. Para cargar el archivo, haga clic en el icono de carga y seleccione el archivo que desea cargar. El servicio extrae las sugerencias del archivo y las agrega a la lista.

## <a name="remove-suggestions"></a>Eliminación de sugerencias

Para quitar una sugerencia de cadena de consulta, haga clic en el icono de eliminación situado junto a la sugerencia que desea quitar.

## <a name="block-suggestions"></a>Bloqueo de sugerencias

Si incluye sugerencias de Bing, puede agregar una lista de cadenas de consulta de búsqueda que no desea que Bing devuelva. Para agregar cadenas de consulta bloqueadas, haga clic en **Show blocked suggestions** (Mostrar sugerencias bloqueadas). Agregue la cadena de consulta a la lista y presione la tecla ENTRAR o haga clic en el icono **+** . Puede agregar un máximo de 50 cadenas de consulta bloqueadas.



[!INCLUDE [publish or revert](./includes/publish-revert.md)]

>[!NOTE]  
>La configuración de Custom Autosuggest puede tardar hasta 24 horas en surtir efecto.


## <a name="enabling-autosuggest-in-hosted-ui"></a>Habilitación de Autosuggest en la interfaz de usuario hospedada

Para habilitar las sugerencias de cadena de consulta para la interfaz de usuario hospedada, haga clic en **Hosted UI** (IU hospedada). Desplácese hacia abajo hasta la sección **Additional Configuration** (Configuración adicional). En **Búsqueda web**, seleccione **Activado** para **Enable autosuggest** (Habilitar las sugerencias automáticas). Para habilitar Autosuggest, debe seleccionar un diseño que incluye un cuadro de búsqueda.


## <a name="calling-the-autosuggest-api"></a>Llamada a Autosuggest API

Para obtener cadenas de consulta sugeridas mediante Bing Custom Search API, envíe una solicitud `GET` al punto de conexión siguiente.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

La respuesta contiene una lista de objetos `SearchAction` que contienen las cadenas de consulta sugeridas.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Cada sugerencia incluye un campo `displayText` y `query`. El campo `displayText` contiene la cadena de consulta sugerida que usa para rellenar la lista desplegable del cuadro de búsqueda.

Si el usuario selecciona una cadena de consulta sugerida de la lista desplegable, use la cadena de consulta en el campo `query` al llamar a [Bing Custom Search API](overview.md).


## <a name="next-steps"></a>Pasos siguientes

- [Get custom suggestions](./get-custom-suggestions.md) (Obtención de sugerencias personalizadas)
- [Search your custom instance](./search-your-custom-view.md) (Búsqueda de su instancia personalizada)
- [Configure and consume custom hosted UI](./hosted-ui.md) (Configuración y uso de la IU hospedada personalizada)
