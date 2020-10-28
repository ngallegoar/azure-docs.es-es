---
title: Autenticación RADIUS con Azure Active Directory
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
ms.openlocfilehash: ff6210741d87602b4f695633b11d2641a6bb6781
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114040"
---
# <a name="radius-authentication-with-azure-active-directory"></a>Autenticación RADIUS con Azure Active Directory

El Servicio de autenticación remota telefónica de usuario (RADIUS) es un protocolo de red que protege una red al habilitar la autenticación centralizada y la autorización de los usuarios de acceso telefónico. Muchas aplicaciones todavía dependen del protocolo RADIUS para autenticar a los usuarios.

Microsoft Windows Server tiene un rol denominado Servidor de directivas de redes (NPS), que puede actuar como servidor RADIUS y admitir la autenticación RADIUS.

Azure Active Directory (Azure AD) permite la autenticación multifactor con sistemas basados en RADIUS. Si un cliente quiere aplicar Azure Multi-Factor Authentication a cualquiera de las cargas de trabajo de RADIUS mencionadas anteriormente, puede instalar la extensión NPS de Azure Multi-Factor Authentication en su servidor NPS de Windows. 

El servidor NPS de Windows autentica las credenciales de un usuario frente a Active Directory y, a continuación, envía la solicitud de Multi-Factor Authentication a Azure. A continuación, el usuario recibe un desafío en su autenticador móvil. Una vez que se resuelve correctamente, se permite que la aplicación cliente se conecte al servicio. 

## <a name="use-when"></a>Úsala en estos casos: 

Debe agregar Multi-Factor Authentication a aplicaciones como:
* Una red privada virtual (VPN).
* Acceso Wi-Fi.
* Puerta de enlace de Escritorio remoto (RDG)
* Infraestructura de escritorio virtual (VDI)
* Cualquier otra que dependa del protocolo RADIUS para autenticar a los usuarios en el servicio. 

> [!NOTE]
> En lugar de depender de RADIUS y la extensión NPS de Azure Multi-Factor Authentication para aplicar Azure Multi-Factor Authentication a las cargas de trabajo de VPN, se recomienda actualizar la VPN a SAML y federar directamente la VPN con Azure AD. Esto proporciona a la VPN el amplio espectro de protección de Azure AD, como el acceso condicional, Multi-Factor Authentication, el cumplimiento de los dispositivos y Identity Protection.

![Diagrama de arquitectura](./media/authentication-patterns/radius-auth.png)


## <a name="components-of-the-system"></a>Componentes del sistema 

* **Aplicación cliente (cliente VPN)** : Envía la solicitud de autenticación al cliente RADIUS.

* **Cliente RADIUS** : Convierte las solicitudes de la aplicación cliente y las envía al servidor RADIUS que tiene instalada la extensión NPS.

* **Servidor RADIUS** : Se conecta con Active Directory para realizar la autenticación principal de la solicitud RADIUS. Cuando se completa correctamente, pasa la solicitud a la extensión NPS de Azure Multi-Factor Authentication.

* **Extensión NPS** : Desencadena una solicitud para Azure Multi-Factor Authentication para la autenticación secundaria. Si se realiza correctamente, la extensión NPS completa la solicitud de autenticación al entregar al servidor RADIUS los tokens de seguridad que incluyen la notificación de Multi-Factor Authentication, emitida por el servicio de token de seguridad de Azure.

* **Azure Multi-Factor Authentication** : Se comunica con Azure AD para recuperar los detalles del usuario y realiza la autenticación secundaria con un método de comprobación configurado para el usuario.

## <a name="implement-radius-with-azure-ad"></a>Implementación de RADIUS con Azure AD 

* [Entrega de funcionalidades de Azure Multi-Factor Authentication mediante NPS](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension) 

* [Configuración de la extensión NPS de Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension-advanced) 

* [VPN con Azure Multi-Factor Authentication mediante la extensión NPS](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension-vpn) 

  
‎ 

 
