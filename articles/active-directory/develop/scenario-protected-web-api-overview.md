---
title: 'API web protegida: Información general'
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una API web protegida (información general).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: abe4ecf77e7a65251830ff822b15f79291471202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773337"
---
# <a name="scenario-protected-web-api"></a>Escenario: API web protegida

En este escenario, aprenderá a exponer una API web. También aprenderá a proteger la API web para que solo los usuarios autenticados puedan tener acceso a ella.

Para usar la API web, debe habilitar usuarios autenticados con cuentas profesionales y educativas, o bien habilitar cuentas de Microsoft personales.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Características específicas

Esta es la información específica que necesita saber para proteger las API web:

- El registro de la aplicación debe exponer al menos un ámbito. La versión del token aceptada por la API web depende de la audiencia de inicio de sesión.
- La configuración de código de la API web debe validar el token usado cuando se llama a la API web.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-protected-web-api-app-registration.md)
