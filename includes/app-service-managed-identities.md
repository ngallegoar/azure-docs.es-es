---
author: mattchenderson
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: mahender
ms.openlocfilehash: 3b282a99bb7f6f107717d9c265a46d285d03b849
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649077"
---
Una identidad administrada de Azure Active Directory (Azure AD) permite a la aplicación acceder fácilmente a otros recursos protegidos por Azure AD, como Azure Key Vault. La identidad está administrada por la plataforma Azure y no requiere que aprovisione o rote los secretos. Para más información sobre las identidades administradas en Azure AD, consulte [Identidades administradas para recursos de Azure](../articles/active-directory/managed-identities-azure-resources/overview.md).

La aplicación puede tener dos tipos de identidades:

- Una **identidad asignada por el sistema** está asociada a la aplicación y se elimina si se elimina la aplicación. Una aplicación solo puede tener una identidad asignada por el sistema.
- Una **identidad asignada por el usuario** es un recurso de Azure independiente que puede asignarse a la aplicación. Una aplicación puede tener varias identidades asignadas por el usuario.