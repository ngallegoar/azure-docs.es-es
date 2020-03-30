---
title: Solución de problemas de LDAP seguro en Azure AD Domain Services | Microsoft Docs
description: Aprenda a solucionar los problemas de LDAP seguro (LDAPS) para un dominio administrado de Azure Active Directory Domain Services
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: 22d1b6e2344256b52cfdbc48720a680a770a4216
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132169"
---
# <a name="troubleshoot-secure-ldap-connectivity-issues-to-an-azure-active-directory-domain-services-managed-domain"></a>Solución de problemas de conectividad con LDAP seguro en un dominio administrado de Azure Active Directory Domain Services

Las aplicaciones y los servicios que usan el Protocolo ligero de acceso a directorios (LDAP) para comunicarse con Azure Active Directory Domain Services (Azure AD DS) se pueden [configurar para usar LDAP seguro](tutorial-configure-ldaps.md). Para que LDAP seguro funcione correctamente, deben estar abiertos un certificado adecuado y los puertos de red necesarios.

Este artículo le ayuda a solucionar problemas de acceso LDAP seguro en Azure AD DS.

## <a name="common-connection-issues"></a>Problemas de conexión comunes

Si tiene problemas para conectarse a un dominio administrado de Azure AD DS mediante LDAP seguro, revise los pasos siguientes de solución de problemas. Después de cada paso, intente conectarse de nuevo al dominio administrado de Azure AD DS:

* La cadena del emisor del certificado LDAP seguro debe ser de confianza en el cliente. Puede agregar la entidad de certificación raíz (CA) en el almacén de certificados raíz de confianza en el cliente para establecer la confianza.
    * Asegúrese de [exportar el certificado a las máquinas cliente y aplicarlo][client-cert].
* Compruebe que el certificado LDAP seguro para el dominio administrado tiene el nombre DNS en el atributo *Sujeto* o *Nombres alternativos del sujeto*.
    * Revise los [requisitos de certificado LDAP seguro][certs-prereqs] y cree un certificado de reemplazo si es necesario.
* Compruebe que el cliente de LDAP (por ejemplo, *ldp.exe*) se conecta al punto de conexión de LDAP seguro con un nombre DNS, no con la dirección IP.
    * El certificado que se aplica al dominio administrado de Azure AD DS no incluye las direcciones IP del servicio, sino solo los nombres DNS.
* Compruebe el nombre DNS al que se conecta el cliente LDAP. Debe resolver a la dirección IP pública para LDAP seguro en el dominio administrado de Azure AD DS.
    * Si el nombre DNS se resuelve en la dirección IP interna, actualice el registro DNS para que se resuelva en la dirección IP externa.
* Para la conectividad externa, el grupo de seguridad de red debe incluir una regla que permita el tráfico al puerto TCP 636 desde Internet.
    * Si puede conectarse al dominio administrado de Azure AD DS mediante LDAP seguro desde los recursos conectados directamente a la red virtual, pero no a las conexiones externas, asegúrese de [crear una regla de grupo de seguridad de red que permita el tráfico LDAP seguro][ldaps-nsg].

## <a name="next-steps"></a>Pasos siguientes

Si los problemas persisten, [abra una solicitud de soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-ldaps]: tutorial-configure-ldaps.md
[certs-prereqs]: tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap
[client-cert]: tutorial-configure-ldaps.md#export-a-certificate-for-client-computers
[ldaps-nsg]: tutorial-configure-ldaps.md#lock-down-secure-ldap-access-over-the-internet
