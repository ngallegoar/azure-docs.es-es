---
title: Problema al guardar las credenciales de administrador durante la configuración de la aplicación de la galería de Azure AD
description: Obtenga información sobre cómo solucionar problemas al guardar las credenciales de administrador al configurar el aprovisionamiento de usuarios en una aplicación de galería de Azure Active Directory.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/21/2018
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 7a3340e72499087dce7773264272601dfce8a50f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91266789"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Problema al guardar las credenciales de administrador durante la configuración del aprovisionamiento de usuarios para una aplicación de la galería de Azure Active Directory 

Al usar Azure Portal para configurar el [aprovisionamiento de usuarios automático](user-provisioning.md) para una aplicación empresarial, puede hallarse en una situación en la que:

* Las **credenciales de administrador** especificadas para la aplicación son válidas y el botón **Probar conexión** funciona. Sin embargo, las credenciales no se pueden guardar y Azure Portal devuelve un mensaje de error genérico.

Si el inicio de sesión único basado en SAML también está configurado para la misma aplicación, la causa más probable del error será que se ha superado el límite de almacenamiento por aplicación interno de Azure AD en el caso de los certificados y las credenciales.

En la actualidad, Azure AD cuenta con una capacidad de almacenamiento máximo de un 1024 bytes para todos los certificados, tokens secretos, credenciales y datos de configuración relacionados asociados a una sola instancia de una aplicación (también conocida como registro de entidades de servicio en Azure AD).

Al configurarse el inicio de sesión único basado en SAML, el certificado usado para firmar los tokens SAML se almacena aquí. Además, suele consumir más del 50 % del espacio.

Los tokens secretos, URI, direcciones de correo electrónico de notificación, nombres de usuario y contraseñas que se escriben durante la configuración del aprovisionamiento de usuarios pueden hacer que se supere el límite de almacenamiento.

## <a name="how-to-work-around-this-issue"></a>Cómo evitar este problema 

Hay dos formas posibles de evitar este problema hoy:

1. **Usar dos instancias de aplicación de galería, una para el inicio de sesión único y la otra para el aprovisionamiento de usuarios**: tomando la aplicación de galería [LinkedIn Elevate](../saas-apps/linkedinelevate-tutorial.md) como ejemplo, puede agregar LinkedIn Elevate desde la galería y configurarla para el inicio de sesión único. Para el aprovisionamiento, agregue otra instancia de LinkedIn Elevate de la galería de aplicaciones y asígnele el nombre "LinkedIn Elevate (aprovisionamiento)". Para esta segunda instancia, configure el [aprovisionamiento](../saas-apps/linkedinelevate-provisioning-tutorial.md), pero no el inicio de sesión único. Al usar esta solución alternativa, los mismos usuarios y grupos deben [asignarse](../manage-apps/assign-user-or-group-access-portal.md) a ambas aplicaciones. 

2. **Reducir la cantidad de datos de configuración almacenados**: todos los datos escritos en la sección [Credenciales de administrador](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) de la pestaña de aprovisionamiento se almacenan en el mismo lugar que el certificado SAML. Mientras que puede que no sea posible reducir la longitud de todos estos datos, algunos campos de configuración opcionales como el **Correo electrónico de notificación** se pueden quitar.

## <a name="next-steps"></a>Pasos siguientes
[Configuración del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS](user-provisioning.md)
