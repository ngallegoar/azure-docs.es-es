---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 5360cbb7bdfbdc59e87366e73a891b5c2583672e
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993119"
---
Una vez creado el monitor, se desencadenará el evento `AnchorLocated` para cada delimitador solicitado. Este evento se desencadena cuando se encuentra un delimitador o si no puede encontrarse. Si se produce esta situación, el motivo se incluirá en el estado. Una vez procesados todos los delimitadores de un monitor, tanto si se encuentran como si no, se desencadenará el evento `LocateAnchorsCompleted`. Hay un límite de 35 identificadores por monitor. 
