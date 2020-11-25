---
title: 'Inicio rápido: Creación de un índice de búsqueda mediante las API REST'
titleSuffix: Azure Cognitive Search
description: En este inicio rápido de API REST, aprenderá a llamar a las API REST de Azure Cognitive Search mediante Postman o Visual Studio Code.
zone_pivot_groups: URL-test-interface-rest-apis
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.devlang: rest-api
ms.date: 11/17/2020
ms.openlocfilehash: 971ee806d8b5f9a336b40d96d500ce47d2f5166e
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94711398"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-using-rest-apis"></a>Inicio rápido: Creación de un índice de Azure Cognitive Search mediante las API REST

En este artículo se explica cómo formular solicitudes de API REST de forma interactiva mediante las [API REST de Azure Cognitive Search](/rest/api/searchservice) y un cliente API para enviar y recibir solicitudes. Con un cliente API y estas instrucciones, puede enviar solicitudes y ver las respuestas sin necesidad de escribir código.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

::: zone pivot="url-test-tool-rest-postman"

[!INCLUDE [Send requests using Postman](includes/search-get-started-rest-postman.md)]

::: zone-end

::: zone pivot="url-test-tool-rest-vscode-ext"

[!INCLUDE [Send requests using Visual Studio Code](includes/search-get-started-rest-vscode-ext.md)]

::: zone-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando trabaje con su propia suscripción, es una buena idea al final de un proyecto identificar si todavía se necesitan los recursos que ha creado. Los recursos que se dejan en ejecución pueden costarle mucho dinero. Puede eliminar los recursos de forma individual o eliminar el grupo de recursos para eliminar todo el conjunto de recursos.

Puede encontrar y administrar recursos en el portal, mediante el vínculo **Todos los recursos** o **Grupos de recursos** en el panel de navegación izquierdo.

Si está usando un servicio gratuito, recuerde que está limitado a tres índices, indexadores y orígenes de datos. Puede eliminar elementos individuales en el portal para mantenerse por debajo del límite. 

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo realizar las tareas básicas, puede continuar con las llamadas API REST para características más avanzadas, como indexadores o la [configuración de una canalización de enriquecimiento](cognitive-search-tutorial-blob.md) que permite agregar transformaciones de contenido a la indexación. Para realizar el paso siguiente, le recomendamos que vea este vínculo:

> [!div class="nextstepaction"]
> [Tutorial: Uso de REST y AI para generar contenido en el que se pueden realizar búsquedas desde blobs de Azure](cognitive-search-tutorial-blob.md)