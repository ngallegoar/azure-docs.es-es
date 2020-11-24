---
title: Registro de la aplicación para utilizar Azure Active Directory | Microsoft Docs
description: Escrito para los profesionales de TI, este artículo ofrece instrucciones para integrar las aplicaciones de Azure con Active Directory.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: de16c947c59f5a0111b9325dbefe7daf1268fb40
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94649167"
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Desarrollo de aplicaciones de línea de negocio para Azure Active Directory
Esta guía proporciona información general sobre el desarrollo de aplicaciones de línea de negocio (LoB) para Azure Active Directory (AD) y se ha diseñado expresamente para administradores globales de Active Directory y Microsoft 365.

## <a name="overview"></a>Información general
La creación de aplicaciones integradas con Azure AD proporciona a los usuarios de la organización el inicio de sesión único con Microsoft 365. Si tiene la aplicación en Azure AD, podrá controlar la directiva de autenticación de dicha aplicación. Para más información sobre el acceso condicional y cómo proteger las aplicaciones con Multi-Factor Authentication (MFA), consulte [Configuración de las reglas de acceso](../authentication/tutorial-enable-azure-mfa.md).

Registro de la aplicación para utilizar Azure Active Directory Registrar la aplicación significa que los programadores pueden usar Azure AD para autenticar usuarios y solicitar acceso a recursos de usuarios; por ejemplo, correo electrónico, calendario y documentos.

Cualquier miembro del directorio (no invitados) puede registrar una aplicación, conocido también como *crear un objeto de aplicación*. Si no puede registrar una aplicación, significa que el administrador global del directorio tiene restringida esta funcionalidad y es posible que tenga que ponerse en contacto con él para [obtener los derechos adecuados](../roles/delegate-app-roles.md#assign-built-in-application-admin-roles) para poder registrar la aplicación. Para más información sobre cómo restringir el usuario, consulte [Delegación de permisos de registro de aplicaciones en Azure Active Directory](../roles/delegate-app-roles.md#restrict-who-can-create-applications).

El registro de una aplicación permite a cualquier usuario hacer lo siguiente:

* Obtener una identidad para su aplicación que Azure AD reconoce
* Obtener uno o más secretos o claves que la aplicación puede usar para autenticarse a sí misma en AD
* Identificar la aplicación, por ejemplo, con un nombre o logotipo personalizado en el Portal de Azure
* Aplicar las características de autorización de Azure AD en su aplicación, como las siguientes:

  * Control de acceso basado en rol (RBAC)
  * Azure Active Directory como servidor de autorización de OAuth (proteger una API expuesta por la aplicación)
* Indicar los permisos necesarios para que la aplicación funcione según lo previsto, como:

     - Permisos de la aplicación (solo administradores globales). Por ejemplo: pertenencia a roles en otra aplicación de Azure AD o respecto a una suscripción, un grupo de recursos o un recurso de Azure.
     - Permisos delegados (cualquier usuario) Por ejemplo: Azure AD, inicio de sesión y perfil de lectura

> [!NOTE]
> De forma predeterminada, cualquier miembro puede registrar una aplicación. Para obtener información sobre cómo restringir permisos de registro de aplicaciones a miembros específicos, consulte [Cómo se agregan aplicaciones a Azure AD](../develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

A continuación, se indica lo que el administrador global tendrá que hacer para ayudar a los desarrolladores a que su aplicación esté lista para la producción:

* Configurar las reglas de acceso (directiva de acceso/MFA)
* Configurar la aplicación para requerir asignación de usuarios y asignar usuarios
* Suprimir la experiencia de consentimiento del usuario predeterminada

## <a name="configure-access-rules"></a>Configuración de las reglas de acceso
Configure las reglas de acceso por aplicación a las aplicaciones SaaS. Por ejemplo, puede obligar a utilizar MFA o solo permitir el acceso a usuarios de redes de confianza. En el documento de [configuración de las reglas de acceso](../authentication/tutorial-enable-azure-mfa.md)se muestran los detalles pertinentes.

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Configurar la aplicación para requerir asignación de usuarios y asignar usuarios
De forma predeterminada, los usuarios pueden acceder a las aplicaciones sin asignación previa. Sin embargo, si la aplicación expone roles o si quiere que esta aparezca en la página Aplicaciones de un usuario, debe exigir la asignación de usuarios.

Si es un suscriptor de Azure AD Premium o Enterprise Mobility Suite (EMS), se recomienda encarecidamente utilizar grupos. La asignación de grupos a la aplicación permite delegar la administración del acceso continuo al propietario del grupo. Puede crear el grupo o pedir a la parte responsable de la organización que lo haga usando sus recursos de administración de grupos.

[Asignar usuarios y grupos a una aplicación](./assign-user-or-group-access-portal.md)  


## <a name="suppress-user-consent"></a>Supresión del consentimiento del usuario
De forma predeterminada, los usuarios otorgan su consentimiento para iniciar sesión. El consentimiento, que se solicita para conceder permisos a una aplicación, puede desconcertar a los usuarios que no estén familiarizados con la toma de este tipo de decisiones.

Para aplicaciones de confianza, puede otorgar el consentimiento para la aplicación en nombre de su organización con el fin de simplificar la experiencia de los usuarios.

Para obtener más información sobre el consentimiento de usuarios y cómo se utiliza esta característica en Azure, consulte [Integración de aplicaciones con Azure Active Directory](../develop/quickstart-register-app.md).

## <a name="related-articles"></a>Artículos relacionados
* [Provisión de acceso remoto seguro a aplicaciones locales](application-proxy.md)
* [Administración del acceso a las aplicaciones con Azure AD](what-is-access-management.md)