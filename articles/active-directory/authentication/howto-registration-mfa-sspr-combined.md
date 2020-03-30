---
title: 'Introducción al registro combinado: Azure Active Directory'
description: Habilite el registro de autoservicio de restablecimiento de contraseña y Multi-Factor Authentication de Azure AD (versión preliminar) combinados
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25c64d9e959b1d68de23e83e26d3495bd3939986
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75425190"
---
# <a name="enable-combined-security-information-registration-preview"></a>Habilitar el registro de información de seguridad combinado (vista preliminar)

Antes de habilitar la nueva experiencia, revise el artículo [Registro de información de seguridad combinado (vista preliminar)](concept-registration-mfa-sspr-combined.md) para asegurarse de que entiende la funcionalidad y los efectos de esta característica.

![Experiencia mejorada del registro de información de seguridad combinado](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| El registro de información de seguridad combinado para el autoservicio de restablecimiento de contraseña de Azure Active Directory (Azure AD) y Azure Multi-Factor Authentication es una característica en vista previa pública de Azure AD. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!NOTE]
> Las organizaciones que habilitaron la versión preliminar anterior para registrar y administrar la información de seguridad deben completar los pasos siguientes para habilitar la versión preliminar mejorada. En las organizaciones que no realicen el cambio, el 8 de octubre de 2019, Microsoft cambiará a los usuarios de la versión preliminar anterior para registrar y administrar la información de seguridad en la mejorada. 
> 
> Si no ha habilitado ninguna versión preliminar, la organización no se verá afectada.

## <a name="enable-combined-registration"></a>Habilitar registro combinado

Siga estos pasos para habilitar el registro combinado:

1. Inicie sesión en Azure Portal como administrador de usuarios o administrador global.
2. Vaya a **Azure Active Directory** > **Configuración de usuario** > **Administrar la configuración de la versión preliminar de características del usuario**.
3. En **Los usuarios pueden utilizar las características en versión preliminar para registrar y administrar la información de seguridad**, puede elegir habilitarlas para un grupo de usuarios **Seleccionado** o para **Todos** los usuarios.

   ![Habilitar la experiencia de versión preliminar de información de seguridad combinada para todos los usuarios](media/howto-registration-mfa-sspr-combined/enable-the-combined-security-info-preview.png)

> [!IMPORTANT]
> A partir de marzo de 2019, las opciones de llamada de teléfono no estarán disponibles para los usuarios de Multi-Factor Authentication y SSPR en inquilinos de Azure AD gratis o de evaluación. Los mensajes SMS no se ven afectados por este cambio. Las opciones de llamada de teléfono seguirán disponibles para los usuarios de inquilinos de Azure AD de pago.

> [!NOTE]
> Después de habilitar el registro combinado, los usuarios que registren o confirmen su número de teléfono o aplicación móvil a través de la nueva experiencia pueden usarlos para Multi-Factor Authentication y SSPR, si estos métodos están habilitados en las directivas de Multi-Factor Authentication y SSPR. Si luego deshabilita esta experiencia, los usuarios que vayan a la página de registro de SSPR anterior en `https://aka.ms/ssprsetup` deberán realizar la autenticación multifactor para poder acceder a la página.

Si ha configurado el sitio para la lista de asignación de zonas en Internet Explorer, los sitios siguientes deben estar en la misma zona:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Directivas de acceso condicional para el registro combinado

Proteger cuándo y cómo se registran los usuarios para Azure Multi-Factor Authentication y el restablecimiento de contraseñas de autoservicio ya es posible con las acciones del usuario en la directiva de acceso condicional. Esta característica de versión preliminar está disponible para organizaciones que han habilitado la [vista previa del registro combinado](../authentication/concept-registration-mfa-sspr-combined.md). Esta funcionalidad se puede habilitar en las organizaciones que quieren que los usuarios se registren para Azure Multi-factor Authentication y SSPR desde una ubicación central, como una ubicación de red de confianza durante la incorporación de recursos humanos. Para obtener más información sobre la creación de ubicaciones de confianza en el acceso condicional, consulte el artículo [What is the location condition in Azure Active Directory Conditional Access?](../conditional-access/location-condition.md#named-locations) (¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Crear una directiva para requerir el registro desde una ubicación de confianza

La siguiente directiva se aplica a todos los usuarios seleccionados, que intentan registrarse con la experiencia de registro combinado, y bloquea su acceso a menos que se conecten desde una ubicación marcada como red de confianza.

![Crear una directiva de acceso condicional para controlar el registro de información de seguridad](media/howto-registration-mfa-sspr-combined/require-registration-from-trusted-location.png)

1. En **Azure Portal**, vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**
1. En Nombre, escriba un nombre para la directiva. Por ejemplo, **Registro de información de seguridad combinada en redes de confianza**
1. En **Asignaciones**, haga clic en **Usuarios y grupos** y seleccione los usuarios y grupos a los que quiera aplicar esta directiva

   > [!WARNING]
   > Los usuarios deben estar habilitados para la [versión preliminar del registro combinado](../authentication/howto-registration-mfa-sspr-combined.md).

1. En **Aplicaciones en la nube o acciones**, seleccione **Acciones del usuario** y marque **Registrar la información de seguridad (versión preliminar)**
1. En **Condiciones** > **Ubicaciones**
   1. Configure **Sí**
   1. Incluya **Cualquier ubicación**
   1. Excluya **Todas las ubicaciones de confianza**
   1. Haga clic en **Hecho** en la hoja de ubicaciones
   1. Haga clic en **Listo** en la hoja Condiciones
1. En **Controles de acceso** > **Conceder**
   1. Haga clic en **Bloquear acceso**
   1. Después, haga clic en **Seleccionar**
1. Establezca **Habilitar directiva** en **Activado**
1. A continuación, haga clic en **Crear**

## <a name="next-steps"></a>Pasos siguientes

[Forzar a los usuarios a que vuelvan a registrar los métodos de autenticación](howto-mfa-userdevicesettings.md#manage-authentication-methods)

[Métodos disponibles para Multi-Factor Authentication y SSPR](concept-authentication-methods.md)

[Configuración del autoservicio de restablecimiento de contraseña](howto-sspr-deployment.md)

[Configuración de Azure Multi-Factor Authentication](howto-mfa-getstarted.md)

[Solución de problemas de registro de información de seguridad combinado](howto-registration-mfa-sspr-combined-troubleshoot.md)

[¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?](../conditional-access/location-condition.md)
