---
title: Solución de problemas de unión a un dominio con Azure AD Domain Services | Microsoft Docs
description: Aprenda a solucionar problemas comunes cuando intente unir una máquina virtual a un dominio o conectar una aplicación a Azure Active Directory Domain Services y no pueda conectar o autenticarse en el dominio administrado.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: iainfou
ms.openlocfilehash: f187dba4eace61695a72e4b7b08731e65ff0d7f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299115"
---
# <a name="troubleshoot-domain-join-problems-with-an-azure-ad-domain-services-managed-domain"></a>Solución de problemas de unión a un dominio administrado de Azure AD Domain Services

Cuando intente unir una máquina virtual (VM) o conectar una aplicación a un dominio administrado de Azure Active Directory Domain Services (AD DS), es posible que reciba un error en el que se le informará de que no puede hacerlo. Para solucionar los problemas de unión a un dominio, observe en cuál de los siguientes puntos se produce el problema:

* Si no recibe un mensaje de autenticación, la máquina virtual o la aplicación no podrá conectarse al dominio administrado de Azure AD DS.
    * Intente solucionar los [problemas de conectividad de unión a un dominio](#connectivity-issues-for-domain-join).
* Si recibe un error durante la autenticación, la conexión al dominio administrado de Azure AD DS se habrá realizado correctamente.
    * Intente solucionar los [problemas relacionados con las credenciales durante la unión a un dominio](#credentials-related-issues-during-domain-join).

## <a name="connectivity-issues-for-domain-join"></a>Problemas de conectividad de unión a un dominio

Si la máquina virtual no puede encontrar el dominio administrado de Azure AD DS, suele deberse a un problema con la configuración o la conexión de red. Consulte los siguientes procedimientos para resolver el problema:

1. Asegúrese de que la máquina virtual está conectada a la misma red virtual, o a una del mismo nivel, que la subred que está habilitada para Azure AD DS. De lo contrario, la máquina virtual no podrá encontrar el dominio al que debe unirse ni establecer conexión con él.
    * Si la máquina virtual no está conectada a la misma red virtual, asegúrese de que el emparejamiento de red virtual o la conexión VPN esté *activa* o *conectada* para que el tráfico pueda fluir correctamente.
1. Intente hacer ping al dominio utilizando el nombre del dominio administrado de Azure AD DS (por ejemplo, `ping aaddscontoso.com`).
    * Si hay algún error en la respuesta de ping, intente hacer ping a las direcciones IP del dominio que aparecen en la página de información general del portal del dominio administrado de Azure AD DS; por ejemplo, `ping 10.0.0.4`.
    * Si puede hacer ping correctamente a la dirección IP pero no al dominio, es posible que DNS no esté configurado correctamente. Asegúrese de que ha configurado los servidores DNS del dominio administrado de Azure AD DS de la red virtual.
1. Intente vaciar la memoria caché de resolución DNS de la máquina virtual; por ejemplo, `ipconfig /flushdns`.

### <a name="network-security-group-nsg-configuration"></a>Configuración de grupos de seguridad de red (NSG)

Cuando crea un dominio administrado de Azure AD DS, también se crea un grupo de seguridad de red con las reglas apropiadas para que las operaciones del dominio se realicen correctamente. Si edita o crea otras reglas del grupo de seguridad de red, es posible que bloquee por accidente los puertos necesarios para que Azure AD DS proporcione servicios de conexión y autenticación. Estas reglas del grupo de seguridad de red pueden provocar problemas, como que no se complete la sincronización de contraseñas, que los usuarios no puedan iniciar sesión o incidencias con la unión al dominio.

Si los problemas de conexión persisten, consulte estos procedimientos:

1. Compruebe el estado del dominio administrado de Azure AD DS en Azure Portal. Si tiene una alerta de *AADDS001*, hay un grupo de seguridad de red que está bloqueando el acceso.
1. Consulte las [reglas de los puertos y el grupo de seguridad de red][network-ports]. Compruebe que no se haya aplicado ninguna regla del grupo de seguridad de red a la máquina virtual o a la red virtual a la que intenta conectarse que bloquee los puertos de red.
1. Cuando se hayan resuelto los problemas de configuración del grupo de seguridad de red, la alerta *AADDS001* desaparecerá de la página de estado en un plazo de unas dos horas. Cuando la conectividad de red esté disponible, intente unir la máquina virtual al dominio de nuevo.

## <a name="credentials-related-issues-during-domain-join"></a>Problemas relacionados con las credenciales durante la unión a un dominio

Si aparece un cuadro de diálogo que le pide las credenciales para unirse al dominio administrado de Azure AD DS, la máquina virtual podrá conectarse al dominio utilizando la red virtual de Azure. El proceso de unión al dominio no puede realizar la autenticación en el dominio o la autorización no puede completarse con las credenciales proporcionadas.

Para solucionar los problemas relacionados con las credenciales, consulte los siguientes pasos:

1. Pruebe a usar el formato UPN para especificar las credenciales, por ejemplo `dee@aaddscontoso.onmicrosoft.com`. Asegúrese de que el UPN esté configurado correctamente en Azure AD.
    * El atributo *SAMAccountName* de su cuenta se puede generar automáticamente si hay varios usuarios con el mismo prefijo UPN en el inquilino o si el prefijo UPN es demasiado largo. Por lo tanto, el formato del atributo *SAMAccountName* de su cuenta puede que no sea el que espera o el que usa en su dominio local.
1. Pruebe a usar las credenciales de una cuenta de usuario que forma parte del dominio administrado de Azure AD DS para unir máquinas virtuales al dominio administrado.
1. Asegúrese de que la [sincronización de contraseñas esté habilitada][enable-password-sync] y espere el tiempo suficiente hasta que la sincronización inicial se complete.

## <a name="next-steps"></a>Pasos siguientes

Para entender mejor los procesos de Active Directory durante la operación de unión a un dominio, consulte [Problemas de unión y autenticación][join-authentication-issues].

Si sigue teniendo problemas para unir la máquina virtual al dominio administrado de Azure AD DS, [busque ayuda y abra una incidencia de soporte técnico sobre Azure Active Directory][azure-ad-support].

<!-- INTERNAL LINKS -->
[enable-password-sync]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

<!-- EXTERNAL LINKS -->
[join-authentication-issues]: /previous-versions/windows/it-pro/windows-2000-server/cc961817(v=technet.10)
