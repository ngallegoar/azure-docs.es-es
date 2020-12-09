---
title: Solución de problemas de unión a un dominio con Azure AD Domain Services | Microsoft Docs
description: Aprenda a solucionar problemas comunes cuando intente unir una máquina virtual a un dominio o conectar una aplicación a Azure Active Directory Domain Services y no pueda conectar o autenticarse en el dominio administrado.
services: active-directory-ds
author: justinha
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: justinha
ms.openlocfilehash: 86d6ca79a12e4706f558e92c3c83c5bddaa99b3c
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96618610"
---
# <a name="troubleshoot-domain-join-problems-with-an-azure-active-directory-domain-services-managed-domain"></a>Solucionar problemas de unión al dominio con un dominio administrado de Azure Active Directory Domain Services

Al intentar unir una máquina virtual (VM) o conectar una aplicación a un dominio administrado Azure Active Directory Domain Services (Azure AD DS), es posible que reciba un error que le informará que no puede hacerlo. Para solucionar los problemas de unión a un dominio, observe en cuál de los siguientes puntos se produce el problema:

* Si no recibe un mensaje de autenticación, la máquina virtual o la aplicación no podrá conectarse al dominio administrado de Azure AD DS.
    * Intente solucionar los [problemas de conectividad de unión a un dominio](#connectivity-issues-for-domain-join).
* Si recibe un error durante la autenticación, la conexión al dominio administrado se realiza correctamente.
    * Intente solucionar los [problemas relacionados con las credenciales durante la unión a un dominio](#credentials-related-issues-during-domain-join).

## <a name="connectivity-issues-for-domain-join"></a>Problemas de conectividad de unión a un dominio

Si la máquina virtual no puede encontrar el dominio administrado, normalmente hay una conexión de red o un problema de configuración. Consulte los siguientes procedimientos para resolver el problema:

1. Asegúrese de que la máquina virtual está conectada a la misma red virtual o a una emparejada, que el dominio administrado. De lo contrario, la máquina virtual no podrá encontrar el dominio al que debe unirse ni establecer conexión con él.
    * Si la máquina virtual no está conectada a la misma red virtual, asegúrese de que el emparejamiento de red virtual o la conexión VPN esté *activa* o *conectada* para que el tráfico pueda fluir correctamente.
1. Intente hacer ping en el dominio con el nombre de dominio del dominio administrado, como `ping aaddscontoso.com`.
    * Si se produce un error en la respuesta de ping, intente hacer ping a las direcciones IP del dominio que aparece en la página de información general del portal para el dominio administrado, como `ping 10.0.0.4`.
    * Si puede hacer ping correctamente a la dirección IP pero no al dominio, es posible que DNS no esté configurado correctamente. Asegúrese de que ha [configurado los servidores DNS del dominio administrado para la red virtual][configure-dns].
1. Intente vaciar la memoria caché de resolución DNS de la máquina virtual; por ejemplo, `ipconfig /flushdns`.

### <a name="network-security-group-nsg-configuration"></a>Configuración de grupos de seguridad de red (NSG)

Cuando se crea un dominio administrado, también se crea un grupo de seguridad de red con las reglas adecuadas para el funcionamiento correcto del dominio. Si edita o crea reglas de grupo de seguridad de red adicionales, puede bloquear involuntariamente los puertos necesarios para que Azure AD DS proporcione servicios de conexión y autenticación. Estas reglas del grupo de seguridad de red pueden provocar problemas, como que no se complete la sincronización de contraseñas, que los usuarios no puedan iniciar sesión o incidencias con la unión al dominio.

Si los problemas de conexión persisten, consulte estos procedimientos:

1. Compruebe el estado de mantenimiento del dominio administrado en el Azure Portal. Si tiene una alerta de *AADDS001*, hay un grupo de seguridad de red que está bloqueando el acceso.
1. Consulte las [reglas de los puertos y el grupo de seguridad de red][network-ports]. Compruebe que no se haya aplicado ninguna regla del grupo de seguridad de red a la máquina virtual o a la red virtual a la que intenta conectarse que bloquee los puertos de red.
1. Cuando se hayan resuelto los problemas de configuración del grupo de seguridad de red, la alerta *AADDS001* desaparecerá de la página de estado en un plazo de unas dos horas. Cuando la conectividad de red esté disponible, intente unir la máquina virtual al dominio de nuevo.

## <a name="credentials-related-issues-during-domain-join"></a>Problemas relacionados con las credenciales durante la unión a un dominio

Si aparece un cuadro de diálogo que solicita credenciales para unirse al dominio administrado, la máquina virtual puede conectarse al dominio con la red virtual de Azure. El proceso de unión al dominio no puede realizar la autenticación en el dominio o la autorización no puede completarse con las credenciales proporcionadas.

Para solucionar los problemas relacionados con las credenciales, consulte los siguientes pasos:

1. Pruebe a usar el formato UPN para especificar las credenciales, por ejemplo `dee@contoso.onmicrosoft.com`. Asegúrese de que el UPN esté configurado correctamente en Azure AD.
    * El atributo *SAMAccountName* de su cuenta se puede generar automáticamente si hay varios usuarios con el mismo prefijo UPN en el inquilino o si el prefijo UPN es demasiado largo. Por lo tanto, el formato del atributo *SAMAccountName* de su cuenta puede que no sea el que espera o el que usa en su dominio local.
1. Intente usar las credenciales para una cuenta de usuario que forme parte del dominio administrado para unir las máquinas virtuales al dominio administrado.
1. Asegúrese de que la [sincronización de contraseñas esté habilitada][enable-password-sync] y espere el tiempo suficiente hasta que la sincronización inicial se complete.

## <a name="next-steps"></a>Pasos siguientes

Para entender mejor los procesos de Active Directory durante la operación de unión a un dominio, consulte [Problemas de unión y autenticación][join-authentication-issues].

Si sigue teniendo problemas para unir la máquina virtual al dominio administrado, [busque ayuda y abra una incidencia de soporte técnico para Azure Active Directory][azure-ad-support].

<!-- INTERNAL LINKS -->
[enable-password-sync]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-dns]: tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network

<!-- EXTERNAL LINKS -->
[join-authentication-issues]: /previous-versions/windows/it-pro/windows-2000-server/cc961817(v=technet.10)
