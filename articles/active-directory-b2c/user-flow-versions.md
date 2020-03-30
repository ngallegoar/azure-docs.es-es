---
title: Versiones de flujos de usuario de Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre las versiones de los flujos de usuario disponibles en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 40d21d3390396e0cb7e44d4e19598f9b0b691087
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185630"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Versiones de flujos de usuario de Azure Active Directory B2C

Los flujos de usuario de Azure Active Directory B2C (Azure AD B2C) le permiten configurar [directivas](user-flow-overview.md) que describan de forma exhaustiva la experiencia de los clientes con las identidades. Estas experiencias incluyen las operaciones de registro, inicio de sesión, restablecimiento de contraseña y edición de perfiles. En Azure AD B2C, puede seleccionar un flujo de usuario entre una colección de flujos de usuario recomendados y de flujos de usuario en versión preliminar.

Los nuevos flujos de usuario se agregan como versiones nuevas. Cuando un flujo de usuario se hace estable, se recomienda para su uso. Los flujos de usuario se identifican como **recomendados** si se han probado exhaustivamente. Se considera que los flujos de usuario están en versión preliminar hasta que se identifican como recomendados. Utilice un flujo de usuario recomendado en las aplicaciones de producción, pero seleccione otra versión para probar las nuevas funcionalidades cuando estén disponibles. No debe usar versiones anteriores de los flujos de usuario recomendados.

>[!IMPORTANT]
> A menos que un flujo de usuario se identifique como **Recomendado**, se considera que está en *versión preliminar*. Solo debe usar flujos de usuario recomendados en las aplicaciones de producción.

## <a name="v1"></a>V1

| Flujo de usuario | Recomendado | Descripción |
| --------- | ----------- | ----------- |
| Restablecimiento de contraseña | Sí | Permite a los usuarios elegir una contraseña nueva después de verificar el correo electrónico. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>Configuración de compatibilidad de tokens</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul> |
| Edición de perfiles | Sí | Permite al usuario configurar los atributos de usuario. Con este flujo de usuario, puede configurar: <ul><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li></ul> |
| Iniciar sesión mediante ROPC | No | Permite a los usuarios con una cuenta local iniciar sesión directamente en las aplicaciones nativas (no se necesita un explorador). Con este flujo de usuario, puede configurar: <ul><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li></ul> |
| Iniciar sesión | No | Permite a los usuarios iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>Bloqueo de inicios de sesión</li><li>Forzar el restablecimiento de contraseñas</li><li>Mantener la sesión iniciada (KMSI)</ul><br>No se puede personalizar la interfaz de usuario con este flujo de usuario. |
| Suscripción | No | Permite a los usuarios crear una cuenta. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul> |
| Registrarse e iniciar sesión | Sí | Permite a los usuarios crear una cuenta o iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul>|

## <a name="v11"></a>V1.1

| Flujo de usuario | Recomendado | Descripción |
| --------- | ----------- | ----------- |
| Restablecimiento de contraseña v1.1 | No | Permite al usuario elegir una nueva contraseña después de comprobar su correo electrónico (nuevo diseño de página disponible). Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>Configuración de compatibilidad de tokens</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul> |

## <a name="v2"></a>V2

| Flujo de usuario | Recomendado | Descripción |
| --------- | ----------- | ----------- |
| Restablecimiento de contraseña v2 | No | Permite a los usuarios elegir una contraseña nueva después de verificar el correo electrónico. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>Configuración de compatibilidad de tokens</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul> |
| Edición de perfil v2 | Sí | Permite al usuario configurar los atributos de usuario. Con este flujo de usuario, puede configurar: <ul><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li></ul> |
| Inicio de sesión v2 | No | Permite a los usuarios iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>Personalización de la página de inicio de sesión</li></ul> |
| Registrarse v2 | No | Permite a los usuarios crear una cuenta. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>[Vigencia de tokens](tokens-overview.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul> |
| Registrarse e iniciar sesión v2 | No | Permite a los usuarios crear una cuenta o iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Autenticación multifactor](custom-policy-multi-factor-authentication.md)</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>[Requisitos de complejidad de la contraseña](user-flow-password-complexity.md)</li></ul> |
