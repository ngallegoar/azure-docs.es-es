---
title: Dominios suspendidos en Azure AD Domain Services | Microsoft Docs
description: Conozca los distintos estados de mantenimiento de un dominio administrado de Azure AD DS y cómo restaurar un dominio suspendido.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/09/2020
ms.author: iainfou
ms.openlocfilehash: 79b5f4492d05880e263f8d489a64ba0cc218d355
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223404"
---
# <a name="understand-the-health-states-and-resolve-suspended-domains-in-azure-active-directory-domain-services"></a>Descripción de los estados de mantenimiento y resolución de dominios suspendidos en Azure Active Directory Domain Services

Cuando Azure Active Directory Domain Services (Azure AD DS) no puede atender un dominio administrado durante un largo período de tiempo, coloca el dominio administrado en un estado suspendido. Si un dominio administrado permanece en estado suspendido, se eliminará automáticamente. Para mantener el dominio administrado de Azure AD DS correcto y evitar la suspensión, resuelva las alertas tan pronto como pueda.

En este artículo se explica por qué se suspenden los dominios administrados y cómo recuperar un dominio suspendido.

## <a name="overview-of-managed-domain-states"></a>Información general de los estados de dominio administrado

A lo largo del ciclo de vida de un dominio administrado, hay distintos estados que indican su estado. Si el dominio administrado informa de un problema, resuelva rápidamente la causa subyacente para evitar que el estado siga degradado.

![La progresión de estados que un dominio administrado lleva a la suspensión](media/active-directory-domain-services-suspension/suspension-timeline.PNG)

Un dominio administrado puede encontrarse en uno de los siguientes estados:

* [Ejecución](#running-state)
* [Requiere atención](#needs-attention-state)
* [Suspendido](#suspended-state)
* [Eliminado](#deleted-state)

## <a name="running-state"></a>Estado "En ejecución"

Un dominio administrado que está configurado correctamente y se ejecuta sin problemas se encuentra en el estado *En ejecución*. Este es el estado que se quiere en un dominio administrado.

### <a name="what-to-expect"></a>Qué esperar

* La plataforma Azure puede supervisar periódicamente el estado del dominio administrado.
* Los controladores de dominio del dominio administrado se revisan y actualizan de manera periódica.
* Los cambios que se realizan en Azure Active Directory se sincronizan periódicamente con el dominio administrado.
* Se realizan copias de seguridad periódicas del dominio administrado.

## <a name="needs-attention-state"></a>Estado "Requiere atención"

Un dominio administrado con uno o varios problemas que es necesario corregir se encuentra en el estado *Requiere atención*. En la página de estado del dominio administrado se muestran las alertas y se indica dónde hay un problema.

Algunas alertas son transitorias y se resuelven automáticamente en la plataforma Azure. En el caso de otras alertas, puede corregir el problema siguiendo los pasos de resolución proporcionados. Si hay una alerta crítica, [abra una solicitud de Soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

Un ejemplo de alerta es cuando hay un grupo de seguridad de red restrictivo. En esta configuración, es posible que la plataforma Azure no pueda actualizar y supervisar el dominio administrado. Se genera una alerta y el estado cambia a *Requiere atención*.

Para más información, consulte [Solución de problemas de alertas para un dominio administrado][resolve-alerts].

### <a name="what-to-expect"></a>Qué esperar

Cuando un dominio administrado se encuentra en el estado *Requiere atención*, es posible que la plataforma de Azure no pueda supervisar, aplicar revisiones, actualizar o realizar copias de seguridad de los datos de forma periódica. En algunos casos, como en el caso de una configuración de red no válida, es posible que no se pueda acceder a los controladores de dominio del dominio administrado.

* El dominio administrado tiene un estado incorrecto y el seguimiento de estado actual se podría detener hasta que se resuelva la alerta.
* Los controladores de dominio del dominio administrado no se pueden revisar ni actualizar.
* Es posible que los cambios que se realicen en Azure Active Directory no se puedan sincronizar con el dominio administrado.
* Puede que no se realicen copias de seguridad de los dominios administrados.
* Si resuelve alertas no críticas que afectan al dominio administrado, el estado debe volver al estado *En ejecución*.
* Las alertas críticas se desencadenan en caso de errores de configuración cuando la plataforma Azure no puede acceder a los controladores de dominio. Si las alertas en cuestión no se resuelven en un plazo de 15 días, el dominio administrado se coloca en el estado *Suspendido*.

## <a name="suspended-state"></a>Estado "Suspendido"

Un dominio administrado pasa al estado **Suspendido** por uno de los motivos siguientes:

* Una o varias alertas críticas no se han resuelto en 15 días.
    * Las alertas críticas pueden deberse a un error de configuración que bloquea el acceso a los recursos que Azure AD DS necesita. Por ejemplo, la alerta [AADDS104: Error de red][alert-nsg] ha estado sin resolver durante más de 15 días en el dominio administrado.
* Hay un problema con la facturación de la suscripción de Azure o esta expiró.

Los dominios administrados se suspenden cuando la plataforma Azure no puede administrar, supervisar, aplicar revisiones ni crear copias de seguridad del dominio. Un dominio administrado solo permanece en un estado *Suspendido* durante 15 días. Para mantener el acceso al dominio administrado, resuelva inmediatamente las alertas críticas.

### <a name="what-to-expect"></a>Qué esperar

Cuando un dominio administrado se encuentra en el estado *Suspendido*, se produce el siguiente comportamiento:

* Los controladores de dominio del dominio administrado se desaprovisionan y no son accesibles dentro de la red virtual.
* El acceso de LDAP seguro al dominio administrado a través de Internet, si se ha habilitado, deja de funcionar.
* Se observan errores en la autenticación en el dominio administrado, el inicio de sesión en máquinas virtuales unidas al dominio o en la conexión a través de LDAP/LDAPS.
* Se dejan de crear copias de seguridad del dominio administrado.
* La sincronización con Azure AD se detiene.

### <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>¿Cómo es posible saber si el dominio administrado está suspendido?

Verá una [alerta][resolve-alerts] en la página Estado de Azure AD DS de Azure Portal que declara el dominio suspendido. El estado del dominio también muestra *Suspendido*.

### <a name="restore-a-suspended-domain"></a>Restauración de un dominio suspendido

Para restaurar el estado de un dominio administrado que se encuentra en el estado *Suspendido*, complete los pasos siguientes:

1. En Azure Portal, busque y seleccione **Domain Services**.
1. En la lista, elija un dominio administrado como, por ejemplo, *aaddscontoso.com* y, luego, seleccione **Estado**.
1. Seleccione la alerta, como *AADDS503* o *AADDS504*, en función de la causa de la suspensión.
1. Elija el vínculo de resolución que se proporciona en la alerta y siga los pasos para resolverla.

Un dominio administrado solo se puede restaurar a la fecha de la última copia de seguridad. La fecha de la última copia de seguridad se muestra en la página **Estado** del dominio administrado. No se restaurarán los cambios que se produjeron después de la última copia de seguridad. Las copias de seguridad de un dominio administrado se almacenan por hasta 30 días. Las copias de seguridad que tienen una antigüedad mayor a 30 días se eliminan.

Después de resolver las alertas cuando el dominio administrado se encuentra en el estado *Suspendido*, [abra una solicitud de Soporte técnico de Azure][azure-support] para volver a un estado correcto. Si hay una copia de seguridad de menos de 30 días, el Soporte técnico de Azure puede restaurar el dominio administrado.

## <a name="deleted-state"></a>Estado "Eliminado"

Si un dominio administrado permanece en el estado *Suspendido* durante 15 días, se elimina. Este proceso es irrecuperable.

### <a name="what-to-expect"></a>Qué esperar

Cuando un dominio administrado pasa al estado *Eliminado*, se produce el comportamiento siguiente:

* Se eliminan todos los recursos y las copias de seguridad del dominio administrado.
* No puede restaurar el dominio administrado. Debe crear un dominio administrado de reemplazo para reutilizar Azure AD DS.
* Después de eliminarse, no se le facturará el dominio administrado.

## <a name="next-steps"></a>Pasos siguientes

Para mantener el dominio administrado en estado correcto y minimizar el riesgo de que se suspenda, obtenga información sobre cómo [resolver alertas para el dominio administrado][resolve-alerts].

<!-- INTERNAL LINKS -->
[alert-nsg]: alert-nsg.md
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[resolve-alerts]: troubleshoot-alerts.md
