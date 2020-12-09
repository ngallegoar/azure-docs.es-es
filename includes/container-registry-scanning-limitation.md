---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 09/17/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9528fa3b434a0bc09d191fe6808214feb8245ae4
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509322"
---
> [!NOTE]
> Actualmente, Azure Security Center no puede realizar el [análisis de vulnerabilidades de imagen](../articles/security-center/defender-for-container-registries-introduction.md?bc=%2fazure%2fcontainer-registry%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fcontainer-registry%2ftoc.json) en un registro que restringe el acceso a los puntos de conexión privados, las subredes seleccionadas o las direcciones IP. Las instancias de determinados servicios de Azure, como Azure DevOps Services y Azure Container Instances, tampoco pueden tener acceso a un registro de contenedor con restricciones de red.