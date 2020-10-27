---
title: Autenticación de LDAP con Azure Active Directory
description: Guía arquitectónica para lograr este patrón de autenticación
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a70cb4754d98f4573670860c510692a7a2d134c
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114025"
---
# <a name="ldap-authentication-with-azure-active-directory"></a>Autenticación de LDAP con Azure Active Directory

El protocolo ligero de acceso a directorios (LDAP) es un protocolo de aplicación para trabajar con varios servicios de directorio. Los servicios de directorio, como Active Directory, [almacenan información de usuarios y cuentas](https://www.dnsstuff.com/active-directory-service-accounts), e información de seguridad, como contraseñas. Luego, el servicio permite que la información se comparta con otros dispositivos de la red. Las aplicaciones empresariales, como el correo electrónico, los administradores de relaciones con el cliente (CRM) y el software de recursos humanos (RR. HH.), pueden usar LDAP para autenticación, acceso y búsqueda de información. 

Azure Active Directory (Azure AD) admite este patrón a través de Azure AD Domain Services (AD DS). Permite que las organizaciones que adoptan una estrategia de primero en la nube modernicen su entorno al mover sus recursos LDAP locales a la nube. Las ventajas inmediatas serán: 

* La integración en Azure AD. Las adiciones de usuarios y grupos, o los cambios de atributo en sus objetos, se sincronizan automáticamente desde el inquilino de Azure AD en AD DS. Los cambios en los objetos de Active Directory en el entorno local se sincronizan con Azure AD y, a continuación, AD DS.

* Se simplifican las operaciones. Se reduce la necesidad de mantener y revisar manualmente las infraestructuras locales. 

* Confiable. Obtiene servicios administrados de alta disponibilidad. 

## <a name="use-when"></a>Cuándo se utiliza

Existe la necesidad de que una aplicación o un servicio usen la autenticación LDAP.

![Diagrama de la arquitectura](./media/authentication-patterns/ldap-auth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario** : Accede a las aplicaciones dependientes de LDAP a través de un explorador.

* **Explorador web** : Interfaz con la que el usuario interactúa para acceder a la dirección URL externa de la aplicación.

* **Red virtual** : Red privada en Azure a través de la cual la aplicación heredada puede consumir servicios de LDAP. 

* **Aplicaciones heredadas** : Aplicaciones o cargas de trabajo de servidor que requieren LDAP implementado en una red virtual de Azure, o que tienen visibilidad para IP de instancia de AD DS a través de rutas de red. 

* **Azure AD** : Sincroniza la información de identidad del directorio local de la organización a través de Azure AD Connect.

* **Azure AD Domain Services (AD DS)** : Realiza una sincronización unidireccional desde Azure AD para proporcionar acceso a un conjunto central de usuarios, grupos y credenciales. La instancia de AD DS se asigna a una red virtual. Las aplicaciones, los servicios y las VM de Azure que se conectan a esta red virtual asignada a AD DS pueden usar las características de AD DS comunes, como LDAP, la unión a un dominio, la directiva de grupo, la autenticación NTLM y Kerberos.
   > [!NOTE]
   >  En entornos en los que la organización no puede sincronizar los hashes de contraseña, o donde los usuarios inician sesión mediante tarjetas inteligentes, se recomienda usar un bosque de recursos en AD DS. 

* **Azure AD Connect** : Herramienta para la sincronización de la información de identidad local con Microsoft Azure AD. El asistente para la implementación y las experiencias guiadas le ayudan a configurar los requisitos previos y los componentes necesarios para la conexión, incluida la sincronización y el inicio de sesión de Active Directory a Azure AD. 

* **Active Directory** : Servicio de directorio que almacena [información de identidades locales, como información de usuarios y cuentas](https://www.dnsstuff.com/active-directory-service-accounts), e información de seguridad, como contraseñas.

## <a name="implement-ldap-authentication-with-azure-ad"></a>Implementación de la autenticación LDAP con Azure AD

* [Creación y configuración de una instancia de Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance) 

* [Configuración de redes virtuales para una instancia de Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-configure-networking) 

* [Configuración de LDAP seguro para un dominio administrado de Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-configure-ldaps) 

* [Creación de una confianza de bosque de salida a un dominio local en Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-forest-trust)

 
