---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76694443"
---
## <a name="provide-feedback-to-the-user"></a>Envío de comentarios al usuario

Puede escribir código para controlar el evento actualizado de la sesión. Este evento se desencadena cada vez que la sesión mejora su capacidad de reconocimiento del entorno. Esto le permite hacer lo siguiente:

- Usar la clase `UserFeedback` para proporcionar comentarios al usuario cuando el dispositivo se mueve y la sesión actualiza su capacidad de reconocimiento del entorno. Para ello:
- Determine en qué momento hay suficientes datos espaciales de seguimiento para crear anclajes espaciales, lo que se puede determinar con `ReadyForCreateProgress` o `RecommendedForCreateProgress`. Una vez que `ReadyForCreateProgress` sea superior a 1, tendremos suficientes datos para guardar un anclaje espacial de la nube, aunque se recomienda esperar hasta que `RecommendedForCreateProgress` sea superior a 1 para hacerlo.
