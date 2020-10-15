---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: e5166934c4f7e1ecc917d2a5afcbb26ea9a637a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186119"
---
## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos, las instancias de Azure Cache for Redis y todos los recursos relacionados en el grupo de recursos.

```azurecli
az group delete --name contosoGroup
```