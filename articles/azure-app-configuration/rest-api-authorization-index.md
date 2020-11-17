---
title: 'API REST de Azure App Configuration: autorización'
description: Páginas de referencia para la autorización mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 54f9cfab1f49837a3765c978de6deeb9e5e7d644
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423781"
---
# <a name="authorization"></a>Authorization

La autorización se refiere al procedimiento que se usa para determinar los permisos que tiene un autor de llamada cuando envía una solicitud. Hay varios modelos de autorización. El modelo de autorización que se usa para una solicitud depende del método de [autenticación](./rest-api-authentication-index.md) que se utilice. A continuación se indican los modelos de autorización.

## <a name="hmac"></a>HMAC

El [modelo de autorización](./rest-api-authorization-hmac.md) asociado a la autenticación HMAC divide los permisos en permisos de solo lectura o permisos de lectura y escritura. Consulte la página de [autorización HMAC](./rest-api-authorization-hmac.md) para obtener más información.

## <a name="azure-active-directory"></a>Azure Active Directory

El [modelo de autorización](./rest-api-authorization-azure-ad.md) asociado a la autenticación de Azure Active Directory (Azure AD) usa Azure RBAC para controlar los permisos. Consulte la página de [autorización de Azure AD](./rest-api-authorization-azure-ad.md) para obtener más información.
