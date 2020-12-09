---
title: Búsqueda de momentos exactos en vídeos con Video Indexer
titleSuffix: Azure Media Services
description: Obtenga información sobre cómo buscar momentos exactos en vídeos con Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 11/23/2019
ms.author: juliako
ms.openlocfilehash: a1ca7c677e5f1eb55d74cca45e757676674f303c
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030527"
---
# <a name="search-for-exact-moments-in-videos-with-video-indexer"></a>Búsqueda de momentos exactos en vídeos con Video Indexer

En este tema se muestra cómo usar el sitio web de Video Indexer para buscar momentos exactos en los vídeos.

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
1. Especifique las palabras clave de búsqueda; se realizará la búsqueda entre todos los vídeos de la biblioteca de su cuenta. 

    Para filtrar la búsqueda, seleccione **Filtros**. En el ejemplo siguiente, se busca "Microsoft" que aparece solo como texto en pantalla (OCR).

    :::image type="content" source="./media/video-indexer-search/filter.png" alt-text="Filtro, solo texto":::
1. Presione **Buscar** para ver el resultado.

    :::image type="content" source="./media/video-indexer-search/results.png" alt-text="Resultado de la búsqueda en vídeos":::

    Si se selecciona uno de los resultados, el reproductor le llevará a ese momento exacto en el vídeo.
1. Para ver y buscar las conclusiones resumidas del vídeo, haga clic en **Reproducir** en el vídeo o seleccione uno de los resultados de la búsqueda original. 

    Puede ver, buscar y editar las **conclusiones**. Si selecciona una de las conclusiones, el reproductor le llevará a ese momento exacto en el vídeo.  

    :::image type="content" source="./media/video-indexer-search/insights.png" alt-text="Vista, búsqueda y edición de las conclusiones del vídeo":::

    Si inserta el vídeo a través de widgets de Video Indexer, puede conseguir la vista del reproductor o las conclusiones y la sincronización de la aplicación. Para más información, vea [Inserción de widgets de Video Indexer en las aplicaciones](video-indexer-embed-widgets.md).
1. Para ver, buscar y editar las transcripciones, haga clic en la pestaña **Escala de tiempo**. 

    :::image type="content" source="./media/video-indexer-search/timeline.png" alt-text="Vista, búsqueda y edición de transcripciones del vídeo":::

    Para editar el texto, seleccione **Editar** en la esquina superior derecha y cambie el texto según sea necesario. 

    También puede traducir y descargar las transcripciones seleccionando la opción adecuada en la esquina superior derecha. 

## <a name="embed-download-create-projects"></a>Inserción, descarga y creación de proyectos

Para insertar el vídeo, seleccione **</> Insertar** en el vídeo. Para más información, consulte [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md).

Puede descargar el vídeo de origen, las conclusiones del vídeo o las transcripciones haciendo clic en **Descargar** debajo del vídeo.

Puede crear un clip de líneas y momentos específicos del vídeo haciendo clic en **Abrir en el editor**. Después, edite el vídeo y guarde el proyecto. Para más información, consulte [Uso de las conclusiones detalladas de los vídeos](use-editor-create-project.md).

:::image type="content" source="./media/video-indexer-search/embed-download-create-projects.png" alt-text="Inserción, descarga y creación de proyectos del vídeo":::

## <a name="next-steps"></a>Pasos siguientes

[Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
