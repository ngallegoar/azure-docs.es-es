---
title: 'API REST de Azure App Configuration: autenticación'
description: Páginas de referencia para la autenticación mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 21a43a005b78c8916d06e97ca9d2ba21d5a585a3
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423842"
---
# <a name="authentication"></a>Autenticación

Se deben autenticar todas las solicitudes HTTP. Se admiten los siguientes esquemas de autenticación.

## <a name="hmac"></a>HMAC

La [autenticación HMAC](./rest-api-authentication-hmac.md) usa un secreto generado aleatoriamente para firmar las cargas de las solicitudes. Puede encontrar información detallada sobre cómo se autorizan las solicitudes que usan este método de autenticación en la sección [Autorización de HMAC](./rest-api-authorization-hmac.md).

## <a name="azure-active-directory"></a>Azure Active Directory

La [autenticación de Azure Active Directory (Azure AD)](/azure/active-directory/authentication/overview-authentication) emplea un token de portador que se obtiene de Azure Active Directory para autenticar las solicitudes. Puede encontrar información detallada sobre cómo se autorizan las solicitudes que usan este método de autenticación en la sección [Autorización de Azure AD](./rest-api-authorization-azure-ad.md).
