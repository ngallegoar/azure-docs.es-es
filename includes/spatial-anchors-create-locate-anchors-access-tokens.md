---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 63725d55e2b2935ec6a899789249259b096865c3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186010"
---
### <a name="access-tokens"></a>Tokens de acceso

Los tokens de acceso son un método más sólido para autenticarse con Azure Spatial Anchors, sobre todo al preparar la aplicación para una implementación de producción. En resumen, este enfoque consiste en configurar un servicio back-end con el que la aplicación cliente se pueda autenticar de forma segura. El servicio de back-end se comunica con AAD en tiempo de ejecución y con el servicio de token seguro de Azure Spatial Anchors para solicitar un token de acceso. Después, este token se entrega a la aplicación cliente y se usa en el SDK para autenticarse con Azure Spatial Anchors.
