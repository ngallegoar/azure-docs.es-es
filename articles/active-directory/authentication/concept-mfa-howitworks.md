---
title: 'Cómo funciona Azure MFA: Azure Active Directory'
description: Azure Multi-Factor Authentication ayuda a proteger el acceso a los datos y las aplicaciones, además de satisfacer la demanda de los usuarios de un proceso de inicio de sesión simple.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39948214f5bd080be417ed515bea6bff87d3b303
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484067"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Funcionamiento: Azure Multi-Factor Authentication

La seguridad de la comprobación en pos pasos se basa en el enfoque por niveles. El uso de varias fases de autenticación supone un reto importante para los atacantes. Aunque un atacante consiga descifrar la contraseña de usuario, no servirá de nada si no dispone también del método de autenticación adicional. Funciona mediante la solicitud de dos, o más, de los siguientes métodos de autenticación:

* Un elemento que conoce (normalmente una contraseña).
* Un elemento del que dispone (un dispositivo de confianza que no se puede duplicar con facilidad, como un teléfono).
* Un elemento físico que le identifica (biométrica).

<center>

![Imagen de los métodos de autenticación conceptuales](./media/concept-mfa-howitworks/methods.png)</center>

Azure Multi-Factor Authentication (MFA) ayuda a proteger el acceso a los datos y aplicaciones, y al mismo tiempo mantiene la simplicidad para los usuarios. Proporciona más seguridad, ya que requiere una segunda forma de autenticación y ofrece autenticación segura a través de una gama de [método de autenticación](concept-authentication-methods.md) fáciles de usar. Puede que los usuarios se vean desafiados por la MFA en función de las decisiones de configuración que tome un administrador.

## <a name="how-to-get-multi-factor-authentication"></a>¿Cómo se consigue Multi-Factor Authentication?

Multi-Factor Authentication forma parte de las siguientes ofertas:

* **Azure Active Directory Premium** o **Microsoft 365 Empresa**: uso de la versión completa de Azure Multi-Factor Authentication mediante directivas de acceso condicional para requerir autenticación multifactor con funciones completas.

* Licencias de **Azure AD Free** u **Office 365** independiente: uso de [opciones predeterminadas de seguridad](../fundamentals/concept-fundamentals-security-defaults.md) a fin de requerir autenticación multifactor para los usuarios y administradores.

* **Administradores globales de Azure Active Directory**: un subconjunto de las funcionalidades de capacidades de Azure Multi-Factor Authentication está disponible como medio de protección de las cuentas de administrador global.

> [!NOTE]
> A partir del 1 de septiembre de 2018, los nuevos clientes ya no pueden comprar Azure Multi-factor Authentication como oferta independiente. La autenticación multifactor seguirá siendo una característica disponible en las licencias de Azure AD Premium.

## <a name="supportability"></a>Compatibilidad

Dado que la mayoría de los usuarios están acostumbrados a usar solo las contraseñas para realizar la autenticación, es importante que la organización comunique el proceso a todos los usuarios. Esto reducir la probabilidad de que los usuarios llamen al departamento de soporte técnico para problemas poco importantes relacionados con MFA. Sin embargo, hay algunos escenarios en los que es necesario deshabilitar temporalmente MFA. Use las instrucciones siguientes para entender cómo administrar estos escenarios:

* Entrene al personal de soporte técnico para administrar escenarios en los que el usuario no puede iniciar sesión porque no tiene acceso a sus métodos de autenticación o porque estos no funcionan correctamente.
   * Mediante las directivas de acceso condicional del servicio Azure MFA, el personal de soporte técnico puede agregar un usuario a un grupo que se excluye de una directiva que requiere MFA.
* Considere el uso de ubicaciones con nombre como una forma de minimizar los mensajes de la verificación en dos pasos. Con esta funcionalidad, los administradores pueden omitir la verificación en dos pasos para los usuarios que inician sesión desde una ubicación de red de confianza segura como un segmento de red que se usa para la incorporación de un usuario nuevo.
* Implemente [Azure AD Identity Protection](../active-directory-identityprotection.md) y desencadene la verificación en dos pasos en función de las detecciones de riesgos.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación paso a paso de Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
