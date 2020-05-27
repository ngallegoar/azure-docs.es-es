---
title: ¿Qué es la colaboración B2B en Azure Active Directory?
description: La colaboración de Azure Active Directory B2B admite el acceso de usuarios invitados, para poder compartir recursos de forma segura y colaborar con asociados externos.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 05/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0effb6bb6e3d3f80775a3cee1ad8a63013a78805
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585845"
---
# <a name="what-is-guest-user-access-in-azure-active-directory-b2b"></a>¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?

La colaboración de negocio a negocio (B2B) de Azure AD permite compartir de forma segura las aplicaciones y los servicios de la empresa con usuarios invitados de cualquier otra organización, mientras conserva el control sobre sus propios datos corporativos. Trabaje de forma segura con asociados externos, grandes o pequeños, incluso si no tienen Azure AD o un departamento de TI. Un proceso de invitación y canje sencillo permite a los asociados usar sus propias credenciales para acceder a los recursos de su empresa. Los desarrolladores pueden usar las API de negocio a negocio de Azure AD para personalizar el proceso de invitación o escribir aplicaciones como los portales de suscripción de autoservicio.

Vea el vídeo para saber cómo puede colaborar de forma segura con usuarios invitados invitándolos a iniciar sesión en las aplicaciones y los servicios de la empresa con el uso de sus propias identidades.

El vídeo siguiente proporciona una introducción práctica.

>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

   > [!IMPORTANT]
   > **A partir del 31 de marzo de 2021**, Microsoft dejará de admitir el canje de invitaciones mediante la creación de cuentas de Azure AD no administradas e inquilinos para escenarios de colaboración B2B. Como preparación, se recomienda a los clientes que opten por la [autenticación de código de acceso de un solo uso por correo electrónico](one-time-passcode.md). Agradecemos sus comentarios sobre esta característica en vista previa pública. Nos alegra poder crear más formas de colaborar.

## <a name="collaborate-with-any-partner-using-their-identities"></a>Colaboración con cualquier asociado mediante sus identidades

Con Azure AD B2B, el asociado utiliza su propia solución de administración de identidades, así que no hay ninguna sobrecarga administrativa externa para su organización.

- El socio utiliza sus propias identidades y credenciales; Azure AD no es necesario.
- No es necesario administrar las cuentas externas o las contraseñas.
- No es necesario sincronizar las cuentas o administrar los ciclos de vida de las cuentas.  

![Captura de pantalla que muestra la página Agregar miembros](media/what-is-b2b/add-member.png)

## <a name="invite-guest-users-with-a-simple-invitation-and-redemption-process"></a>Invitación a usuarios con un proceso de canje e invitación sencillo

Los usuarios invitados inician sesión en las aplicaciones y los servicios con sus propias identidades profesionales, educativas o sociales. Si el usuario invitado no tiene una cuenta de Microsoft o de Azure AD, se crea una en su nombre al canjear la invitación. 

- Invite a usuarios invitados mediante la identidad de correo electrónico de su elección.
- Envíe un vínculo directo a una aplicación o envíe una invitación al panel de acceso propio del usuario invitado.
- Los usuarios invitados siguen unos pasos de canje sencillos para iniciar sesión.

![Captura de pantalla que muestra la página Revisar permisos](media/what-is-b2b/consentscreen.png)

## <a name="use-policies-to-securely-share-your-apps-and-services"></a>Uso de directivas para compartir de forma segura sus aplicaciones y servicios

Puede usar las directivas de autorización para proteger el contenido corporativo. Las directivas de acceso condicional, como la autenticación multifactor, se pueden aplicar:

- En el nivel de inquilino.
- En el nivel de aplicación.
- A usuarios invitados específicos para proteger los datos y las aplicaciones empresariales.

![Captura de pantalla que muestra la opción Acceso condicional](media/what-is-b2b/tutorial-mfa-policy-2.png)


## <a name="easily-add-guest-users-in-the-azure-ad-portal"></a>Incorporación sencilla de usuarios invitados en el portal de Azure AD

Como administrador, puede agregar fácilmente usuarios invitados a su organización en Azure Portal.

- Cree un usuario invitado en Azure AD, similar a cómo se agrega un nuevo usuario.
- El usuario invitado recibe inmediatamente una invitación personalizable que le permite iniciar sesión en su panel de acceso.
- Los usuarios invitados del directorio pueden asignarse a grupos o aplicaciones.  

![Captura de pantalla que muestra la página de entrada de invitación Nuevo usuario invitado](media/what-is-b2b/add-a-b2b-user-to-azure-portal.png)

## <a name="let-application-and-group-owners-manage-their-own-guest-users"></a>Permitir que los propietarios de aplicaciones y grupos administren sus propios usuarios invitados

Puede delegar la administración de usuarios invitados a los propietarios de aplicaciones para que puedan agregar usuarios invitados directamente a cualquier aplicación que deseen compartir, ya sea una aplicación de Microsoft o no.

- Los administradores configuran la administración de aplicaciones y grupos de autoservicio.
- Los usuarios no administradores usan su [panel de acceso](https://myapps.microsoft.com) para agregar usuarios invitados a aplicaciones o grupos.

![Captura de pantalla que muestra el Panel de acceso de los usuarios invitados](media/what-is-b2b/access-panel-manage-app.png)

## <a name="customize-the-onboarding-experience-for-b2b-guest-users"></a>Personalización de la experiencia de incorporación de usuarios invitados de B2B

Incorpore a los asociados externos de manera personalizada según las necesidades de su organización.

- Use la [administración de derechos de Azure AD](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) para configurar directivas que [administren el acceso para los usuarios externos](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users#how-access-works-for-external-users).
- Use la [API de invitación de colaboración B2B](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) para personalizar las experiencias de incorporación.

## <a name="integrate-with-identity-providers"></a>Integración con proveedores de identidades

Azure AD admite proveedores de identidades externos (como Facebook), cuentas de Microsoft, Google o proveedores de identidades empresariales. Puede configurar la federación con proveedores de identidades para que los usuarios externos puedan iniciar sesión con sus cuentas empresariales o sociales existentes en lugar de crear una nueva cuenta solo para la aplicación. Obtenga más información sobre los proveedores de identidades para External Identities.

![Captura de pantalla que muestra la página Proveedores de identidades](media/what-is-b2b/identity-providers.png)


## <a name="create-a-self-service-sign-up-user-flow-preview"></a>Creación de un flujo de usuario de registro de autoservicio (versión preliminar)

Con un flujo de usuario de registro de autoservicio, puede crear una experiencia de registro para los usuarios externos que quieran tener acceso a sus aplicaciones. Como parte del flujo de registro, puede proporcionar opciones para diferentes proveedores de identidades sociales o empresariales y recopilar información sobre el usuario. Obtenga información sobre el registro de autoservicio y cómo configurarlo.

![Captura de pantalla que muestra la página Flujos de usuario](media/what-is-b2b/self-service-sign-up-user-flow-overview.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía sobre concesión de licencias de colaboración de Azure AD B2B](licensing-guidance.md)
- [Agregar usuarios invitados de colaboración B2B en el portal](add-users-administrator.md)
- [Información sobre el proceso de canje de invitaciones](redemption-experience.md)
- Y, como siempre, conéctese con el equipo del producto para obtener comentarios, conversaciones y sugerencias a través de nuestra [Comunidad tecnológica de Microsoft](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
