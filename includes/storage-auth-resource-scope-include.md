---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/17/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 74ef8270b5efcd3b7cdf756c103dcc2e1c935508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75460507"
---
Antes de asignar un rol RBAC a una entidad de seguridad, determine el ámbito de acceso que debería tener la entidad de seguridad. Los procedimientos recomendados dictan que siempre es mejor conceder únicamente el ámbito más restringido posible.

En la lista siguiente se describen los niveles en los que puede definir el ámbito de acceso a recursos de blob y cola de Azure, empezando por el ámbito más restringido:

- **Un contenedor individual.** En este ámbito, se aplica una asignación de roles a todos los blobs del contenedor, así como las propiedades y los metadatos del contenedor.
- **Una cola individual.** En este ámbito, se aplica una asignación de roles a los mensajes de la cola, así como las propiedades y los metadatos de la cola.
- **La cuenta de almacenamiento.** En este ámbito, se aplica una asignación de roles a todos los contenedores y sus blobs, o bien a todas las colas y sus mensajes.
- **El grupo de recursos.** En este ámbito, se aplica una asignación de roles a todos los contenedores o colas de todas las cuentas de almacenamiento del grupo de recursos.
- **La suscripción.** En este ámbito, se aplica una asignación de roles a todos los contenedores o las colas de todas las cuentas de almacenamiento de todos los grupos de recursos de la suscripción.

> [!IMPORTANT]
> Si la suscripción incluye un espacio de nombres de Azure DataBricks, los roles del ámbito de la suscripción no podrán conceder acceso a los datos del blob y la cola. Defina el ámbito de los roles en el grupo de recursos, la cuenta de almacenamiento, o bien el contenedor o la cola en su lugar.     
