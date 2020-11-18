---
title: Requisitos previos de la CLI de Azure para Azure HPC Cache
description: Pasos de configuración para usar la CLI de Azure para crear o modificar Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/08/2020
ms.author: v-erkel
ms.openlocfilehash: 13f45c96a830110bd0f4a2d4a2b422921d7a2e31
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654463"
---
# <a name="set-up-azure-cli-for-azure-hpc-cache"></a>Configuración de la CLI de Azure para Azure HPC Cache

Siga estos pasos para preparar el entorno antes de usar la CLI de Azure para crear o administrar la extensión Azure HPC Cache.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

 - Azure HPC Cache requiere la versión 2.7 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="set-default-resource-group-optional"></a>Configuración del grupo de recursos predeterminado (opcional)

La mayoría de los comandos hpc-cache requieren que se pase el grupo de recursos de la memoria caché. Puede establecer el grupo de recursos predeterminado mediante [az configure](/cli/azure/reference-index#az-configure).

## <a name="next-steps"></a>Pasos siguientes

Después de instalar la extensión de la CLI de Azure e iniciar sesión, puede usar la CLI de Azure para crear y administrar sistemas de Azure HPC Cache.

* [Creación de una instancia de Azure HPC Cache](hpc-cache-create.md)
* [Documentación de hpc-cache de la CLI de Azure](/cli/azure/ext/hpc-cache/hpc-cache)
