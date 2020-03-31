---
title: Corrección de errores de directorios no coincidentes en Azure AD Domain Services | Microsoft Docs
description: Más información sobre lo que significa un error de directorio no coincidente y cómo resolverlo en Azure AD Domain Services
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: iainfou
ms.openlocfilehash: 76dc964b7fe7f5e8acfcfb03b2e89bebb2caa176
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613388"
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Resolver errores de directorios que no coinciden en dominios administrados existentes de Azure AD Domain Services

Si un dominio administrado de Azure Active Directory Domain Services (Azure AD DS) muestra un error de inquilino no coincidente, no se puede administrar el dominio administrado hasta que se resuelva. Este error se produce si la red virtual de Azure subyacente se mueve a otro directorio de Azure AD.

En este artículo se explica por qué se produce el error y cómo resolverlo.

## <a name="what-causes-this-error"></a>¿Qué causa este error?

Un error de directorio no coincidente se produce cuando un dominio administrado de Azure AD DS y una red virtual pertenecen a dos inquilinos de Azure AD distintos. Por ejemplo, tiene un dominio administrado de Azure AD DS denominado *aaddscontoso.com* que se ejecuta en el inquilino de Azure AD de Contoso, pero la red virtual de Azure para el dominio administrado forma parte del inquilino de Azure AD de Fabrikam.

Azure utiliza el control de acceso basado en rol (RBAC) para limitar el acceso a los recursos. Al habilitar Azure AD DS en un inquilino de Azure AD, los hashes de credenciales se sincronizan con el dominio administrado. Esta operación requiere que se sea administrador de inquilinos en el directorio de Azure AD y debe controlarse el acceso a las credenciales. Para implementar recursos en una red virtual de Azure y controlar el tráfico, debe tener privilegios administrativos en la red virtual en la que implementa Azure AD DS.

Para que RBAC funcione de forma coherente y proteja el acceso a todos los recursos que Azure AD DS utiliza, el dominio administrado y la red virtual deben pertenecer al mismo inquilino de Azure AD.

Las reglas siguientes se aplican al entorno de Resource Manager:

- Un directorio de Azure AD puede tener varias suscripciones de Azure.
- Una suscripción de Azure puede tener varios recursos, como redes virtuales.
- Solo hay un dominio administrado de Azure AD Domain Services habilitado para un directorio de Azure AD.
- Un dominio administrado de Azure AD Domain Services puede habilitarse en una red virtual que pertenezca a cualquiera de las suscripciones de Azure incluidas dentro del mismo inquilino de Azure AD.

### <a name="valid-configuration"></a>Configuración válida

En este escenario de implementación de ejemplo, el dominio administrado de Azure AD DS de Contoso está habilitado en el inquilino de Azure AD de Contoso. El dominio administrado se implementa en una red virtual que pertenece a una suscripción de Azure propiedad del inquilino de Azure AD de Contoso. Tanto el dominio administrado como la red virtual pertenecen al mismo inquilino de Azure AD. Esta configuración de ejemplo es válida y totalmente compatible.

![Configuración valida de inquilino de Azure AD DS con el dominio administrado y parte de la red virtual del mismo inquilino de Azure AD](./media/getting-started/valid-tenant-config.png)

### <a name="mismatched-tenant-configuration"></a>Configuración de inquilino que no coincide

En el siguiente escenario de implementación de ejemplo, el dominio administrado de Azure AD DS de Contoso está habilitado en el inquilino de Azure AD de Contoso, pero el dominio administrado se implementa en una red virtual que pertenece a una suscripción de Azure propiedad del inquilino de Azure AD de Fabrikam. El dominio administrado y la red virtual pertenecen a dos inquilinos de Azure AD distintos. Esta configuración de ejemplo corresponde a un inquilino no coincidente y no se admite. La red virtual debe moverse al mismo inquilino de Azure AD (es decir, Contoso) que el dominio administrado.

![Configuración de inquilino que no coincide](./media/getting-started/mismatched-tenant-config.png)

## <a name="resolve-mismatched-tenant-error"></a>Resolución de errores de inquilinos no coincidentes

Las dos opciones siguientes resuelven el error de directorio no coincidente:

* [Eliminación del dominio administrado de Azure AD DS](delete-aadds.md) del directorio de Azure AD existente. [Creación de un dominio administrado de Azure AD DS de reemplazo](tutorial-create-instance.md) en el mismo directorio de Azure AD que la red virtual que se quiere utilizar. Cuando esté listo, una todas las máquinas que antes estaban unidas al dominio eliminado al dominio administrado ha vuelto a crear.
* [Mueva la suscripción de Azure](../cost-management-billing/manage/billing-subscription-transfer.md) que contiene la red virtual al mismo directorio de Azure AD que el dominio administrado de Azure AD DS.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la solución de problemas con Azure AD DS, consulte la [guía de solución de problemas](troubleshoot.md).
