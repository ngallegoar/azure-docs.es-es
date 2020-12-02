---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 7210b05566f5cd6f3c56792bce0904b3c9b645da
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993046"
---
## <a name="set-up-authentication"></a>Configuración de la autenticación

Para acceder al servicio, deberá proporcionar una clave de cuenta, un token de acceso o un token de autenticación de Azure Active Directory. También puede leer más información al respecto en la [página Concepto de autenticación](../articles/spatial-anchors/concepts/authentication.md).

### <a name="account-keys"></a>Claves de cuenta

Las claves de cuenta son una credencial que permite que la aplicación se autentique con el servicio Azure Spatial Anchors. Las claves de cuenta están pensadas para ayudarle a empezar a trabajar rápidamente, sobre todo durante la fase de desarrollo de integración de la aplicación con Azure Spatial Anchors. Para usar las claves de cuenta, debe insertarlas en las aplicaciones cliente durante el desarrollo. A medida que avance en el desarrollo, se recomienda encarecidamente que cambie a un mecanismo de autenticación que sea de nivel de producción, compatible con tokens de acceso, o a la autenticación de usuario de Azure Active Directory. Para obtener una clave de cuenta para el desarrollo, visite su cuenta de Azure Spatial Anchors y vaya a la pestaña "Claves".