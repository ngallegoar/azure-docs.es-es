---
title: 'Almacenamiento de datos de identidad para clientes europeos: Azure AD'
description: Obtenga información sobre dónde almacena Azure Active Directory los datos relacionados con la identidad de los clientes Europaos.
services: active-directory
author: ajburnle
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/15/2020
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06900483598e1c03bd56cd8ac8c905f885012f08
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2020
ms.locfileid: "91395896"
---
# <a name="identity-data-storage-for-european-customers-in-azure-active-directory"></a>Almacenamiento de datos de identidad para los clientes Europaos en Azure Active Directory
Azure AD almacena los datos de identidad en una ubicación geográfica en función de la dirección que proporcione la organización al suscribirse a un servicio de Microsoft Online, como Microsoft 365 y Azure. Para obtener información sobre dónde se almacenan los datos de identidad, puede consultar la sección [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Dónde se encuentran sus datos) de Microsoft Trust Center.

Para los clientes que proporcionaron una dirección en Europa, Azure AD conserva la mayoría de los datos de identidad en centros de datos europeos. En este documento se proporciona información sobre los datos que los servicios de Azure AD almacenan fuera de Europa.

## <a name="microsoft-azure-multi-factor-authentication"></a>Microsoft Azure Multi-Factor Authentication

Para Azure Multi-Factor Authentication basado en la nube, la autenticación se completa en el centro de datos más cercano al usuario. Azure Multi-Factor Authentication tiene centros de datos en Estados Unidos, Europa y Asia Pacífico.

* La autenticación multifactor que usa llamadas telefónicas procede de centros de datos de EE. UU. y se enruta mediante proveedores globales.
* Los proveedores globales enrutan la autenticación multifactor mediante SMS.
* Las solicitudes de autenticación multifactor que usan las notificaciones de inserción de la aplicación Microsoft Authenticator que se originan en los centros de datos de Europa se procesan en los centros datos de Europa.
    * Los servicios específicos del fabricante del dispositivo, como las notificaciones de inserción de Apple, pueden estar fuera de Europa.
* Las solicitudes de autenticación multifactor que usan códigos OATH que se originan en los centros de datos de Europa se validan en Europa.

Para más información sobre qué información de usuario recopila el Servidor Microsoft Azure Multi-Factor Authentication (Servidor MFA) y Azure MFA basada en la nube, consulte [Recopilación de datos de usuario de Azure Multi-Factor Authentication](../authentication/howto-mfa-reporting-datacollection.md).

## <a name="password-based-single-sign-on-for-enterprise-applications"></a>Inicio de sesión único basado en contraseña para aplicaciones empresariales
 
Si un cliente crea una nueva aplicación empresarial (ya sea a través de la galería de Azure AD o fuera de la galería) y habilita el inicio de sesión único basado en contraseña, la dirección URL de inicio de sesión de la aplicación y los campos de inicio de sesión de captura personalizados se almacenan en Estados Unidos. Para obtener más información acerca de esta característica, consulte [Configuración del inicio de sesión único basado en contraseña.](../manage-apps/configure-password-single-sign-on-non-gallery-applications.md)

## <a name="microsoft-azure-active-directory-b2c-azure-ad-b2c"></a>Microsoft Azure Active Directory B2C (Azure AD B2C)

Los datos de configuración de directivas y contenedores de claves de Azure AD B2C se almacenan en los centros de datos de EE. UU. Estos no contienen datos personales de los usuarios. Para obtener más información sobre las configuraciones de directivas, consulte el artículo [Azure Active Directory B2C: directivas integradas](../../active-directory-b2c/user-flow-overview.md).

## <a name="microsoft-azure-active-directory-b2b-azure-ad-b2b"></a>Microsoft Azure Active Directory B2B (Azure AD B2B) 
    
Azure AD B2B almacena las invitaciones con vínculos de canje y la información sobre la dirección URL de redirección en centros de datos de EE. UU. Además, la dirección de correo electrónico de los usuarios que cancelan la suscripción a invitaciones B2B también se almacena en centros de datos de EE. UU.

## <a name="microsoft-azure-active-directory-domain-services-azure-ad-ds"></a>Microsoft Azure Active Directory Domain Services (Azure AD DS)

Azure AD DS almacena los datos de usuario en la misma ubicación que la instancia de Azure Virtual Network que seleccionó el cliente. Por lo tanto, si la red está fuera de Europa, los datos se replican y almacenan fuera de Europa.

## <a name="federation-in-microsoft-exchange-server-2013"></a>Federación en Microsoft Exchange Server 2013
    
- Identificador de la aplicación (AppID): un número único que genera el sistema de autenticación de Azure Active Directory para identificar las organizaciones de Exchange.
- Lista de dominios federados aprobados para la aplicación
- Clave pública de firma de tokens de la aplicación 

Para más información acerca de la federación en Microsoft Exchange Server, consulte el artículo [Federación: Ayuda de Exchange 2013](/exchange/federation-exchange-2013-help).


## <a name="other-considerations"></a>Otras consideraciones

Los servicios y las aplicaciones que se integran en Azure AD tienen acceso a datos de identidad. Evalúe cada servicio y aplicación que use para determinar la manera en que cada servicio y aplicación específico procesa los datos de identidad, y si cumple los requisitos de almacenamiento de datos de la empresa.

Para obtener más información sobre la residencia de los datos de los servicios de Microsoft, consulte la sección [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Dónde se encuentran sus datos) de Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre cualquiera de las características y funcionalidades que se han descrito anteriormente, consulte estos artículos:
- [¿Qué es Multi-Factor Authentication?](../authentication/concept-mfa-howitworks.md)

- [Autoservicio de restablecimiento de contraseña de Azure AD](../authentication/concept-sspr-howitworks.md)

- [¿Qué es Azure Active Directory B2C?](../../active-directory-b2c/overview.md)

- [¿Qué es la colaboración B2B de Azure AD?](../external-identities/what-is-b2b.md)

- [Azure Active Directory (AD) Domain Services](../../active-directory-domain-services/overview.md)