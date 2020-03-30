---
title: Visualización de un almacén de conocimiento (versión preliminar) con el Explorador de Storage
titleSuffix: Azure Cognitive Search
description: Visualice y analice un almacén de conocimiento de Azure Cognitive Search con el Explorador de Storage de Azure Portal. El almacén de conocimiento está actualmente en versión preliminar pública.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/30/2019
ms.openlocfilehash: 167316eca1f85530a040d4543f98ae34a9fb93c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75754074"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Visualización de un almacén de conocimiento con el Explorador de Storage

> [!IMPORTANT] 
> El almacén de conocimiento está actualmente en versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST versión 2019-05-06-Preview](search-api-preview.md) se proporcionan características en versión preliminar. Actualmente hay compatibilidad limitada con el portal y no la hay con el SDK de .NET.

En este artículo, aprenderá por ejemplo a conectar y explorar un almacén de conocimiento con el Explorador de Storage en Azure Portal.

## <a name="prerequisites"></a>Prerequisites

+ Siga los pasos descritos en [Creación de un almacén de conocimiento en Azure Portal](knowledge-store-create-portal.md) para crear el almacén de conocimiento de ejemplo que se usa en este tutorial.

+ También necesitará el nombre de la cuenta de Azure Storage que usó para crear el almacén de conocimiento, junto con su clave de acceso de Azure Portal.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Visualización, edición y consulta de un almacén de conocimiento en el Explorador de Storage

1. En Azure Portal, [abra la cuenta de almacenamiento](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) que usó para crear el almacén de conocimiento.

1. En el panel de navegación izquierdo de la cuenta de almacenamiento, haga clic en **Explorador de Storage**.

1. Expanda la lista **Tablas** para mostrar una lista de las proyecciones de tablas de Azure que se crearon al ejecutar el asistente para **Importar datos** en los datos de ejemplo de reseñas de hoteles.

Seleccione una tabla para ver los datos enriquecidos, incluidas las frases clave y las puntuaciones de opinión.

   ![Visualización de tablas en el Explorador de Storage](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Visualización de tablas en el Explorador de Storage")

Para cambiar el tipo de datos de cualquier valor de tabla o para cambiar los valores individuales de la tabla, haga clic en **Editar**. Al cambiar el tipo de datos de una columna de una fila de la tabla, se aplicará a todas las filas.

   ![Edición de tablas en el Explorador de Storage](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Edición de tablas en el Explorador de Storage")

Para ejecutar consultas, haga clic en **Consultar** en la barra de comandos y escriba las condiciones.  

   ![Consulta de tablas en el Explorador de Storage](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Consulta de tablas en el Explorador de Storage")

## <a name="clean-up"></a>Limpieza

Cuando trabaje con su propia suscripción, es una buena idea al final de un proyecto identificar si todavía se necesitan los recursos que ha creado. Los recursos que se dejan en ejecución pueden costarle mucho dinero. Puede eliminar los recursos de forma individual o eliminar el grupo de recursos para eliminar todo el conjunto de recursos.

Puede encontrar y administrar recursos en el portal, mediante el vínculo **Todos los recursos** o **Grupos de recursos** en el panel de navegación izquierdo.

Si está usando un servicio gratuito, recuerde que está limitado a tres índices, indexadores y orígenes de datos. Puede eliminar elementos individuales en el portal para mantenerse por debajo del límite.

## <a name="next-steps"></a>Pasos siguientes

Conecte este almacén de conocimiento a Power BI para un análisis más profundo, o bien continúe con el código, mediante la API REST y Postman para crear un almacén de información diferente.

> [!div class="nextstepaction"]
> [Conexión con Power BI](knowledge-store-connect-power-bi.md)
> [Creación de un almacén de conocimiento con REST](knowledge-store-create-rest.md)
