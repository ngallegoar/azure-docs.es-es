---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: c9b4e0fdf95c2bb4888a3a11820f1785d8545bd4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993078"
---
### <a name="access-tokens"></a>Tokens de acceso

Los tokens de acceso son un método más sólido para autenticarse con Azure Spatial Anchors, sobre todo al preparar la aplicación para una implementación de producción. En resumen, este enfoque consiste en configurar un servicio back-end con el que la aplicación cliente se pueda autenticar de forma segura. El servicio de back-end se comunica con AAD en tiempo de ejecución y con el servicio de token seguro de Azure Spatial Anchors para solicitar un token de acceso. Después, este token se entrega a la aplicación cliente y se usa en el SDK para autenticarse con Azure Spatial Anchors.
