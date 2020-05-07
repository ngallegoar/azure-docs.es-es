---
title: 'Controles de concesión en una directiva de acceso condicional: Azure Active Directory'
description: Qué son los controles de concesión en una directiva de acceso condicional de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01c625bebbcd2e619a8125fdfb92673cd02966b2
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583197"
---
# <a name="conditional-access-grant"></a>Acceso condicional: Conceder

Dentro de una directiva de acceso condicional, un administrador puede hacer uso de los controles de acceso para conceder o bloquear el acceso a los recursos.

![Directiva de acceso condicional con un control de concesión que requiere autenticación multifactor](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Bloquear acceso

El bloqueo tiene en cuenta las asignaciones y evita el acceso en función de la configuración de la directiva de acceso condicional.

El bloqueo es un control eficaz y se debe manejar con el conocimiento adecuado. Es algo que los administradores deben utilizar en el [modo de solo informe](concept-conditional-access-report-only.md) para probar antes de habilitar.

## <a name="grant-access"></a>Conceder acceso

Los administradores pueden optar por aplicar uno o varios controles al conceder acceso. Entre estos controles se incluyen las siguientes opciones: 

- [Requerir la autenticación multifactor (Azure Multi-Factor Authentication)](../authentication/concept-mfa-howitworks.md)
- [Requerir que el dispositivo esté marcado como compatible (Microsoft Intune)](/intune/protect/device-compliance-get-started)
- [Requerir un dispositivo unido a Azure AD híbrido](../devices/concept-azure-ad-join-hybrid.md)
- [Requerir aplicación cliente aprobada](app-based-conditional-access.md)
- [Requerir la directiva de protección de aplicaciones](app-protection-based-conditional-access.md)

Cuando los administradores eligen combinar estas opciones, pueden elegir los siguientes métodos:

- Requerir todos los controles seleccionados (control **Y** control)
- Requerir uno de los controles seleccionados (control **O** control)

De forma predeterminada, el acceso condicional requiere todos los controles seleccionados.

### <a name="require-multi-factor-authentication"></a>Requerir autenticación multifactor

Al seleccionar esta casilla, los usuarios deberán ejecutar Azure Multi-Factor Authentication. Se puede encontrar más información acerca de la implementación de Azure Multi-Factor Authentication en el artículo [Planeación de una implementación de Azure Multi-Factor Authentication basada en la nube](../authentication/howto-mfa-getstarted.md).

### <a name="require-device-to-be-marked-as-compliant"></a>Requerir que el dispositivo esté marcado como compatible

Las organizaciones que han implementado Microsoft Intune pueden usar la información que devuelven sus dispositivos para identificar aquellos que satisfacen los requisitos de cumplimiento específicos. Esta información del cumplimiento de las directivas se reenvía de Intune a Azure AD donde se pueden tomar decisiones de acceso condicional para conceder o bloquear el acceso a los recursos. Para más información sobre las directivas de cumplimiento, consulte el artículo [Establecimiento de reglas en los dispositivos para permitir el acceso a recursos de su organización con Intune](/intune/protect/device-compliance-get-started).

Un dispositivo se puede marcar como compatible con Intune (para cualquier sistema operativo del dispositivo) o por el sistema MDM de terceros para dispositivos Windows 10. Jamf Pro es el único sistema MDM de terceros compatible. Puede encontrar más información sobre la integración en el artículo [Integración de Jamf Pro con Intune para cumplimiento](/intune/protect/conditional-access-integrate-jamf).

Los dispositivos deben estar registrados en Azure AD para poder marcarlos como compatibles. Puede encontrar más información sobre el registro del dispositivo en el artículo [¿Qué es una identidad de dispositivo?](../devices/overview.md)

### <a name="require-hybrid-azure-ad-joined-device"></a>Requerir un dispositivo unido a Azure AD híbrido

Las organizaciones pueden optar por usar la identidad del dispositivo como parte de la directiva de acceso condicional. Las organizaciones pueden requerir que los dispositivos estén unidos a Azure AD híbrido utilizando esta casilla. Para más información sobre las identidades del dispositivo, consulte el artículo [¿Qué es una identidad de dispositivo?](../devices/overview.md)

### <a name="require-approved-client-app"></a>Requerir aplicación cliente aprobada

Las organizaciones pueden requerir que los intentos de acceso a las aplicaciones en la nube seleccionadas se tengan que realizar desde una aplicación cliente aprobada. Estas aplicaciones cliente aprobadas son compatibles con las [directivas de protección de aplicaciones de Intune](/intune/app-protection-policy) independientemente de cualquier otra solución de administración de dispositivos móviles (MDM).

Con el fin de aprovechar este control de concesión, el acceso condicional requiere que el dispositivo esté registrado en Azure Active Directory, lo que requiere el uso de una aplicación de agente. La aplicación de agente puede ser Microsoft Authenticator para iOS o el portal de empresa de Microsoft para dispositivos Android. Si no hay ninguna aplicación de agente instalada en el dispositivo cuando el usuario intenta autenticarse, será redirigido a la tienda de aplicaciones para que instale la aplicación de agente.

Esta configuración se aplica a las siguientes aplicaciones de iOS y Android:

- Microsoft Azure Information Protection
- Microsoft Bookings
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Flow
- Explorador administrado de Microsoft Intune
- Microsoft Invoicing
- Microsoft Kaizala
- Microsoft Launcher
- Microsoft Office
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerApps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Empresarial
- Microsoft StaffHub
- Microsoft Stream
- Equipos de Microsoft
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer
- Microsoft Whiteboard

**Comentarios:**

- Las aplicaciones cliente aprobadas admiten la característica de administración de aplicaciones móviles de Intune.
- Requisito de la opción **Solicitar aplicación cliente aprobada**:
   - Solo admite iOS y Android como condición de plataformas de dispositivo.
   - Se requiere una aplicación de agente para registrar el dispositivo. En iOS, la aplicación de agente es Microsoft Authenticator y, en Android, es la aplicación Portal de empresa de Intune.
- El acceso condicional no puede considerar Microsoft Edge en modo InPrivate como aplicación cliente aprobada.

Consulte el artículo [Uso obligatorio de aplicaciones cliente aprobadas para el acceso a aplicaciones en la nube mediante el acceso condicional](app-based-conditional-access.md) para obtener ejemplos de configuración.

### <a name="require-app-protection-policy"></a>Requerir la directiva de protección de aplicaciones

En la directiva de acceso condicional, puede requerir que una [directiva de protección de aplicaciones de Intune](/intune/app-protection-policy) esté presente en la aplicación cliente antes de que el acceso esté disponible para las aplicaciones en la nube seleccionadas. 

Con el fin de aprovechar este control de concesión, el acceso condicional requiere que el dispositivo esté registrado en Azure Active Directory, lo que requiere el uso de una aplicación de agente. La aplicación de agente puede ser Microsoft Authenticator para iOS o el portal de empresa de Microsoft para dispositivos Android. Si no hay ninguna aplicación de agente instalada en el dispositivo cuando el usuario intenta autenticarse, será redirigido a la tienda de aplicaciones para que instale la aplicación de agente.

Esta configuración se aplica a las aplicaciones cliente siguientes:

- Microsoft Cortana
- Microsoft OneDrive
- Microsoft Outlook
- Microsoft Planner

**Comentarios:**

- Las aplicaciones para la directiva de protección de aplicaciones admiten la característica de administración de aplicaciones móviles de Intune con la protección de la directiva.
- Requisitos para la **Exigencia de la directiva de protección de aplicaciones**:
    - Solo admite iOS y Android como condición de plataformas de dispositivo.
    - Se requiere una aplicación de agente para registrar el dispositivo. En iOS, la aplicación de agente es Microsoft Authenticator y, en Android, es la aplicación Portal de empresa de Intune.

Consulte el artículo [Uso obligatorio de directivas de protección de aplicaciones y una aplicación cliente aprobada para el acceso a aplicaciones en la nube con acceso condicional](app-protection-based-conditional-access.md) para obtener ejemplos de configuración.

### <a name="terms-of-use"></a>Términos de uso

Si su organización ha redactado términos de uso, es posible que aparezcan opciones adicionales debajo de Conceder controles. Estas opciones permiten a los administradores exigir la aceptación de los términos de uso como una condición para acceder a los recursos protegidos por la directiva. Puede encontrar más información sobre los términos de uso en el artículo [Términos de uso de Azure Active Directory](terms-of-use.md).

## <a name="next-steps"></a>Pasos siguientes

- [Acceso condicional: Controles de sesión](concept-conditional-access-session.md)

- [Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

- [Modo de solo informe](concept-conditional-access-report-only.md)
