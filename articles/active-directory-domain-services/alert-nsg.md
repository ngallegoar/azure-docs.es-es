---
title: Resolución de alertas de grupo de seguridad de red en Azure AD DS | Microsoft Docs
description: Información sobre cómo resolver problemas de alertas de configuración de grupo de seguridad de red para Azure Active Directory Domain Services
services: active-directory-ds
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: joflore
ms.openlocfilehash: f8917d7bd8fc1a4091607b9a405cfefbb51bc188
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91962792"
---
# <a name="known-issues-network-configuration-alerts-in-azure-active-directory-domain-services"></a>Problemas conocidos: Alertas de configuración de red en Azure Active Directory Domain Services

Para permitir que las aplicaciones y los servicios se comuniquen correctamente con un dominio administrado de Azure Active Directory Domain Services (Azure AD DS), los puertos de red específicos deben estar abiertos para permitir el flujo del tráfico. En Azure, puede controlar el flujo de tráfico mediante grupos de seguridad de red. El estado de mantenimiento de un dominio administrado de Azure AD DS muestra una alerta si las reglas de grupo de seguridad de red necesarias no están en su lugar.

Este artículo le ayuda a comprender y resolver alertas comunes de problemas de configuración de grupos de seguridad de red.

## <a name="alert-aadds104-network-error"></a>Alerta AADDS104: Error de red

### <a name="alert-message"></a>Mensaje de alerta

*Microsoft no puede tener acceso a los controladores de dominio de este dominio administrado. Esto puede ocurrir si un grupo de seguridad de red (NSG) configurado en la red virtual bloquea el acceso al dominio administrado. Otro motivo posible es que hay una ruta definida por el usuario que bloquea el tráfico entrante desde Internet.*

Las reglas de grupo de seguridad de red no válidas son la causa más común de los errores de red en Azure AD DS. El grupo de seguridad de red para la red virtual debe permitir el acceso a puertos y protocolos específicos. Si estos puertos están bloqueados, la plataforma Azure no puede supervisar ni actualizar el dominio administrado. También se verá afectada la sincronización entre el directorio de Azure AD y Azure AD DS. Asegúrese de mantener abiertos los puertos predeterminados para evitar la interrupción del servicio.

## <a name="default-security-rules"></a>reglas de seguridad predeterminadas

Las siguientes reglas de seguridad de entrada y de salida predeterminadas se aplican al grupo de seguridad de red para un dominio administrado. Estas reglas mantienen Azure AD DS seguro y permiten que la plataforma de Azure supervise, administre y actualice el dominio administrado.

### <a name="inbound-security-rules"></a>Reglas de seguridad de entrada

| Priority | Nombre | Port | Protocolo | Source | Destination | Acción |
|----------|------|------|----------|--------|-------------|--------|
| 101      | AllowSyncWithAzureAD | 443 | TCP | AzureActiveDirectoryDomainServices | Any | Allow |
| 201      | AllowRD | 3389 | TCP | CorpNetSaw | Any | Allow |
| 301      | AllowPSRemoting | 5986| TCP | AzureActiveDirectoryDomainServices | Any | Allow |
| 65000    | AllVnetInBound | Any | Any | VirtualNetwork | VirtualNetwork | Allow |
| 65001    | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Any | Allow |
| 65500    | DenyAllInBound | Any | Any | Any | Any | Denegar |

> [!NOTE]
> También puede tener una regla adicional que permita el tráfico entrante si [configura LDAP seguro][configure-ldaps]. Esta regla adicional es necesaria para la comunicación LDAP correcta.

### <a name="outbound-security-rules"></a>Reglas de seguridad de entrada

| Priority | Nombre | Port | Protocolo | Source | Destination | Acción |
|----------|------|------|----------|--------|-------------|--------|
| 65000    | AllVnetOutBound | Any | Any | VirtualNetwork | VirtualNetwork | Allow |
| 65001    | AllowAzureLoadBalancerOutBound | Any | Any |  Any | Internet | Allow |
| 65500    | DenyAllOutBound | Any | Any | Any | Any | Denegar |

>[!NOTE]
> Azure AD DS necesita un acceso saliente sin restricciones desde la red virtual. No se recomienda crear ninguna regla adicional que restrinja el acceso saliente para la red virtual.

## <a name="verify-and-edit-existing-security-rules"></a>Comprobación y edición de las reglas de seguridad existentes

Para comprobar las reglas de seguridad existentes y asegurarse de que los puertos predeterminados están abiertos, siga estos pasos:

1. En Azure Portal, busque y seleccione **Grupos de seguridad de red**.
1. Elija el grupo de seguridad de red asociado a su dominio administrado, como *AADDS-contoso. com-NSG*.
1. En la página **Información general**, se muestra la lista de reglas de seguridad de entrada y salida existentes.

    Revise las reglas de entrada y de salida, y compárelas con la lista de reglas necesarias en la sección anterior. Si es necesario, seleccione las reglas personalizadas que bloqueen el tráfico necesario y, a continuación, elimínelas. Si falta alguna de las reglas necesarias, agregue una regla en la sección siguiente.

    Después de agregar o eliminar reglas para permitir el tráfico necesario, el estado del dominio administrado se actualiza automáticamente en dos horas y quita la alerta.

### <a name="add-a-security-rule"></a>Agregar una regla de seguridad

Para agregar una regla de seguridad que falta, realice los pasos siguientes:

1. En Azure Portal, busque y seleccione **Grupos de seguridad de red**.
1. Elija el grupo de seguridad de red asociado a su dominio administrado, como *AADDS-contoso. com-NSG*.
1. En **Configuración** del panel de la izquierda, haga clic en *Reglas de seguridad de entrada* o en *Reglas de seguridad de salida*.
1. Seleccione **Agregar** y, después, cree la regla necesaria según el puerto, el protocolo, la dirección, etc. Cuando esté preparado, seleccione **Aceptar**.

La regla de seguridad tarda unos segundos en agregarse y mostrarse en la lista.

## <a name="next-steps"></a>Pasos siguientes

Si los problemas persisten, [abra una solicitud de soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-ldaps]: tutorial-configure-ldaps.md
