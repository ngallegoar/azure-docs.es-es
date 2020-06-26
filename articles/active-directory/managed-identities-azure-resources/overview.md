---
title: Identidades administradas de recursos de Azure
description: Información general sobre Managed Identities for Azure Resources.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 05/20/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: aeeb7b2704474e030e00eda03e73fd523434a207
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2020
ms.locfileid: "84976131"
---
# <a name="what-are-managed-identities-for-azure-resources"></a>¿Qué son las identidades administradas de recursos de Azure?

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Como desarrollador de la nube, es probable que busque el método más sencillo y más seguro para acceder a los recursos de Azure en el código. 

Las identidades administradas para los recursos de Azure pueden ayudarle con este requisito ya que las identidades administradas:

- **Eliminan** la necesidad de credenciales en el código.
- **Cambian** las credenciales automáticamente.
- **Reducen** su participación en la administración de identidades al mínimo.


## <a name="how-it-works"></a>Funcionamiento 

Todos los recursos de Azure que admiten identidades administradas pueden obtener tokens para intercambiar datos sin tener credenciales en el código. El proceso consta de los pasos siguientes:

 
1.  **Habilitar**: crea la identidad administrada para el recurso.
2.  **Conceder acceso**: permite el acceso a los recursos con el control de acceso basado en roles de Azure.
3.  **Acceder**: realiza las acciones permitidas.
4.  **Deshabilitar**: elimina la identidad administrada. 

## <a name="managed-identity-types"></a>Tipos de identidad administrada

Hay dos tipos de identidades administradas:

- Identidad administrada asignada por el sistema

- Identidad administrada asignada por el usuario


En el caso de los recursos independientes de Azure, puede habilitar identidades administradas **asignadas por el sistema**. Las identidades administradas asignadas por el sistema proporcionan la compatibilidad más conveniente desde la perspectiva de la administración de identidades. Con solo un clic, puede habilitar la administración automatizada del ciclo de vida de una identidad para el recurso.   

 ![Identidad administrada asignada por el sistema](./media/overview/system-assigned.png)  

Aunque las identidades administradas asignadas por el sistema proporcionan la solución más conveniente para los recursos independientes, todo cambia si necesita administrar un grupo de recursos de Azure para la misma tarea. En este escenario, es mejor crear una identidad manualmente y asignar esta identidad maestra a todos los recursos de Azure que necesita agrupar. Esta asignación se denomina identidad administrada **asignada por el usuario**. 
  

## <a name="supported-services"></a>Servicios admitidos

Puede usar las identidades administradas para recursos de Azure para autenticarse en los servicios que admiten la autenticación de Azure AD. Para ver una lista de los servicios de Azure que admiten la característica Managed Identities for Azure Resources, consulte [Services that support managed identities for Azure resources](services-support-msi.md) (Servicios que admiten la característica Managed Identities for Azure Resources).


## <a name="next-steps"></a>Pasos siguientes

Empiece a utilizar la característica Managed Identities for Azure Resources con las guías de inicio rápido siguientes:

* [Uso de las identidades administradas asignadas por el sistema de una máquina virtual Windows para acceder a Resource Manager](tutorial-windows-vm-access-arm.md)
* [Uso de las identidades administradas asignadas por el sistema de una máquina virtual Linux para acceder a Resource Manager](tutorial-linux-vm-access-arm.md)
