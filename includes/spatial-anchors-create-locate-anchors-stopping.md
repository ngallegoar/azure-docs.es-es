---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186036"
---
## <a name="pause-reset-or-stop-the-session"></a>Pausa, restablecimiento o detención de la sesión

Para detener la sesión temporalmente, puede invocar `Stop()`. Si lo hace, se detendrán los monitores y el procesamiento de entorno, incluso si invoca ProcessFrame(). Puede invocar `Start()` para reanudar el procesamiento. Cuando se reanude, se conservarán los datos del entorno que ya se hayan capturado en la sesión.
