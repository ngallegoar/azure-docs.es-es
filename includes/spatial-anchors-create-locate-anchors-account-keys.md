---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 9bd213b63b69a25fb2530cd8f6659abf5357616a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76694547"
---
## <a name="set-up-authentication"></a>Configuración de la autenticación

Para acceder al servicio, deberá proporcionar una clave de cuenta, un token de acceso o un token de autenticación de Azure Active Directory. También puede leer más información al respecto en la [página Concepto de autenticación](/azure/spatial-anchors/concepts/authentication).

### <a name="account-keys"></a>Claves de cuenta

Las claves de cuenta son una credencial que permite que la aplicación se autentique con el servicio Azure Spatial Anchors. Las claves de cuenta están pensadas para ayudarle a empezar a trabajar rápidamente, sobre todo durante la fase de desarrollo de integración de la aplicación con Azure Spatial Anchors. Para usar las claves de cuenta, debe insertarlas en las aplicaciones cliente durante el desarrollo. A medida que avance en el desarrollo, se recomienda encarecidamente que cambie a un mecanismo de autenticación que sea de nivel de producción, compatible con tokens de acceso, o a la autenticación de usuario de Azure Active Directory. Para obtener una clave de cuenta para el desarrollo, visite su cuenta de Azure Spatial Anchors y vaya a la pestaña "Claves".
