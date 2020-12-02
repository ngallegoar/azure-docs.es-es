---
title: Aumento de la resistencia en la autenticación de usuarios externos con Azure Active Directory
description: Una guía para que los administradores y arquitectos de TI creen una autenticación resistente para los usuarios externos
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae8e24341c321fbfffb84d9b072abc4cf925aff3
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919231"
---
# <a name="build-resilience-in-external-user-authentication"></a>Aumento de la resistencia en la autenticación de usuarios externos

La [colaboración de Azure Active Directory B2B](https://docs.microsoft.com/azure/active-directory/external-identities/what-is-b2b) (Azure AD B2B) es una característica de [External Identities](https://docs.microsoft.com/azure/active-directory/external-identities/delegate-invitations) que permite la colaboración con otras organizaciones y usuarios. Esta característica hace posible la incorporación segura de usuarios invitados al inquilino de Azure AD sin tener que administrar sus credenciales. Los usuarios externos llevan con ellos la identidad y las credenciales de un proveedor de identidades externo (IdP), por lo que no tienen que recordar nuevas credenciales. 

## <a name="ways-to-authenticate-external-users"></a>Formas de autenticar a los usuarios externos

Puede elegir los métodos de autenticación de usuarios externos en el directorio. Puede usar proveedores de identidades de Microsoft u otros.

Con cada proveedor de identidades externo, se acepta una dependencia de la disponibilidad de dicho proveedor. Con algunos métodos de conexión a los proveedores de identidades, hay cosas que puede hacer para aumentar la resistencia.

> [!NOTE] 
> Azure AD B2B incorpora la posibilidad de autenticar a cualquier usuario de cualquier inquilino de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory) o con una [cuenta Microsoft](https://account.microsoft.com/account) personal. Con estas opciones integradas, no es necesario realizar ninguna configuración.

### <a name="considerations-for-resilience-with-other-idps"></a>Consideraciones para la resistencia con otros proveedores de identidades

Cuando se usen proveedores de identidades externos para la autenticación de usuarios invitados, es necesario asegurarse de que se mantienen determinadas configuraciones para evitar interrupciones.

| Método de autenticación| Consideraciones sobre la resistencia |
| - | - |
| Federación con proveedores de identidades sociales, como [Facebook](https://docs.microsoft.com/azure/active-directory/external-identities/facebook-federation) o [Google](https://docs.microsoft.com/azure/active-directory/external-identities/google-federation).| Debe mantener su cuenta con el proveedor de identidades y configurar el identificador y el secreto de cliente. |
| [Federación directa con proveedores de identidades de SAML y WS-Federation](https://docs.microsoft.com/azure/active-directory/external-identities/direct-federation)| Debe colaborar con el propietario del proveedor de identidades para acceder a sus puntos de conexión, de los que depende. <br>Debe mantener los metadatos que contienen los certificados y los puntos de conexión. |
| [Código de acceso de un solo uso de correo electrónico](https://docs.microsoft.com/azure/active-directory/external-identities/one-time-passcode)| Con este método, depende del sistema de correo electrónico de Microsoft y del sistema de correo electrónico y el cliente de correo electrónico del usuario. |


 

## <a name="self-service-sign-up-preview"></a>Registro de autoservicio (versión preliminar)

Como alternativa al envío de invitaciones o vínculos, puede habilitar el [registro de autoservicio](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-overview).  Esta funcionalidad permite que los usuarios externos soliciten acceso a una aplicación. Debe crear un [conector de API](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-api-connector) y asociarlo a un flujo de usuario. Puede asociar flujos de usuario que definan la experiencia del usuario con una o más aplicaciones. 

Es posible usar [conectores de API](https://docs.microsoft.com/azure/active-directory/external-identities/api-connectors-overview) para integrar el flujo de usuario de registro de autoservicio con las API de los sistemas externos. Esta integración de API se puede usar con los [flujos de trabajo de aprobación personalizados](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-approvals) para [realizar comprobaciones de identidad](https://docs.microsoft.com/azure/active-directory/external-identities/code-samples-self-service-sign-up), y para otras tareas, como sobrescribir los atributos de usuario. El uso de API requiere la administración de las dependencias siguientes.

* **Autenticación del conector de API**: para configurar un conector se requiere una dirección URL de punto de conexión, un nombre de usuario y una contraseña. Configure un proceso por el que se mantengan estas credenciales y trabaje con el propietario de la API para confirmar que sabe cuándo está programada la expiración.

* **Respuesta del conector de API**: diseñe conectores de API en el flujo de registro para que no funcionen si la API no está disponible. Examine y proporcione a los desarrolladores de la API estas [respuestas de ejemplo de API](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-api-connector) y los [procedimientos recomendados para la solución de problemas](https://docs.microsoft.com/azure/active-directory/external-identities/self-service-sign-up-add-api-connector). Trabaje con el equipo de desarrollo de API para probar todos los escenarios de respuesta posibles, como continuación, error de validación y bloqueo. 

## <a name="next-steps"></a>Pasos siguientes
Recursos de resistencia para administradores y arquitectos
 
* [Aumento de la resistencia con la administración de credenciales](resilience-in-credentials.md)

* [Aumento de la resistencia con los estados de dispositivos](resilience-with-device-states.md)

* [Aumento de la resistencia mediante Evaluación continua de acceso (CAE)](resilience-with-continuous-access-evaluation.md)

* [Aumento de la resistencia en la autenticación híbrida](resilience-in-hybrid.md)

* [Aumento de la resistencia en el acceso a las aplicaciones con Application Proxy](resilience-on-premises-access.md)

Recursos de resistencia para desarrolladores

* [Aumento de la resistencia de IAM en las aplicaciones](resilience-app-development-overview.md)

* [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
 
