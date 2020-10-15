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
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 1c3326456fefbbef9228cf26b5821f306cd21601
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80127905"
---
# <a name="search-for-exact-moments-in-videos-with-video-indexer"></a>Búsqueda de momentos exactos en vídeos con Video Indexer

En este tema se muestran las opciones de búsqueda que permiten encontrar momentos exactos dentro de vídeos.

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
2. Busque en todos los vídeos de la cuenta.

    En el siguiente ejemplo buscamos todos los vídeos que tratan sobre la seguridad y en los que aparece Satya.

    ![Buscar un vídeo en Video Indexer](./media/video-indexer-search/video-indexer-search01.png)

3. Busque la información resumida del vídeo.

    Después, para buscar en un vídeo, puede seleccionar **Reproducir** en el vídeo. Tras ello, para buscar momentos exactos en el vídeo, puede seleccionar la pestaña **Buscar**.

    En el siguiente ejemplo buscamos "secure" en el vídeo seleccionado.

    ![Buscar en un vídeo con Video Indexer](./media/video-indexer-search/video-indexer-search02.png)

    Si se selecciona uno de los resultados, el reproductor le llevará a ese momento exacto en el vídeo. Puede lograr la vista del reproductor o información detallada y la sincronización en la aplicación. Para más información, vea [Inserción de widgets de Video Indexer en las aplicaciones](video-indexer-embed-widgets.md).

4. Busque el desglose detallado del vídeo.

    Si quiere crear su propio clip basado en el vídeo que ha encontrado, seleccione el botón **Editar**. En esta página se muestra el vídeo junto con su información como filtros. Para más información, consulte [Visualización y edición de la información detallada de Video Indexer](video-indexer-view-edit.md).

    Puede buscar momentos exactos dentro del vídeo para mostrar únicamente las líneas que le interesen. Use la información lateral para filtrar los elementos que quiera ver. Cuando haya terminado, puede obtener la vista previa del clip y seleccionar **Publicar** para crear el clip que aparecerá en la galería.

    En el siguiente ejemplo se busca el texto "mixed reality". También se aplican filtros adicionales, como se muestra en la siguiente pantalla.

    ![Buscar un momento exacto en Video Indexer](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Pasos siguientes

Cuando encuentre en el vídeo el momento exacto con el que quiera trabajar, puede continuar procesando el vídeo. Para obtener más información, vea los temas siguientes:

- [Uso de información detallada propia en los vídeos](use-editor-create-project.md)
- [Proceso de contenido con Video Indexer REST API](video-indexer-use-apis.md)
- [Inserción de widgets visuales en la aplicación](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte también

[Introducción a Video Indexer](video-indexer-overview.md)
