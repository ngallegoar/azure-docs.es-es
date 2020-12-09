---
title: Resolución de alertas de LDAP seguro en Azure AD Domain Services | Microsoft Docs
description: Obtenga información acerca de cómo solucionar problemas y resolver alertas comunes con LDAP seguro para Azure Active Directory Domain Services.
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/09/2020
ms.author: justinha
ms.openlocfilehash: 15c1f3a1731edf7b45061646d43688b4aacc6104
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620314"
---
# <a name="known-issues-secure-ldap-alerts-in-azure-active-directory-domain-services"></a>Problemas conocidos: Alertas de LDAP seguro en Azure Active Directory Domain Services

Las aplicaciones y los servicios que usan el Protocolo ligero de acceso a directorios (LDAP) para comunicarse con Azure Active Directory Domain Services (Azure AD DS) se pueden [configurar para usar LDAP seguro](tutorial-configure-ldaps.md). Para que LDAP seguro funcione correctamente, deben estar abiertos un certificado adecuado y los puertos de red necesarios.

Este artículo le ayuda a comprender y resolver las alertas comunes con acceso a LDAP seguro en Azure AD DS.

## <a name="aadds101-secure-ldap-network-configuration"></a>AADDS101: Configuración de red de LDAP seguro

### <a name="alert-message"></a>Mensaje de alerta

*LDAP seguro a través de Internet está habilitado para el dominio administrado. Sin embargo, el acceso al puerto 636 no está bloqueado mediante un grupo de seguridad de red. Esto puede exponer las cuentas de usuario del dominio administrado a los ataques de fuerza bruta de contraseñas.*

### <a name="resolution"></a>Resolución

Al habilitar LDAP seguro, se recomienda crear reglas adicionales que restrinjan el acceso de LDAPS entrantes a direcciones IP específicas. Estas reglas protegen el dominio administrado de ataques por fuerza bruta. Para actualizar el grupo de seguridad de red y restringir el acceso al puerto TCP 636 para LDAP seguro, complete los pasos siguientes:

1. En Azure Portal, busque y seleccione **Grupos de seguridad de red**.
1. Elija el grupo de seguridad de red asociado a su dominio administrado, como *AADDS-contoso.com-NSG* y, a continuación, seleccione **Reglas de seguridad de entrada**.
1. Seleccione **+Agregar** para crear una regla para el puerto TCP 636. Si es necesario, seleccione **Avanzado** en la ventana para crear una regla.
1. En **Origen**, elija *Direcciones IP* en el menú desplegable. Escriba las direcciones IP de origen a las que desea conceder acceso para el tráfico LDAP seguro.
1. Elija *Cualquiera* como **Destino** y escriba *636* en **Intervalos de puerto de destino**.
1. Establezca el **Protocolo** como *TCP* y la **Acción** en *Permitir*.
1. Especifique la prioridad de la regla y escriba un nombre como *RestrictLDAPS*.
1. Cuando esté preparado, seleccione **Agregar** para crear la regla.

El estado del dominio administrado se actualiza automáticamente en dos horas y quita la alerta.

> [!TIP]
> El puerto 636 de TCP no es la única regla necesaria para que Azure AD DS se ejecute sin problemas. Para más información, consulte los [grupos de seguridad de red y los puertos requeridos de Azure AD DS](network-considerations.md#network-security-groups-and-required-ports).

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502: expiración del certificado de LDAP seguro

### <a name="alert-message"></a>Mensaje de alerta

*El certificado LDAP seguro del dominio administrado expirará en [fecha].*

### <a name="resolution"></a>Resolución

Cree un certificado LDAP seguro de reemplazo; para ello, siga los pasos y [cree un certificado para LDAP seguro](tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap). Aplique el certificado de reemplazo a Azure AD DS y distribúyalo a los clientes que se conectan mediante LDAP seguro.

## <a name="next-steps"></a>Pasos siguientes

Si los problemas persisten, [abra una solicitud de soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
