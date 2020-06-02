---
ms.openlocfilehash: a165061b2f927d6302504819ef27c846d76d0f30
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006402"
---
## <a name="pause-reset-or-stop-the-session"></a>Pausa, restablecimiento o detención de la sesión

Para detener la sesión temporalmente, puede invocar `Stop()`. Si lo hace, se detendrán los monitores y el procesamiento de entorno, incluso si invoca `ProcessFrame()`. Puede invocar `Start()` para reanudar el procesamiento. Cuando se reanude, se conservarán los datos del entorno que ya se hayan capturado en la sesión.
