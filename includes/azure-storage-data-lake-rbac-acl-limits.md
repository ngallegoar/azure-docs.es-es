---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 8b793a28c31df185555b9a548df52e4beeccf305
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131639"
---
| Mechanism | Ámbito |Límites | Nivel de permiso admitido |
|---|---|---|---|
| RBAC | Cuentas de almacenamiento, contenedores. <br>Asignaciones de roles RBAC entre recursos en el nivel de suscripción o de grupo de recursos. | 2000 asignaciones de roles RBAC en una suscripción | Roles RBAC (integrados o personalizados) |
| ACL| Directorio, archivo |32 entradas de ACL (realmente 28 entradas de ACL) por archivo y por directorio. Las ACL de acceso y predeterminadas tienen su propio límite de 32 entradas de ACL. |Permiso de ACL|
