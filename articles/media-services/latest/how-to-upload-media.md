---
title: Carga de soportes físicos
titleSuffix: Azure Media Services
description: Obtenga información sobre cómo cargar elementos multimedia para la transmisión por secuencias o la codificación.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: inhenkel
ms.openlocfilehash: a046a3caba9a0909d873356bda8d1fa6cf1f9860
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719264"
---
# <a name="upload-media-for-streaming-or-encoding"></a>Carga de elementos multimedia para la transmisión por secuencias o la codificación

En Media Services, los archivos digitales (elementos multimedia) se cargan en un contenedor de blobs asociado con un recurso. La entidad [Recurso](/rest/api/media/operations/asset) puede contener archivos de vídeo, audio, imágenes, colecciones de miniaturas, pistas de texto y subtítulos (y los metadatos sobre estos archivos). Una vez cargados los archivos en el contenedor del recurso, el contenido se almacena de forma segura en la nube para un posterior procesamiento y streaming.

Antes de empezar, deberá recopilar o pensar algunos valores.

1. La ruta de acceso del archivo local que desea cargar
1. El identificador de recurso del recurso (contenedor)
1. El nombre que desea usar para el archivo cargado, incluida la extensión
1. El nombre de la cuenta de almacenamiento que utiliza
1. La clave de acceso de la cuenta de almacenamiento

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-portal.md)]

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-cli.md)]

## <a name="rest"></a>[REST](#tab/rest/)

Una vez que haya [creado un recurso mediante Postman u otro método REST y haya obtenido la dirección URL de SAS del recurso](how-to-create-asset.md?tabs=rest), use las API de Azure Storage o los SDK (por ejemplo, la [API REST de Storage](../../storage/common/storage-rest-api-auth.md) o el [SDK para .NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md)).

---
<!-- add these to the tabs when available -->
Para otros métodos, consulte la [documentación de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/) para trabajar con blobs en [.NET](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet), [Java](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-java), [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python) y [JavaScript (Node.js)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-nodejs).

## <a name="next-steps"></a>Pasos siguientes

> [Introducción a Media Services](media-services-overview.md)
