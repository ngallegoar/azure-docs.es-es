---
title: Visualización de un almacén de conocimiento con el Explorador de Storage
titleSuffix: Azure Cognitive Search
description: Visualice y analice un almacén de conocimiento de Azure Cognitive Search con el Explorador de Storage de Azure Portal.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: ba0b02067c032f9038051c169866588ded44af73
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85566030"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Visualización de un almacén de conocimiento con el Explorador de Storage

En este artículo, aprenderá por ejemplo a conectar y explorar un almacén de conocimiento con el Explorador de Storage en Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

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
