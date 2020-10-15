---
ms.openlocfilehash: 849777ac3ec11915c602861fea0c42a49fb08bcb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006465"
---
## <a name="provide-frames-to-the-session"></a>Suministro de fotogramas a la sesión

La sesión de anclaje espacial funciona mediante la asignación del espacio en torno al usuario. Esto ayuda a determinar dónde se encuentran los delimitadores. Las plataformas móviles (iOS y Android) requieren una llamada a la fuente de la cámara para obtener fotogramas de la biblioteca de AR de la plataforma nativa. En cambio, HoloLens examina constantemente el entorno, por lo que no hay ninguna necesidad de realizar una llamada concreta, como en las plataformas móviles.