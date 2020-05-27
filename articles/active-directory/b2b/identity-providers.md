---
title: Proveedores de identidades para External Identities - Azure AD
description: La colaboración B2B de Azure Active Directory admite Multi-Factor Authentication (MFA) para poder acceder de manera selectiva a las aplicaciones corporativas.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 395473e70137ead2b7185d54d165f82e9b3f1f04
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595714"
---
# <a name="identity-providers-for-external-identities"></a>Proveedores de identidades para External Identities

Un *proveedor de identidades* crea, mantiene y administra la información de identidad al tiempo que proporciona servicios de autenticación a las aplicaciones. Al compartir aplicaciones y recursos con usuarios externos, Azure AD es el proveedor de identidades predeterminado para el uso compartido. Esto significa que, al invitar a usuarios externos que ya tienen una cuenta de Azure AD o de Microsoft, estos pueden iniciar sesión automáticamente sin tener que realizar ninguna configuración adicional.

Sin embargo, puede permitir que los usuarios inicien sesión con varios proveedores de identidades. Por ejemplo, puede configurar la federación con proveedores de identidades de redes sociales que sean compatibles con Azure AD, incluidos Google y Facebook. También puede federar con cualquier proveedor de identidades externo que admita los protocolos SAML o WS-Fed. Con la federación de proveedores de identidades externos, puede ofrecer a los usuarios externos la posibilidad de iniciar sesión con sus cuentas de redes sociales o de empresa existentes.

## <a name="how-it-works"></a>Funcionamiento

Azure AD External Identities está preconfigurada para la federación con Google y Facebook. Para configurar estos proveedores de identidades en el inquilino de Azure AD, cree una aplicación en cada proveedor de identidades y configure las credenciales. Obtendrá un id. de cliente o de aplicación y un secreto de cliente o de aplicación, que puede agregar a su inquilino de Azure AD.

Una vez que haya agregado un proveedor de identidades a su inquilino de Azure AD:

- Cuando invite a un usuario externo a las aplicaciones o recursos de la organización, el usuario externo puede iniciar sesión con la cuenta que tenga con ese proveedor de identidades.
- Al habilitar el [registro de autoservicio](self-service-sign-up-overview.md) para las aplicaciones, los usuarios externos pueden registrarse en las aplicaciones con las cuentas de los proveedores de identidades que haya agregado. 

Al canjear la invitación o al registrarse en la aplicación, el usuario externo tiene la opción de iniciar sesión y autenticarse con el proveedor de identidades de redes sociales:

![Captura de pantalla que muestra la pantalla de inicio de sesión con las opciones de Google y Facebook.](media/identity-providers/sign-in-with-social-identity.png)

Para conseguir una experiencia de inicio de sesión óptima, fedérese con los proveedores de identidades siempre que sea posible para que pueda proporcionar a sus invitados una experiencia de inicio de sesión sin problemas cuando accedan a las aplicaciones.  

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo agregar proveedores de identidades para iniciar sesión en las aplicaciones, consulte los siguientes artículos:

- [Agregar Google](google-federation.md) a la lista de proveedores de identidades de redes sociales
- [Agregar Facebook](facebook-federation.md) a la lista de proveedores de identidades de redes sociales
- [Configuración de federación directa](direct-federation.md) con cualquier organización cuyo proveedor de identidades (IdP) admita el protocolo SAML 2.0 o WS-Fed. Tenga en cuenta que la federación directa no es una opción para los flujos de usuario de registro de autoservicio.
