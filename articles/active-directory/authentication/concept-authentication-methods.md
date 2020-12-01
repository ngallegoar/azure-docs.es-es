---
title: 'Métodos y características de autenticación: Azure Active Directory'
description: Obtenga información sobre los diferentes métodos y características de autenticación disponibles en Azure Active Directory para ayudar a mejorar y proteger los eventos de inicio de sesión.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.collection: M365-identity-device-management
ms.custom: contperfq4
ms.openlocfilehash: a166a451c405c2321453e02751baad91c2a14c60
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94840024"
---
# <a name="what-authentication-and-verification-methods-are-available-in-azure-active-directory"></a>¿Qué métodos de autenticación y verificación hay disponibles en Azure Active Directory?

Como parte de la experiencia de inicio de sesión de las cuentas en Azure Active Directory (Azure AD), hay distintas maneras en las que un usuario puede autenticarse. Un nombre de usuario y una contraseña son la manera más común en que los usuarios han presentado sus credenciales a lo largo de la historia. Con las características de autenticación y seguridad modernas de Azure AD, esa contraseña básica se debe complementar o reemplazar con métodos de autenticación más seguros.

![Tabla de puntos fuertes y métodos de autenticación preferidos en Azure AD](media/concept-authentication-methods/authentication-methods.png)

Los métodos de autenticación sin contraseña, como Windows Hello, las claves de seguridad FIDO2 y la aplicación Microsoft Authenticator, proporcionan los eventos de inicio de sesión más seguros.

Multi-Factor Authentication de Azure AD aporta seguridad adicional al mero uso de una contraseña cuando un usuario inicia sesión. Se pueden solicitar al usuario formas adicionales de autenticación, como responder a una notificación push, especificar un código de un token de software o hardware, o responder a un SMS o a una llamada de teléfono.

Para simplificar la experiencia de incorporación de los usuarios y registrarse tanto para MFA como para SSPR, se recomienda [habilitar el registro de información de seguridad combinada](howto-registration-mfa-sspr-combined.md). Para mejorar la resistencia, se recomienda exigir a los usuarios que registren varios métodos de autenticación. Así, si un usuario no tiene disponible un método durante el inicio de sesión o el autoservicio de restablecimiento de contraseña, puede elegir autenticarse con otro método. Para obtener más información, consulte [Crear una estrategia de administración de control de acceso resistente con Azure Active Directory](concept-resilient-controls.md).

## <a name="authentication-method-strength-and-security"></a>Seguridad del método de autenticación

Cuando implemente características, como Multi-Factor Authentication de Azure AD en su organización, revise los métodos de autenticación disponibles. Elija los métodos que cumplan o superen sus requisitos en cuanto a seguridad, facilidad de uso y disponibilidad. Siempre que sea posible, use métodos de autenticación con el nivel de seguridad más alto.

En la tabla siguiente se describen las consideraciones sobre seguridad que se deben tener en cuenta en los métodos de autenticación disponibles. La disponibilidad es una indicación de que el usuario puede usar el método de autenticación, no de la disponibilidad del servicio en Azure AD:

| Método de autenticación          | Seguridad | Facilidad de uso | Disponibilidad |
|--------------------------------|:--------:|:---------:|:------------:|
| Windows Hello para empresas     | Alto     | Alto      | Alto         |
| Aplicación Microsoft Authenticator    | Alto     | Alto      | Alto         |
| Clave de seguridad FIDO2 (versión preliminar)   | Alto     | Alto      | Alto         |
| Tokens de hardware OATH (versión preliminar) | Media   | Media    | Alto         |
| Tokens de software OATH           | Media   | Media    | Alto         |
| SMS                            | Media   | Alto      | Media       |
| Voz                          | Media   | Media    | Media       |
| Contraseña                       | Bajo      | Alto      | Alto         |

Para obtener más información sobre la seguridad, consulte las [vulnerabilidades de autenticación y los vectores de ataque](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/all-your-creds-are-belong-to-us/ba-p/855124).

> [!TIP]
> Para mejorar la flexibilidad y facilidad de uso, se recomienda usar la aplicación Microsoft Authenticator. Este método de autenticación ofrece la mejor experiencia de usuario y varios modos, como la opción sin contraseña, las notificaciones push MFA y los códigos OATH.

## <a name="how-each-authentication-method-works"></a>Funcionamiento de cada método de autenticación

Algunos métodos de autenticación se pueden usar como factor principal al iniciar sesión en una aplicación o un dispositivo, como el uso de una clave de seguridad FIDO2 o una contraseña. Otros métodos de autenticación solo están disponibles como factor secundario cuando se usa Multi-Factor Authentication de Azure AD o SSPR.

En la tabla siguiente se describe cuándo se puede usar un método de autenticación en un evento de inicio de sesión:

| Método                         | Autenticación principal | Autenticación secundaria  |
|--------------------------------|:----------------------:|:-------------------------:|
| Windows Hello para empresas     | Sí                    | MFA                       |
| Aplicación Microsoft Authenticator    | Sí (versión preliminar)          | MFA y SSPR              |
| Clave de seguridad FIDO2 (versión preliminar)   | Sí                    | MFA                       |
| Tokens de hardware OATH (versión preliminar) | No                     | MFA                       |
| Tokens de software OATH           | No                     | MFA                       |
| SMS                            | Sí (versión preliminar)          | MFA y SSPR              |
| Llamada de voz                     | No                     | MFA y SSPR              |
| Contraseña                       | Sí                    |                           |

Todos estos métodos de autenticación se pueden configurar en Azure Portal y, cada vez más, con la [versión beta de la API REST Microsoft Graph](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

Para obtener más información sobre cómo funciona cada método de autenticación, consulte los siguientes artículos conceptuales independientes:

* [Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-overview)
* [Aplicación Microsoft Authenticator](concept-authentication-authenticator-app.md)
* [Clave de seguridad FIDO2 (versión preliminar)](concept-authentication-passwordless.md#fido2-security-keys)
* [Tokens de hardware OATH (versión preliminar)](concept-authentication-oath-tokens.md#oath-hardware-tokens-preview)
* [Tokens de software OATH](concept-authentication-oath-tokens.md#oath-software-tokens)
* [Inicio de sesión (versión preliminar)](howto-authentication-sms-signin.md) y [verificación](concept-authentication-phone-options.md#mobile-phone-verification) por SMS
* [Verificación por llamada de voz](concept-authentication-phone-options.md)
* Contraseña

> [!NOTE]
> En Azure AD, la contraseña suele ser uno de los métodos de autenticación principales. El método de autenticación de contraseña no se puede deshabilitar. Si usa una contraseña como factor de autenticación principal, aumente la seguridad de los eventos de inicio de sesión con Multi-Factor Authentication de Azure AD.

En algunos escenarios se pueden usar los siguientes métodos de verificación adicional:

* [Contraseñas de aplicación](howto-mfa-app-passwords.md): se usan con aplicaciones antiguas que no admiten la autenticación moderna y se pueden configurar para la opción de Multi-Factor Authentication de Azure AD por usuario.
* [Preguntas de seguridad](concept-authentication-security-questions.md): solo se usa en SSPR.
* [Dirección de correo electrónico](concept-sspr-howitworks.md#authentication-methods): solo se usa en SSPR.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, consulte el [tutorial del autoservicio de restablecimiento de contraseña][tutorial-sspr] y [Azure AD Multi-Factor Authentication][tutorial-azure-mfa].

Para obtener más información sobre los conceptos del SSPR, consulte [Funcionamiento: Autoservicio de restablecimiento de contraseña de Azure AD][concept-sspr].

Para más información sobre los conceptos de MFA, consulte [Funcionamiento: Multi-Factor Authentication de Azure AD][concept-mfa].

Más información sobre la configuración de métodos de autenticación con la [Versión beta de la API REST Microsoft Graph API](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

Para revisar qué métodos de autenticación están en uso, consulte [Análisis de métodos de autenticación de Multi-Factor Authentication de Azure AD con PowerShell](/samples/azure-samples/azure-mfa-authentication-method-analysis/azure-mfa-authentication-method-analysis/).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
