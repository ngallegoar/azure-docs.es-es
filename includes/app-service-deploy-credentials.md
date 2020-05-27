---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: cephalin
ms.openlocfilehash: c3fa57dd162fbbfbf0d46f73bffc78f279ef2968
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649135"
---
* **Credenciales de nivel de usuario**: un conjunto de credenciales para toda la cuenta de Azure. Se puede utilizar para implementar App Service en cualquier aplicación o suscripción para la que la cuenta de Azure tenga permiso de acceso. Este es también el conjunto predeterminado que aparece en la interfaz gráfica de usuario del portal (como la **información general** y las **propiedades** de la [página de recursos](../articles/azure-resource-manager/management/manage-resources-portal.md#manage-resources) de la aplicación). Cuando a un usuario se le otorga acceso a la aplicación a través del Control de acceso basado en rol (RBAC) o mediante permisos de coadministrador, ese usuario puede usar sus propias credenciales de nivel de usuario hasta que se le revoque el acceso. No comparta estas credenciales con otros usuarios de Azure.

* **Credenciales de nivel de aplicación**: un conjunto de credenciales para cada aplicación. Se puede utilizar para implementar únicamente en esa aplicación. Las credenciales de cada aplicación se generan automáticamente cuando se crea la aplicación. Recuerde que no se pueden configurar manualmente, pero se pueden restablecer en cualquier momento. Para que un usuario obtenga acceso a las credenciales de nivel de aplicación a través del control de acceso basado en rol, ese usuario debe ser un colaborador o tener un rol superior en la aplicación (incluido el rol integrado de colaborador de sitio web). A los lectores no se les permite publicar contenido y no pueden obtener acceso a dichas credenciales.