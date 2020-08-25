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
ms.openlocfilehash: 7b362ba30baf13211dc41a6043dcedd05bd506ea
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2020
ms.locfileid: "88585428"
---
# <a name="create-an-asset"></a>Creación de un recurso

En este artículo se muestra cómo crear un recurso de Media Services.  Usará un recurso para almacenar el contenido multimedia de la codificación y el streaming.  Para más información sobre los recursos de Media Services, lea [Recursos en Azure Media Services v3](assets-concept.md).

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos descritos de [Creación de una cuenta de Media Services](./create-account-howto.md) para crear la cuenta de Media Services y el grupo de recursos necesarios para crear un recurso.

## <a name="methods"></a>Métodos

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Create an asset with CLI](./includes/task-create-asset-cli.md)]

## <a name="cli-shell"></a>[Shell de la CLI](#tab/clishell/)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

---

## <a name="next-steps"></a>Pasos siguientes

[Administración de recursos](manage-asset-concept.md)
