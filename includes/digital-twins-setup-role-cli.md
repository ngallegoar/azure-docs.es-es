---
author: baanders
description: archivo de inclusión para el requisito de rol en la configuración de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 1714fac465531a2d3a5829e7080132510ccd1ec4
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407578"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

## <a name="prerequisites-permission-requirements"></a>Requisitos previos: Requisitos de permisos

Para poder completar todos los pasos de este artículo, debe clasificarse como propietario en la suscripción de Azure. 

Para comprobar su nivel de permisos, ejecute este comando en Cloud Shell:

```azurecli-interactive
az role assignment list --assignee <your-Azure-email>
```

Si es propietario, el valor `roleDefinitionName` de la salida es *Propietario*:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/cloud-shell/owner-role.png" alt-text="Ventana de Cloud Shell que muestra la salida del comando az role assignment list":::

Si ve que el valor es *Colaborador* u otro que no sea *Propietario*, puede continuar de una de las maneras siguientes:
* Póngase en contacto con el propietario de la suscripción y solicítele que complete los pasos de este artículo en su nombre.
* Póngase en contacto con el propietario de la suscripción o con otra persona que tenga el rol de administrador de acceso de usuarios en la suscripción y solicíteles que lo eleven a propietario en la suscripción para que tenga el permiso para continuar por su cuenta. Que esto sea adecuado depende de la organización y del rol que tenga.