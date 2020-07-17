---
title: 'Introducción al registro combinado: Azure Active Directory'
description: Habilite el registro de autoservicio de restablecimiento de contraseña y Multi-Factor Authentication de Azure AD combinados
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7feb69b2ea53794b780a983ed8ab4ba5874ac022
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85260855"
---
# <a name="enable-combined-security-information-registration-in-azure-active-directory"></a>Habilitación del registro de información de seguridad combinado en Azure Active Directory

Antes del registro combinado, los usuarios se registraban a los métodos de autenticación para Azure Multi-factor Authentication y el autoservicio de restablecimiento de contraseña (SSPR) por separado. La gente estaba confundida por el hecho de que se usaban métodos parecidos para Azure Multi-Factor Authentication y SSPR pero, aún así, se tenían que registrar en las dos características. Ahora, con el registro combinado, los usuarios pueden registrarse una vez y obtener las ventajas de Azure Multi-Factor Authentication y SSPR.

Antes de habilitar la nueva experiencia, revise el artículo [Registro de información de seguridad combinado](concept-registration-mfa-sspr-combined.md) para asegurarse de que entiende la funcionalidad y los efectos de esta característica.

![Experiencia mejorada del registro de información de seguridad combinado](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

## <a name="enable-combined-registration"></a>Habilitar registro combinado

Siga estos pasos para habilitar el registro combinado:

1. Inicie sesión en Azure Portal como administrador de usuarios o administrador global.
2. Vaya a **Azure Active Directory** > **Configuración de usuario** > **Administrar la configuración de la versión preliminar de características del usuario**.
3. En **Los usuarios pueden utilizar la experiencia combinada de registro de información de seguridad**, elija habilitarla para un grupo de usuarios **Seleccionado** o para **Todos** los usuarios.

   ![Habilitación de la experiencia de información de seguridad combinada para los usuarios](media/howto-registration-mfa-sspr-combined/enable-the-combined-security-info.png)

> [!NOTE]
> Después de habilitar el registro combinado, los usuarios que registren o confirmen su número de teléfono o aplicación móvil a través de la nueva experiencia pueden usarlos para Azure Multi-Factor Authentication y SSPR, si estos métodos están habilitados en las directivas de Azure Multi-Factor Authentication y SSPR. Si luego deshabilita esta experiencia, los usuarios que vayan a la página de registro de SSPR anterior en `https://aka.ms/ssprsetup` deberán realizar la autenticación multifactor para poder acceder a la página.

Si ha configurado el sitio para la lista de asignación de zonas en Internet Explorer, los sitios siguientes deben estar en la misma zona:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Directivas de acceso condicional para el registro combinado

Proteger cuándo y cómo se registran los usuarios para Azure Multi-Factor Authentication y el restablecimiento de contraseñas de autoservicio ya es posible con las acciones del usuario en la directiva de acceso condicional. Esta característica está disponible para organizaciones que han habilitado la [característica de registro combinado](../authentication/concept-registration-mfa-sspr-combined.md). Esta funcionalidad se puede habilitar en las organizaciones que quieren que los usuarios se registren para Azure Multi-factor Authentication y SSPR desde una ubicación central, como una ubicación de red de confianza durante la incorporación de recursos humanos.

> [!NOTE]
> Esta directiva solo se aplica cuando un usuario tiene acceso a una página de registro combinada. Esta directiva no exige que el usuario se inscriba a MFA cuando tiene acceso a otras aplicaciones. Puede crear una directiva de registro de MFA mediante [Azure Identity Protection: configuración de la directiva de MFA](../identity-protection/howto-identity-protection-configure-mfa-policy.md).

Para obtener más información sobre la creación de ubicaciones de confianza en el acceso condicional, consulte el artículo [What is the location condition in Azure Active Directory Conditional Access?](../conditional-access/location-condition.md#named-locations) (¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Crear una directiva para requerir el registro desde una ubicación de confianza

La siguiente directiva se aplica a todos los usuarios seleccionados que intentan registrarse con la experiencia de registro combinado, y bloquea su acceso a menos que se conecten desde una ubicación marcada como red de confianza.

1. En **Azure Portal**, vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **+ Nueva directiva**.
1. Escriba el nombre de esta directiva, por ejemplo, *Registro de información de seguridad combinado en redes de confianza*.
1. En **Asignaciones**, seleccione **Usuarios y grupos**. Elija los usuarios y grupos a los que desea aplicar esta directiva y, a continuación, seleccione **Listo**.

   > [!WARNING]
   > Los usuarios deben estar habilitados para el registro combinado.

1. En **Aplicaciones o acciones en la nube**, seleccione **Acciones del usuario**. Active **Registro de la información de seguridad** y, a continuación, seleccione **Listo**.

    ![Creación de una directiva de acceso condicional para controlar el registro de la información de seguridad](media/howto-registration-mfa-sspr-combined/require-registration-from-trusted-location.png)

1. En **Condiciones** > **Ubicaciones**, configure las opciones siguientes:
   1. Configure **Sí**.
   1. Incluya **Cualquier ubicación**.
   1. Excluya **Todas las ubicaciones de confianza**.
1. Seleccione **Listo** en la ventana *Ubicaciones* y, a continuación, seleccione **Listo** en la ventana *Condiciones*.
1. En **Controles de acceso** > **Conceder**, elija **Bloquear acceso** y, después, **Seleccionar**.
1. Establezca **Habilitar directiva** en **Activado**.
1. Para finalizar la directiva, seleccione **Crear**.

## <a name="next-steps"></a>Pasos siguientes

Si necesita ayuda, consulte [Solución de problemas de registro de información de seguridad combinado](howto-registration-mfa-sspr-combined-troubleshoot.md) u obtenga información sobre [¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?](../conditional-access/location-condition.md)

Para habilitar las características en el inquilino de Azure AD, consulte los tutoriales para [habilitar el autoservicio de restablecimiento de contraseña](tutorial-enable-sspr.md) y [habilitar Azure Multi-Factor Authentication](tutorial-enable-azure-mfa.md).

Obtenga información sobre cómo [forzar a los usuarios a que vuelvan a registrar los métodos de autenticación](howto-mfa-userdevicesettings.md#manage-user-authentication-options).

Además, puede revisar los [métodos disponibles para Multi-Factor Authentication y SSPR](concept-authentication-methods.md).
