---
title: Introducción a Multi-Factor Authentication de Azure AD
description: Aprenda de qué forma Multi-Factor Authentication de Azure AD ayuda a proteger el acceso a los datos y las aplicaciones, además de satisfacer la demanda de los usuarios de un proceso de inicio de sesión simple.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9563ed283229eb6f43d036629cfe8b84fcde25fc
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94839886"
---
# <a name="how-it-works-azure-ad-multi-factor-authentication"></a>Funcionamiento: Azure AD Multi-Factor Authentication

La autenticación multifactor es un procedimiento en el que durante el proceso de inicio de sesión se solicita a un usuario una forma adicional de identificación, como especificar un código en su teléfono móvil o escanear su huella digital.

Si solo usa una contraseña para autenticar a un usuario, deja un vector desprotegido frente a los ataques. Si la contraseña es débil o se ha expuesto en otro lugar, ¿cómo saber si es el usuario quien inicia sesión realmente con el nombre de usuario y la contraseña y no un atacante? Al exigir una segunda forma de autenticación, aumenta la seguridad, ya que este factor adicional no es algo que resulte fácil de obtener o duplicar para un atacante.

![Imagen conceptual de las distintas formas de autenticación multifactor](./media/concept-mfa-howitworks/methods.png)

El funcionamiento de Azure AD Multi-Factor Authentication se basa en exigir uno o varios de los siguientes métodos de autenticación:

* Algo que conoce, normalmente una contraseña.
* Algo que tiene, como un dispositivo de confianza que no se puede duplicar con facilidad (por ejemplo, un teléfono o una clave de hardware).
* Algo que forma parte de usted, información biométrica como una huella digital o una detección de rostro.

Los usuarios pueden registrarse para el autoservicio de restablecimiento de contraseña y Azure AD Multi-Factor Authentication en un solo paso a fin de simplificar la experiencia de incorporación. Los administradores pueden definir qué formas de autenticación secundaria se pueden usar. También se puede exigir Azure AD Multi-Factor Authentication cuando los usuarios realizan un autoservicio de restablecimiento de contraseña para proteger mejor ese proceso.

![Métodos de autenticación en uso en la pantalla de inicio de sesión](media/concept-authentication-methods/overview-login.png)

Multi-Factor Authentication de Azure AD ayuda a proteger el acceso a los datos y las aplicaciones, al mismo tiempo que mantiene la simplicidad para los usuarios. Proporciona más seguridad, ya que requiere una segunda forma de autenticación y ofrece autenticación segura a través de una gama de [método de autenticación](concept-authentication-methods.md) fáciles de usar. Puede que los usuarios se vean desafiados por la MFA en función de las decisiones de configuración que tome un administrador.

No es necesario que sus aplicaciones o servicios realicen ningún cambio para usar Multi-Factor Authentication de Azure AD. Los mensajes de comprobación forman parte del evento de inicio de sesión de Azure AD, que solicita y procesa automáticamente el desafío de MFA cuando es necesario.

## <a name="available-verification-methods"></a>Métodos de comprobación disponibles

Cuando un usuario inicia sesión en una aplicación o servicio, y recibe un mensaje de Multi-Factor Authentication, puede elegir uno de sus formularios registrados de comprobación adicional. Un administrador podría requerir el registro de estos métodos de verificación de Multi-Factor Authentication de Azure AD, o bien el usuario puede acceder a su propia instancia de [Mi perfil](https://myprofile.microsoft.com) para editar o agregar métodos de verificación.

Con Multi-Factor Authentication de Azure AD, se pueden usar las siguientes formas adicionales de verificación:

* Aplicación Microsoft Authenticator
* Token de hardware OATH
* sms
* Llamada de voz

## <a name="how-to-enable-and-use-azure-ad-multi-factor-authentication"></a>Habilitación y uso de Multi-Factor Authentication de Azure AD

Se puede habilitar Multi-Factor Authentication de Azure AD en los usuarios y grupos, de forma que se les solicite una verificación adicional durante el evento de inicio de sesión. [Los valores predeterminados de seguridad](../fundamentals/concept-fundamentals-security-defaults.md) están disponibles para todos los Azure AD inquilinos para habilitar rápidamente el uso de la aplicación Microsoft Authenticator para todos los usuarios.

Para tener controles más pormenorizados, se pueden usar directivas de [acceso condicional](../conditional-access/overview.md) para definir los eventos o aplicaciones que requieren MFA. Estas directivas pueden permitir eventos de inicio de sesión regulares cuando el usuario se encuentra en la red corporativa o en un dispositivo registrado, pero solicitar más factores de comprobación cuando se encuentra en un dispositivo personal o en un entorno remoto.

![Diagrama general sobre el funcionamiento del acceso condicional para proteger el proceso de inicio de sesión](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

## <a name="next-steps"></a>Pasos siguientes

Para información sobre las licencias, consulte [Características y licencias de Multi-Factor Authentication de Azure AD](concept-mfa-licensing.md).

Para ver MFA en acción, habilite Multi-Factor Authentication de Azure AD en un conjunto de usuarios de prueba en el siguiente tutorial:

> [!div class="nextstepaction"]
> [Habilitación de Azure AD Multi-Factor Authentication](./tutorial-enable-azure-mfa.md)