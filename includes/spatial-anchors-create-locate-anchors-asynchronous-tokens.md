---
ms.openlocfilehash: 18f76ea4acc99038939a2653bb66875154529126
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186008"
---
Si tiene que ejecutar un trabajo asincrónico en el controlador, puede aplazar el establecimiento del token. Para ello, solicite un objeto `deferral` y complételo, como se indica en el ejemplo siguiente.
