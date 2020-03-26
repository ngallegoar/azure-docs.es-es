---
title: Comparación de la colaboración B2B y B2C en Azure Active Directory | Microsoft Docs
description: ¿Cuál es la diferencia entre la colaboración B2B de Azure Active Directory y Azure AD B2C?
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 07/22/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e7f5aa324db869e30a8b2fe214416129baca8d9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "68380736"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Comparación de la colaboración B2B y B2C de Azure Active Directory

La colaboración B2B de Azure Active Directory (Azure AD) y Azure AD B2C permiten trabajar con usuarios externos en Azure AD. Pero, ¿cómo se comparan?

**Azure AD B2B** está pensado para empresas que desean compartir archivos y recursos de forma segura con usuarios externos para que estos puedan colaborar. Un administrador de Azure configura B2B en Azure Portal y Azure AD se encarga de la federación entre la empresa y su asociado externo. Los usuarios inician sesión en los recursos compartidos con un proceso sencillo de invitación y canje con su cuenta profesional o educativa o con cualquier cuenta de correo electrónico.
 
**Azure AD B2C** está indicado principalmente para empresas y desarrolladores que crean aplicaciones orientadas al cliente. Con Azure AD B2C, los desarrolladores pueden usar Azure AD como el sistema de identidad completo para su aplicación, permitiendo a los clientes que inicien sesión con una identidad que ya han establecido (como Facebook o Gmail).

En la siguiente tabla se ofrece una comparación detallada.


Funcionalidades de la colaboración B2B |     Oferta independiente de Azure AD B2C
-------- | --------
Destinado a: organizaciones que desean poder autenticar a los usuarios desde una organización asociada, independientemente del proveedor de identidades. | Destinado a: invitar a los clientes de sus aplicaciones web y para dispositivos móviles, independientemente de que sean personas, instituciones u organizaciones, a Azure AD.
Identidades que se admiten: empleados con cuentas profesionales o educativas, asociados con cuentas profesionales o educativas, o cualquier dirección de correo electrónico. Pronto se admitirá la federación directa.  | Identidades que se admiten: usuarios consumidores con cuentas de aplicación local (cualquier dirección de correo electrónico o nombre de usuario) o cualquier identidad social compatible con federación directa.
Los usuarios externos se administran en el mismo directorio que los empleados, pero tienen una anotación especial. Pueden administrarse del mismo modo que los empleados, pueden agregarse a los mismos grupos, etc.  | Los usuarios externos se administran en el directorio de la aplicación. Se administran de manera independiente del directorio de asociados y de empleados de la organización (si existe).
Se admite el inicio de sesión único (SSO) en todas las aplicaciones conectadas a Azure AD. Por ejemplo, puede proporcionar acceso a Office 365, o bien a aplicaciones locales y a otras aplicaciones SaaS como Salesforce o Workday.  |  Se admite el inicio de sesión único para aplicaciones propiedad de los clientes dentro de los inquilinos de Azure AD B2C. No se admite el inicio de sesión único en Office 365 ni en otras aplicaciones SaaS de Microsoft.
Ciclo de vida de asociado: lo administra la organización anfitriona o que realiza la invitación.  | Ciclo de vida de cliente: mediante autoservicio o administrado por la aplicación.
Directiva de seguridad y cumplimiento: los administra la organización anfitriona o que realiza la invitación (por ejemplo, con [directivas de acceso condicional](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Directiva de seguridad y cumplimiento: las administra la aplicación.
Personalización de marca: se utiliza la marca de la organización anfitriona o que realiza la invitación.  |    Personalización de marca: la administra la aplicación. Normalmente suele tratarse de productos con marca, y la organización pasa a un segundo plano.
Más información: [Entrada de blog](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Documentación](what-is-b2b.md)  | Más información: [página de producto](https://azure.microsoft.com/services/active-directory-b2c/), [documentación](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Pasos siguientes

- [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
- [Propiedades de usuario de la colaboración B2B](user-properties.md)

