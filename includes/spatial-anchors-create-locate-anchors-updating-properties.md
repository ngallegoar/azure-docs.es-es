---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186028"
---
## <a name="update-properties"></a>Actualización de propiedades

Para actualizar las propiedades de un delimitador, se usa el método `UpdateAnchorProperties()`. Si dos o más dispositivos intentan actualizar las propiedades del mismo delimitador al mismo tiempo, se usa un modelo de simultaneidad optimista, lo que significa que la primera escritura tendrá prioridad.  En todas las demás se producirá un error de "simultaneidad", es decir, será necesario actualizar las propiedades antes de intentarlo de nuevo.
