---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/11/2020
ms.author: mimart
ms.openlocfilehash: 4e03d076fc9ed7d87326a4c8b4f8bf6c0b21c750
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "79126744"
---
## <a name="disable-email-verification"></a>Deshabilitación de la comprobación de correo electrónico

De forma predeterminada, Azure Active Directory B2C (Azure AD B2C) comprueba la dirección de correo electrónico del cliente para las cuentas locales (cuentas de usuarios que se registran con la dirección de correo electrónico o el nombre de usuario). Azure AD B2C garantiza direcciones de correo electrónico válidas exigiendo que los consumidores puedan comprobarlas durante el proceso de suscripción. También evita que actores malintencionados usen procesos automatizados para generar cuentas fraudulentas en las aplicaciones.

Algunos desarrolladores de aplicaciones prefieren pasar por alto la comprobación de correos electrónicos durante el proceso de suscripción y, en su lugar, son los consumidores los que tienen que comprobar la dirección de correo electrónico más tarde. Para ello, Azure AD B2C puede configurarse para deshabilitar la comprobación de correo electrónico. Si se hace esto, se crea un proceso de inicio de sesión más eficaz y ofrece a los desarrolladores la flexibilidad para diferenciar aquellos consumidores que han comprobado su dirección de correo electrónico de los que no lo ha hecho.

> [!WARNING]
> Deshabilitar la comprobación de correos electrónicos en el proceso de registro puede provocar que se reciban correos no deseados. Si deshabilita la comprobación de correo electrónico predeterminada que proporciona Azure AD B2C, es recomendable implementar un sistema de comprobación de reemplazo.
