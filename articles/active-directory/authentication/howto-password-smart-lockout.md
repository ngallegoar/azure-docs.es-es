---
title: 'Prevención de ataques mediante el bloqueo inteligente: Azure Active Directory'
description: El bloqueo inteligente de Azure Active Directory ayuda a proteger las organizaciones frente a los ataques por fuerza bruta que intentan adivinar contraseñas.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 61ae942ed189dc4245a9a0b282daf4cad5323536
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652580"
---
# <a name="azure-active-directory-smart-lockout"></a>Bloqueo inteligente de Azure Active Directory

El bloqueo inteligente ayuda a bloquear a los actores malintencionados que intentan adivinar las contraseñas de los usuarios o que usan métodos de fuerza bruta para obtenerlas. Puede reconocer los inicios de sesión que proceden de usuarios válidos y tratarlos de forma distinta a los que provienen de atacantes y otros orígenes desconocidos. El bloqueo inteligente puede impedir el paso a los atacantes y permitir al mismo tiempo que los usuarios continúen el acceso a sus cuentas y sean productivos.

De forma predeterminada, el bloqueo inteligente impide los intentos de inicio de sesión en la cuenta durante un minuto, después de realizar diez intentos incorrectos. La cuenta se bloquea de nuevo después de cada intento de inicio de sesión incorrecto, durante un minuto en el primero y más tiempo en los intentos posteriores.

El bloqueo inteligente realiza un seguimiento de los últimos tres códigos hash de contraseña incorrecta para evitar que aumente el contador de bloqueo con la misma contraseña. Si alguien escribe la misma contraseña incorrecta varias veces, este comportamiento no hará que la cuenta se bloquee.

 > [!NOTE]
 > La funcionalidad de seguimiento de hash no está disponible para los clientes con la autenticación de paso a través habilitada, ya que la autenticación se produce en el entorno local y no en la nube.

Las implementaciones federadas que usan AD FS 2016 y AF FS 2019 pueden habilitar ventajas similares mediante el [bloqueo de extranet de AD FS y el bloqueo inteligente de extranet](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection).

El bloqueo inteligente está siempre activado para todos los clientes de Azure AD con la configuración predeterminada que ofrece la combinación correcta de seguridad y facilidad de uso. Para personalizar la configuración del bloqueo inteligente con valores específicos de su organización, los usuarios necesitan licencias de Azure AD de pago.

El uso del bloqueo inteligente no garantiza que nunca se bloqueará un usuario original. Cuando el bloqueo inteligente bloquea una cuenta de usuario, hacemos todo lo posible para no bloquear al usuario original. El servicio de bloqueo intenta garantizar que los actores no válidos no puedan obtener acceso a una cuenta de usuarios genuina.  

* Cada centro de datos de Azure Active Directory realiza un seguimiento del bloqueo de forma independiente. Un usuario tendrá cierto número de intentos (threshold_limit * datacenter_count), si el usuario visita cada centro de datos.
* El bloqueo inteligente compara los datos de una ubicación desconocida con los de una conocida para diferenciar entre un actor no válido y el usuario original. Las ubicaciones conocidas y desconocidas tendrán contadores de bloqueo independientes.

El bloqueo inteligente puede integrarse con implementaciones híbridas mediante la autenticación de hash de contraseña o la autenticación de paso a través, para impedir que los atacantes bloqueen las cuentas de Active Directory local. Mediante el establecimiento correcto de directivas de bloqueo inteligente en Azure AD, se pueden filtrar los atacantes antes de que lleguen al entorno local de Active Directory.

Cuando se usa la [autenticación de paso a través](../hybrid/how-to-connect-pta.md), debe asegurarse de lo siguiente:

* El umbral de bloqueo de Azure AD sea **inferior** al umbral de bloqueo de cuenta de Active Directory. Establezca los valores de modo que el umbral de bloqueo de cuenta de Active Directory sea al menos dos o tres veces mayor que el umbral de bloqueo de Azure AD. 
* La duración del bloqueo de Azure AD debe ser mayor que la del contador de restablecimiento del bloqueo de a cuenta de Azure Directory después de la duración. Tenga en cuenta que la duración de Azure AD está establecido en segundos, mientras que la duración de AD se establece en minutos. 

Por ejemplo, si quiere que el contador de Azure AD sea mayor que AD, el contador de Azure AD sería de 120 segundos (2 minutos), mientras que el contador de AD local está establecido en 1 minuto (60 segundos).

> [!IMPORTANT]
> Actualmente, un administrador no puede desbloquear cuentas en la nube de los usuarios si estos han sido bloqueados por la capacidad de bloqueo inteligente. El administrador deberá esperar a que expire la duración del bloqueo. Pero el usuario puede desbloquear mediante el autoservicio de restablecimiento de contraseña (SSPR) desde un dispositivo o una ubicación de confianza.

## <a name="verify-on-premises-account-lockout-policy"></a>Comprobación de la directiva de bloqueo de cuenta local

Siga estas instrucciones para comprobar la directiva de bloqueo de cuentas de Active Directory local:

1. Abra las herramientas de administración de directivas de grupo.
2. Edite el grupo de directivas que incluye la directiva de bloqueo de cuentas de su organización, por ejemplo, la **directiva de dominio predeterminada**.
3. Vaya a **Configuración del equipo** > **Directivas** > **Configuración de Windows** > **Configuración de seguridad** > **Directivas de cuenta** > **Directiva de bloqueo de cuenta**.
4. Compruebe los valores de **Umbral de bloqueo de cuenta** y **Restablecer contador de bloqueo de cuenta tras**.

![Modificación de la directiva de bloqueo de cuentas de Active Directory local](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Administración de los valores de bloqueo inteligente de Azure AD

En función de los requisitos de su organización, puede ser necesario personalizar los valores de bloqueo inteligente. Para personalizar la configuración del bloqueo inteligente con valores específicos de su organización, los usuarios necesitan licencias de Azure AD de pago.

Para comprobar o modificar los valores de bloqueo inteligente para su organización, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque y seleccione *Azure Active Directory*. Seleccione **Seguridad** > **Métodos de autenticación** > **Protección con contraseña**.
1. Establezca la opción **Umbral de bloqueo** en función del número de inicios de sesión con error permitidos en una cuenta antes de su primer bloqueo. El valor predeterminado es 10.
1. En **Lockout duration in seconds** (Duración de bloqueo en segundos), establezca la longitud en segundos de cada bloqueo. El valor predeterminado es 60 segundos (un minuto).

> [!NOTE]
> Si el primer inicio de sesión después de un bloqueo también produce un error, la cuenta se bloquea de nuevo. Si una cuenta se bloquea de forma repetida, aumenta la duración del bloqueo.

![Personalización de la directiva de bloqueo inteligente de Azure AD en Azure Portal](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>Cómo determinar si la característica de bloqueo inteligente está funcionando o no

Cuando se desencadena el umbral de bloqueo inteligente, aparecerá el siguiente mensaje mientras la cuenta está bloqueada:

**Su cuenta se bloqueó temporalmente para impedir un uso no autorizado. Vuelva a intentarlo y, si sigue teniendo problemas, póngase en contacto con su administrador.**

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de la lista personalizada de contraseñas prohibidas mediante Azure AD](howto-password-ban-bad.md)
* [Configuración del autoservicio de restablecimiento de contraseña para permitir que los usuarios desbloqueen sus propias cuentas](quickstart-sspr.md)
