---
title: 'Licencia para el autoservicio de restablecimiento de contraseña: Azure Active Directory'
description: Requisitos de concesión de licencias del autoservicio de restablecimiento de contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 598f3bd8500a59cd41cc4126915e6cccbd4fb2f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848568"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Licensing requirements for Azure AD self-service password reset (Requisitos de concesión de licencias del autoservicio de restablecimiento de contraseña de Azure AD)

Azure Active Directory (Azure AD) está disponible en varias ediciones: Gratis, Premium P1 y Premium P2. Hay distintas características que componen el autoservicio de restablecimiento de contraseña, entre las que se incluyen el cambio, el restablecimiento, el desbloqueo y la escritura diferida, que se encuentran disponibles en las diferentes ediciones de Azure AD. En este artículo se intenta explicar las diferencias. Encontrará más detalles acerca de las características incluidas en cada edición de Azure AD en la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Comparación de ediciones y características

El autoservicio de restablecimiento de contraseña de Azure AD se ofrece con licencia por usuario; para garantizar el cumplimiento se requiere que las organizaciones asignen la correspondiente licencia a sus usuarios.

* Cambio de contraseña de autoservicio para usuarios en la nube
   * Soy un **usuario solo de nube** y conozco mi contraseña.
      * Me gustaría **cambiarla** por otra.
   * Esta funcionalidad se incluye en todas las ediciones de Azure AD.

* Restablecimiento de contraseña de autoservicio para usuarios en la nube
   * Soy un **usuario solo de nube** y he olvidado mi contraseña.
      * Me gustaría **cambiarla** por otra que conozca.
   * Esta funcionalidad se incluye en las ediciones Premium P1 o Premium P2 de Azure AD, Microsoft 365 Empresa u Office 365.

* Autoservicio de restablecimiento/cambio/desbloqueo **con escritura diferida local**
   * Soy un **usuario de implementación híbrida**, mi cuenta de usuario de Active Directory local se sincroniza con mi cuenta de Azure AD con Azure AD Connect. Me gustaría cambiar la contraseña, la he olvidado o se ha bloqueado.
      * Me gustaría cambiar mi contraseña o restablecerla a otra que conozca, o desbloquear mi cuenta, **y** que ese cambio se sincronice con Active Directory local.
   * Esta funcionalidad se incluye en las ediciones Premium P1 o Premium P2 de Azure AD, o Microsoft 365 Empresa.

> [!WARNING]
> Los planes de licencias de Office 365 independientes *no admiten "Self-Service Password Reset/Change/Unlock with on-premises writeback"* (Autoservicio de restablecimiento/modificación/desbloqueo de contraseñas con escritura en diferido local) y requieren un plan que incluya la versión Premium P1 o P2 de Azure AD, o Microsoft 365 Empresa para que sirva la funcionalidad.
>

En las páginas siguientes, encontrará información adicional sobre licencias, incluidos los costos:

* [Sitio sobre precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Características y funcionalidades de Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)
* [Descripción del servicio Microsoft 365 Empresa](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Habilitar licencias basadas en grupos o usuarios

Ahora, Azure AD admite licencias basadas en grupos. Los administradores pueden asignar licencias en bloque a un grupo de usuarios en lugar de asignarlas una a una. Para más información, consulte [Asignar, comprobar y resolver los problemas con licencias](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Algunos servicios de Microsoft no están disponibles en todas las ubicaciones. Antes de poder asignar una licencia a un usuario, el administrador tiene que especificar la propiedad **Ubicación de uso** en el usuario. La asignación de licencias puede hacerse en la sección **Usuario** > **Perfil** > **Configuración** de Azure Portal. *Cuando se utiliza la asignación de licencias de grupo, los usuarios sin ubicación de uso especificada heredan la ubicación del directorio.*

## <a name="next-steps"></a>Pasos siguientes

* [¿Cómo se realiza un lanzamiento correcto de SSPR?](howto-sspr-deployment.md)
* [Restablecimiento o modificación de la contraseña](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registro para el autoservicio de restablecimiento de contraseñas](../user-help/active-directory-passwords-reset-register.md)
* [¿Qué datos usa SSPR y cuáles se deben rellenar en lugar de los usuarios?](howto-sspr-authenticationdata.md)
* [¿Qué métodos de autenticación están disponibles para los usuarios?](concept-sspr-howitworks.md#authentication-methods)
* [¿Cuáles son las opciones de directiva con SSPR?](concept-sspr-policy.md)
* [¿Qué es la escritura diferida de contraseñas y por qué nos interesa?](howto-sspr-writeback.md)
* [¿Cómo se informa sobre la actividad de SSPR?](howto-sspr-reporting.md)
* [¿Cuáles son todas las opciones en SSPR y qué significan?](concept-sspr-howitworks.md)
* [Creo que algo se ha roto. ¿Cómo se solucionan problemas en SSPR?](active-directory-passwords-troubleshoot.md)
* [Tengo una pregunta que no se ha comentado en ningún otro sitio](active-directory-passwords-faq.md)
