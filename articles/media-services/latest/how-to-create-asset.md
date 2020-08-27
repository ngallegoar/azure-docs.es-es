---
title: Carga de contenido en un recurso de Azure Media Services mediante la CLI de Azure
description: El script de la CLI de Azure de este tema muestra cómo crear un recurso de Media Services al que cargar contenido.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 65b0b276dcda58810f44494c14f06dd412d633f6
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719298"
---
# <a name="create-an-asset"></a>Creación de un recurso

En este artículo se muestra cómo crear un recurso de Media Services.  Usará un recurso para almacenar el contenido multimedia de la codificación y el streaming.  Para más información sobre los recursos de Media Services, lea [Recursos en Azure Media Services v3](assets-concept.md).

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos descritos de [Creación de una cuenta de Media Services](./create-account-howto.md) para crear la cuenta de Media Services y el grupo de recursos necesarios para crear un recurso.

## <a name="methods"></a>Métodos

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Create an asset with CLI](./includes/task-create-asset-cli.md)]

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

## <a name="rest"></a>[REST](#tab/rest/)

### <a name="using-rest"></a>Uso de REST

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-rest.md)]

### <a name="using-curl"></a>Uso de cURL

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-curl.md)]

## <a name="using-postman"></a>Uso de Postman

[!INCLUDE[Create an asset with Postman](./includes/task-create-asset-postman.md)]

## <a name="net"></a>[.NET](#tab/net/)

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-dotnet.md)]

---

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Media Services](media-services-overview.md)
