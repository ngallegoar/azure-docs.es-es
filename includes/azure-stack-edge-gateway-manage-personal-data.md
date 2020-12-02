---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: af4c3829c8b12bfcaae0602dde8f459de7e50f3a
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465888"
---
- **Detalles de pedido**. Una vez creado el pedido, la dirección de envío, la dirección de correo electrónico y la información de contacto del usuario se almacenan en Azure Portal. Entre la información guardada se incluyen los siguientes datos:
  - Nombre de contacto
  - Número de teléfono
  - Dirección de correo electrónico
  - Dirección
  - City
  - Código postal
  - State
  - País/provincia/región
  - Número de seguimiento del envío

    Los detalles del pedido se cifran y almacenan en el servicio. El servicio conserva la información hasta que elimine explícitamente el recurso o el pedido. La eliminación del recurso y el pedido correspondiente se bloquea desde el momento en que se envía el dispositivo hasta que el dispositivo vuelve a Microsoft.

- **Dirección de envío**. Después de realizar el pedido, el servicio Data Box proporciona la dirección de envío a compañías de transporte como UPS.

- **Usuarios de los recursos compartidos**. Los usuarios en el dispositivo también pueden acceder a los datos que se encuentran en los recursos compartidos. Se puede ver una lista de los usuarios que pueden acceder a los datos del recurso compartido. Cuando los recursos compartidos se eliminan, esta lista también se elimina.