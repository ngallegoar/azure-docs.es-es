---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 08/27/2020
ms.author: msmbaldwin
ms.openlocfilehash: 8c8c9563c954e3d1a84ea2b9f411187d5fb9f226
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026502"
---
Key Vault admite hasta 1024 entradas de directiva de acceso, donde cada entrada concede un conjunto de permisos distinto a una entidad de servicio particular: Debido a esta limitación, se recomienda asignar directivas de acceso a grupos de usuarios, siempre que sea posible, en lugar de a usuarios individuales. El uso de grupos facilita la administración de permisos para varias personas de la organización. Para más información, consulte [Administración del acceso a recursos y aplicaciones con grupos en Azure Active Directory](../articles/active-directory/fundamentals/active-directory-manage-groups.md).

Para la información completa sobre el control de acceso a Key Vault, consulte [Seguridad en Azure Key Vault: Administración de identidades y acceso](../articles/key-vault/general/overview-security.md#identity-and-access-management).