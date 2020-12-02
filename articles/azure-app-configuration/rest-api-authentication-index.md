---
title: 'API REST de Azure App Configuration: autenticación'
description: Páginas de referencia para la autenticación mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 56416009395ebf8270ad0fa8d141277424dd6d9a
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183471"
---
# <a name="authentication"></a>Autenticación

Se deben autenticar todas las solicitudes HTTP. Se admiten los siguientes esquemas de autenticación.

## <a name="hmac"></a>HMAC

La [autenticación HMAC](./rest-api-authentication-hmac.md) usa un secreto generado aleatoriamente para firmar las cargas de las solicitudes. Puede encontrar información detallada sobre cómo se autorizan las solicitudes que usan este método de autenticación en la sección [Autorización de HMAC](./rest-api-authorization-hmac.md).

## <a name="azure-active-directory"></a>Azure Active Directory

La [autenticación de Azure Active Directory (Azure AD)](../active-directory/authentication/overview-authentication.md) emplea un token de portador que se obtiene de Azure Active Directory para autenticar las solicitudes. Puede encontrar información detallada sobre cómo se autorizan las solicitudes que usan este método de autenticación en la sección [Autorización de Azure AD](./rest-api-authorization-azure-ad.md).