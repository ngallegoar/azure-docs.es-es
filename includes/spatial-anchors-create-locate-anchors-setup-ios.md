---
ms.openlocfilehash: 26eff7b10542843be561264c316bdcf1a63271f7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186016"
---
## <a name="setting-up-the-library"></a>Configuración de la biblioteca

Invoque Start() para permitir que la sesión procese datos del entorno.

Para controlar los eventos generados por la sesión, establezca la propiedad `delegate` de la sesión en un objeto, como la vista. Este objeto debe implementar el protocolo SSCCloudSpatialAnchorSessionDelegate.
