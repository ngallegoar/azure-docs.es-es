---
ms.openlocfilehash: e7e3a31663e6c166b912ba20166ba0f0f769008a
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006432"
---
## <a name="setting-up-the-library"></a>Configuración de la biblioteca

Invoque `Start()` para permitir que la sesión procese datos del entorno.

Para controlar los eventos generados por la sesión, establezca la propiedad `delegate` de la sesión en un objeto, como la vista. Este objeto debe implementar el protocolo `SSCCloudSpatialAnchorSessionDelegate`.
