---
title: Configuración de la experiencia de Bing Custom Search | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: 'El portal le permite crear una instancia de búsqueda que especifica los sectores de la Web: dominios, subpáginas y páginas web.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: c14376cc80373371ec5fcb8f22a00584a6b2f714
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2020
ms.locfileid: "84710174"
---
# <a name="configure-your-bing-custom-search-experience"></a>Configuración de la experiencia de Bing Custom Search

Una instancia de Custom Search le permite adaptar la experiencia de búsqueda para incluir contenido solo de sitios web que interesan a los usuarios. En lugar de realizar una búsqueda en toda la web, Bing solo busca en el sector de la web que le interesa. Para crear la vista personalizada de la web, use el [portal](https://customsearch.ai) de Bing Custom Search.

El portal le permite crear una instancia de búsqueda que especifica los sectores de la web: dominios, subpáginas y páginas web en los que quiere que busque Bing y aquellos en los que no quiere que busque. El portal también puede sugerir contenido que puede que quiera incluir.

Al definir los sectores de la web, use los siguientes elementos:

| Nombre del sector | Descripción                                                                                                                                                                                                                                                                                                |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Domain     | Un sector de dominio incluye todo el contenido que se encuentra en un dominio de Internet. Por ejemplo, `www.microsoft.com`. Si se omite `www.`, Bing también busca en los subdominios del dominio. Por ejemplo, si especifica `microsoft.com`, Bing también devuelve los resultados de `support.microsoft.com` o `technet.microsoft.com`. |
| Subpágina    | Un sector de subpágina incluye todo el contenido encontrado en la subpágina y las rutas de acceso que hay debajo de ella. Puede especificar como máximo dos subpáginas en la ruta de acceso. Por ejemplo: `www.microsoft.com/en-us/windows/`                                                                                                                       |
| Página web    | Un sector de página web puede incluir solo esa página web en una búsqueda personalizada. Opcionalmente, puede especificar si se deben incluir subpáginas.                                                                                                                                                                                  |

> [!IMPORTANT]
> Todos los dominios, subpáginas y páginas web que especifique deben ser públicos y estar indexados por Bing. Si dispone de un sitio público que quiere incluir en la búsqueda, y Bing no lo ha indexado, consulte la [documentación del administrador web](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) para más información sobre cómo obtener Bing para indexarlo. Además, consulte la documentación del administrador web para más información sobre cómo obtener Bing para actualizar su sitio rastreado si el índice no está actualizado.

## <a name="add-slices-of-the-web-to-your-custom-search-instance"></a>Adición de sectores de la web a la instancia de Custom Search

Al crear la instancia de Custom Search, puede especificar los sectores de la web: dominios, subpáginas y las páginas web que quiere incluir o bloquear desde los resultados de búsqueda. 

Si conoce los sectores que quiere incluir en la instancia de Custom Search, agréguelos a la lista **Active** (Activos) de esta. 

Si no está seguro de qué sectores incluir, puede enviar consultas de búsqueda a Bing en el panel **Vista previa** y seleccionar los sectores que quiera. Para ello, siga estos pasos: 

1. Seleccione Bing en la lista desplegable del panel Vista previa y escriba la consulta de búsqueda.

2. Haga clic en **Add site** (Agregar sitio) junto al resultado que quiere incluir. A continuación, haga clic en Aceptar.

>[!NOTE]
> [!INCLUDE[publish or revert](./includes/publish-revert.md)]

<a name="active-and-blocked-lists"></a>

### <a name="customize-your-search-experience-with-active-and-blocked-lists"></a>Personalice su experiencia de búsqueda con listas de activos y bloqueados. 

Para obtener acceso a la lista de sectores activos y bloqueados, haga clic en las pestañas **Active** (Activos) y **Blocked** (Bloqueados) en la instancia de Custom Search. Los sectores que se agreguen a la lista de activos se incluirán en Custom Search. No se buscará en los sectores bloqueados y estos no aparecerán en los resultados de búsqueda.

Para especificar los sectores de la web que quiera que busque Bing, haga clic en la pestaña **Active** (Activos) y agregue una o varias direcciones URL. Para editar o eliminar direcciones URL, use las opciones de la columna **Controls** (Controles). 

Al agregar direcciones URL a la lista de **activos**, puede agregar direcciones URL únicas o varias direcciones URL a la vez mediante la carga de un archivo de texto con el icono de carga.

![Pestaña Active (Activos) de Bing Custom Search](media/file-upload-icon.png)

Para cargar un archivo, cree un archivo de texto y especifique un único dominio, subpágina o página web por línea. El archivo se rechazarán si no tiene el formato correcto.

> [!NOTE]
> * Solo puede cargar un archivo en la lista de **activos**. No se puede usar para agregar sectores a la lista de **bloqueados**.  
> * Si la lista de **bloqueados** contiene un dominio, una subpágina o una página web que especificó en el archivo de carga, se quitará de la lista de **bloqueados** y se agregará a la de **activos**.
> * Bing Custom Search omitirá las entradas duplicadas del archivo de carga. 

### <a name="get-website-suggestions-for-your-search-experience"></a>Obtención de sugerencias de sitio web para la experiencia de búsqueda

Después de agregar sectores de la web a la lista de **activos**, el portal de Bing Custom Search generará sugerencias de sitio web y subpágina en la parte inferior de la pestaña. Estos son los sectores que Bing Custom Search cree que quiere incluir. Haga clic en **Actualizar** para obtener sugerencias actualizadas después de actualizar la configuración de la instancia de Custom Search. Esta sección solo está visible si hay sugerencias disponibles.

## <a name="search-for-images-and-videos"></a>Búsqueda de imágenes y vídeos

Puede buscar imágenes y vídeos de forma similar al contenido web mediante [Bing Custom Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference) o [Bing Custom Video Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference). Puede mostrar estos resultados con la [UI hospedada](hosted-ui.md) o las API. 

Estas API son similares a las API no personalizadas de [Bing Image Search](../Bing-Image-Search/overview.md) y [Bing Video Search](../Bing-Video-Search/search-the-web.md), pero buscan en toda la web y no requieren el parámetro de consulta `customConfig`. Consulte esta documentación para obtener más información sobre cómo trabajar con imágenes y vídeos. 

## <a name="test-your-search-instance-with-the-preview-pane"></a>Prueba de la instancia de búsqueda con el panel Vista previa

Puede probar la instancia de búsqueda mediante el panel de vista previa situado al lado derecho del portal para enviar consultas de búsqueda y ver los resultados. 

1. Debajo del cuadro de búsqueda, seleccione **My Instance** (Mi instancia). Puede comparar los resultados de la experiencia de búsqueda con Bing seleccionando **Bing**. 
2. Seleccione un filtro de búsqueda seguro y el mercado en el que buscar (consulte [Query Parameters](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) [Parámetros de consulta]).
3. Escriba una consulta y presione Entrar o haga clic en el icono de búsqueda para ver los resultados de la configuración actual. Puede cambiar el tipo de búsqueda que realiza haciendo clic en **Web**, **Imagen** o **Vídeo** para obtener los resultados correspondientes. 

<a name="adjustrank"></a>

## <a name="adjust-the-rank-of-specific-search-results"></a>Ajuste de la clasificación de resultados de búsqueda específicos

El portal le permite ajustar la clasificación de la búsqueda de contenido de supáginas, páginas web y dominios específicos. Después de enviar una consulta de búsqueda en el panel de vista previa, cada resultado de búsqueda contiene una lista de los ajustes que puede realizar:  

|            |                                                                                                                                                                      |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Block      | Mueve el dominio, la subpágina o la página web a la lista de bloqueados. Bing excluirá la aparición de contenido del sitio seleccionado en los resultados de búsqueda.                    |
| Aumentar de nivel      | Coloca el contenido del dominio o la subpágina en un nivel más alto en los resultados de búsqueda.                                                                                        |
| Reducir de nivel     | Coloca el contenido del dominio o la subpágina en un nivel inferior en los resultados de búsqueda. Seleccione si desea reducir de nivel el contenido del dominio o la subpágina a los que pertenece la página web. |
| Anclar al principio | Mueve el dominio, la subpágina o la página web a la lista **Pinned** (Anclados). Esto obliga a la página web a aparecer como el resultado de búsqueda principal para una consulta de búsqueda determinada.                   |

El ajuste de la clasificación no está disponible para las búsquedas de imágenes o vídeos.

### <a name="boosting-and-demoting-search-results"></a>Aumento y disminución de los resultados de búsqueda

Puede aplicar aumento, aumento avanzado o disminución de nivel a cualquier dominio o subpágina de la lista de **activos**. De manera predeterminada, todos los sectores se agregan sin ajustes de clasificación. Los sectores de la web con aumento o aumento avanzado se clasifican mejor en los resultados de búsqueda (teniendo el aumento avanzado una clasificación más alta que el aumento solo). Los elementos que disminuyen de nivel se clasifican por detrás en los resultados de búsqueda.

Para aplicar aumento, aumento avanzado o disminución de nivel a los elementos, puede usar los controles de **ajuste de clasificación** de la lista de **activos** o los controles de aumento y disminución de nivel del panel de vista previa. El servicio agrega el sector a la lista de activos y ajusta la clasificación según corresponda.

> [!NOTE] 
> El aumento y disminución el nivel de dominios y subpáginas es uno de los diversos métodos que Bing Custom Search usa para determinar el orden de los resultados de búsqueda. Debido a otros factores que influyen en la clasificación de diferente contenido web, los efectos del ajuste de clasificación pueden variar. Use el panel de vista previa para probar los efectos del ajustar de clasificación de los resultados de búsqueda. 

Los ajustes de clasificación de aumento avanzado, aumento y disminución no están disponibles para la búsqueda de imágenes y vídeos.

## <a name="pin-slices-to-the-top-of-search-results"></a>Anclaje de los sectores al principio de los resultados de búsqueda

El portal también le permite anclar las direcciones URL al principio de los resultados de búsqueda para términos de búsqueda específicos mediante la pestaña **Pinned** (Anclado). Escriba una dirección URL y la consulta para especificar la página web que se mostrará como resultado principal. Tenga en cuenta que puede anclar un máximo de una página web por consulta de búsqueda y que en las búsquedas solo se mostrarán las páginas web indexadas. El anclaje de los resultados no está disponible para las búsquedas de imágenes o vídeos.

Puede anclar una página web al principio de dos maneras:

* En la pestaña **Pinned** (Anclado), escriba la dirección URL de la página web que se va a anclar al principio y su consulta correspondiente.

* En el panel **Vista previa**, escriba una consulta de búsqueda y haga clic en el botón de búsqueda. Busque la página web que quiere anclar para la consulta y haga clic en **Anclar al principio**. La página web y la consulta se agregarán a la lista **Anclado**.

### <a name="specify-the-pins-match-condition"></a>Especificación de la condición de coincidencia de anclaje

De manera predeterminada, las páginas web solo están ancladas al principio de los resultados de búsqueda cuando la cadena de consulta de un usuario coincide exactamente con una que aparece en la lista **Anclado**. Puede cambiar este comportamiento al especificar una de las siguientes condiciones de coincidencia:

> [!NOTE]
> Todas las comparaciones entre la consulta de búsqueda del usuario y la consulta de búsqueda del anclaje distinguen mayúsculas de minúsculas.

| Value | Descripción                                                                          |
|---------------|----------------------------------------------------------------------------------|
| Empieza por | El anclaje coincide si la cadena de consulta del usuario empieza por lo mismo que la de anclaje. |
| Termina por   | El anclaje coincide si la cadena de consulta del usuario termina por lo mismo que la de anclaje.  |
| Contains    | El anclaje coincide si la cadena de consulta del usuario contiene la de anclaje.   |


Para cambiar la condición de coincidencia de anclaje, haga clic en el icono de edición del anclaje. En la columna **Query match condition** (Condición de coincidencia de la consulta), haga clic en la lista desplegable y seleccione la nueva condición para usarla. Después, haga clic en el icono para guardar el cambio.

### <a name="change-the-order-of-your-pinned-sites"></a>Cambio del orden de los sitios anclados

Para cambiar el orden de los anclajes, puede arrastrarlos y colocarlos o editar su número de orden haciendo clic en el icono de edición en la columna **Controls** (Controles) de la lista **Anclado**.

Si varios anclajes satisfacen una condición de coincidencia, Bing Custom Search usará la más alta de la lista.

## <a name="view-statistics"></a>Visualización de estadísticas

Si se ha suscrito a Custom Search en el nivel adecuado (consulte los [planes de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), se agrega una pestaña **Statistics** (Estadísticas) a las instancias de producción. En la pestaña de estadísticas se muestra información detallada sobre cómo se usan los puntos de conexión de Custom Search, como el volumen de llamadas, las mejores consultas, la distribución geográfica, los códigos de respuesta y la búsqueda segura. Puede filtrar esta información mediante los controles proporcionados.

## <a name="usage-guidelines"></a>Directrices de uso

- En cada instancia de búsqueda personalizada, el número máximo de ajustes de clasificación que puede realizar en los sectores **Active** (Activo) y **Blocked** (Bloqueado) se limita a 400.
- Agregar un sector a las pestañas Active (Activo) o Blocked (Bloqueado) cuenta como un ajuste de clasificación.
- Aumentar y disminuir de nivel cuenta como dos ajustes de clasificación.
- En cada instancia de búsqueda personalizada, el número máximo de elementos anclados que se pueden crear está limitado a 200.

## <a name="next-steps"></a>Pasos siguientes

- [Call your custom search](./search-your-custom-view.md) (Llamada a la búsqueda personalizada)
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](../bing-web-search/hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)
