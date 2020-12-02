---
title: 'Ejemplo de script de la CLI de Azure: creación de una transformación | Microsoft Docs'
description: Las transformaciones describen un flujo de trabajo de tareas simple para procesar archivos de vídeo o audio (que se conoce habitualmente como "receta"). El script de la CLI de Azure de este artículo muestra cómo crear una transformación.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/18/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4784bb8a396a30e9002c789c7aad382640f04a5c
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "95996917"
---
# <a name="create-a-transform"></a>Creación de una transformación

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

El script de la CLI de Azure de este artículo muestra cómo crear una transformación. Las transformaciones describen un flujo de trabajo de tareas simple para procesar archivos de vídeo o audio (que se conoce habitualmente como "receta"). Siempre debe comprobar si ya existe una transformación con el nombre y la "receta" elegidos. Si es así, debe volver a utilizarlos.

## <a name="prerequisites"></a>Prerrequisitos

[Cree una cuenta de Media Services](./create-account-howto.md).

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

> [!NOTE]
> Solo puede especificar una ruta de acceso a un archivo JSON preestablecido del codificador estándar personalizado para [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset), consulte el ejemplo [Codificación con una transformación personalizada](custom-preset-cli-howto.md).
>
> No se puede pasar un nombre de archivo cuando se usa [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset).

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="rest"></a>[REST](#tab/rest/)

[!INCLUDE [task general transform creation](./includes/task-create-basic-audio-rest.md)]

---

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre las transformaciones y los trabajos](transforms-jobs-concept.md)
