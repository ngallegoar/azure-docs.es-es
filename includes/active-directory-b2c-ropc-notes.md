---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/27/2020
ms.author: mimart
ms.openlocfilehash: f4944881e6eff3e9a56a74bc997c280f9fff6be8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78187016"
---
## <a name="ropc-flow-notes"></a>Notas del flujo ROPC
En Azure Active Directory B2C (Azure AD B2C), se admiten las siguientes opciones:

- **Cliente nativo**: la interacción del usuario durante la autenticación se produce cuando se ejecuta el código en un dispositivo en el lado del usuario. El dispositivo puede ser una aplicación móvil que se ejecuta en un sistema operativo nativo como, por ejemplo, Android e iOS.
- **Public client flow** (Flujo de cliente público): en la llamada API, solo se envían credenciales de usuario recopiladas por una aplicación. No se envían las credenciales de la aplicación.
- **Add new claims** (Agregar nuevas notificaciones): se puede cambiar el contenido del token de id. para agregar nuevas notificaciones.

No se admiten los siguientes flujos:

- **De servidor a servidor**: el sistema de protección de identidades necesita una dirección IP de confianza que recopile el autor de la llamada (el cliente nativo) como parte de la interacción. En una llamada API del lado servidor, se utiliza sólo la dirección IP del servidor. Si se supera un umbral dinámico de errores de autenticación, el sistema de protección de identidad puede identificar una dirección IP repetida como un atacante.
- **Confidential client flow (Flujo de cliente confidencial)** : se valida el identificador de cliente de la aplicación, pero no el secreto de la aplicación.

Al usar el flujo ROPC, tenga en cuenta lo siguiente:

- ROPC no funciona cuando se produce una interrupción en el flujo de autenticación que requiere la interacción del usuario. Por ejemplo, cuando una contraseña ha expirado o debe cambiarse, se requiere la autenticación multifactor, o cuando es necesario recopilar más información durante el inicio de sesión (por ejemplo, el consentimiento del usuario).
- ROPC solo admite cuentas locales. Los usuarios no pueden iniciar sesión con proveedores de identidades federados como Microsoft, Google +, Twitter, AD-FS o Facebook.
- La administración de sesiones, que incluye mantener la sesión iniciada (KMSI), no es aplicable.
