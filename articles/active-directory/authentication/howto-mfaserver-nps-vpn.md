---
title: 'Servidor Azure MFA y VPN de terceros: Azure Active Directory'
description: Guías paso a paso para la configuración de Servidor Azure MFA para integrarse con Cisco, Citrix y Juniper.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdb24bcd79f1766a52f290fd6fe0e6e5bf17e7c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74847956"
---
# <a name="advanced-scenarios-with-azure-mfa-server-and-third-party-vpn-solutions"></a>Escenarios avanzados con Servidor Azure MFA y soluciones de VPN de terceros

Servidor Microsoft Azure Multi-Factor Authentication (Servidor Azure MFA) se puede usar para conectar fácilmente con una variedad de soluciones VPN de terceros. Este artículo se centra la aplicación VPN de Cisco® ASA, la aplicación VPN SSL de NetScaler de Citrix y la aplicación VPN SSL de Juniper Networks Secure Access o Pulse Secure Connect Secure. Se han elaborado guías de configuración para abordar estas tres aplicaciones comunes. También se puede integrar Servidor Microsoft Azure Multi-Factor Authentication con la mayoría de los demás sistemas que utilizan RADIUS, LDAP, IIS o autenticación basada en notificaciones en AD FS. Puede encontrar más detalles en las [configuraciones de Servidor Azure MFA](howto-mfaserver-deploy.md#next-steps).

> [!IMPORTANT]
> A partir del 1 de julio de 2019, Microsoft ya no ofrecerá el Servidor MFA para implementaciones nuevas. Los clientes nuevos que quieran exigir la autenticación multifactor a sus usuarios deberán usar Azure Multi-Factor Authentication basado en la nube. Los clientes existentes que hayan activado el Servidor MFA antes del 1 de julio podrán descargar la versión más reciente y las actualizaciones futuras, así como generar credenciales de activación como de costumbre.

## <a name="cisco-asa-vpn-appliance-and-azure-mfa-server"></a>Aplicación VPN de Cisco ASA y Servidor Azure MFA
Servidor Azure MFA se integra con su aplicación VPN de Cisco® ASA para ofrecer más seguridad para el acceso al portal y los inicios de sesión VPN de Cisco AnyConnect®.  Se puede usar el protocolo LDAP o RADIUS.  Seleccione una de las siguientes opciones para descargar las guías de configuración paso a paso detalladas.

| Guía de configuración | Descripción |
| --- | --- |
| [Configuración de Cisco ASA con Anyconnect VPN y Azure MFA para LDAP](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Integración de la aplicación VPN de Cisco ASA con Azure MFA mediante LDAP |
| [Configuración de Cisco ASA con Anyconnect VPN y Azure MFA para RADIUS](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Integración perfecta de la aplicación VPN de Cisco ASA con Azure MFA mediante RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-mfa-server"></a>VPN SSL de Citrix NetScaler y Servidor Azure MFA
Servidor Azure MFA se integra con su aplicación VPN SSL de Citrix NetScaler para ofrecer más seguridad para el acceso al portal y los inicios de sesión VPN SSL de Cisco NetScaler.  Se puede usar el protocolo LDAP o RADIUS.  Seleccione una de las siguientes opciones para descargar las guías de configuración paso a paso detalladas.

| Guía de configuración | Descripción |
| --- | --- |
| [Configuración de VPN SSL de Citrix NetScaler y Azure MFA para LDAP](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Integración de su VPN SSL de Citrix NetScaler con la aplicación de Azure MFA mediante LDAP |
| [Configuración de VPN SSL de Citrix NetScaler y Azure MFA para RADIUS](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Integración de su aplicación VPN SSL de Citrix NetScaler con Azure MFA mediante RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-mfa-server"></a>Aplicación VPN SSL de Juniper/Pulse Secure y Servidor Azure MFA
Server Azure MFA se integra con su aplicación VPN SSL de Juniper/Pulse Secure para ofrecer más seguridad para el acceso al portal y los inicios de sesión VPN SSL de Juniper/Pulse Secure.  Se puede usar el protocolo LDAP o RADIUS.  Seleccione una de las siguientes opciones para descargar las guías de configuración paso a paso detalladas.

| Guía de configuración | Descripción |
| --- | --- |
| [Configuración de VPN SSL de Juniper/Pulse Secure y Azure MFA para LDAP](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Integración de su VPN SSL de Juniper/Pulse Secure con la aplicación de Azure MFA mediante LDAP |
| [Configuración de VPN SSL de Juniper/Pulse Secure y Azure MFA para RADIUS](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Integración de su VPN SSL de Juniper/Pulse Secure con Azure MFA mediante RADIUS |

## <a name="next-steps"></a>Pasos siguientes

- [Aumento de la infraestructura de autenticación existente con la extensión de NPS para Azure Multi-Factor Authentication](howto-mfa-nps-extension.md)

- [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
