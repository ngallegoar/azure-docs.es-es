---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: 893beb0800af0eece4d69e727e427c3e92b79121
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "76044912"
---
El control de acceso basado en rol es una estrategia de seguridad basada en la herencia para la administración de accesos, permisos y roles. Los roles descendientes heredan permisos de los roles primarios. También se pueden asignar permisos sin que se hereden de un rol primario. Además, se pueden asignar para personalizar un rol según sea necesario.

Por ejemplo, un Administrador del espacio podría necesitar acceso global para ejecutar todas las operaciones para un espacio especificado. El acceso incluye todos los nodos bajo o dentro del espacio. Un Instalador de dispositivos puede necesitar solamente permisos de *lectura* y *actualización* para los dispositivos y sensores.

En todos los casos, se concede a los roles *el acceso exacto y no más del necesario* para completar sus tareas, siguiendo el principio de privilegio mínimo. Según este principio, se concede a una identidad *solo*:

* La cantidad de acceso necesario para completar su trabajo.
* Un rol adecuado y limitado para llevar a cabo su trabajo.

>[!IMPORTANT]
> Siga siempre el principio de privilegio mínimo.

Otros dos procedimientos importantes del control de acceso basado en rol que deben seguirse:

> [!div class="checklist"]
> * Audite periódicamente las asignaciones de roles para comprobar que cada rol tiene los permisos correctos.
> * Limpie los roles y las asignaciones cuando las personas cambien de rol o de asignación.