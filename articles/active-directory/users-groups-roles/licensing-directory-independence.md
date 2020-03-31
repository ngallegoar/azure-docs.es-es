---
title: 'Características de la interacción de varios inquilinos: Azure AD | Microsoft Docs'
description: Administre sus inquilinos de Azure Active Directory considerándolos como recursos completamente independientes.
services: active-tenant
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 11/08/2019
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4eb09ab7fa31af5edf14b113a6a88e08df2d115
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562265"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Información de cómo interactúan varios inquilinos de Azure Active Directory

En Azure Active Directory (Azure AD), cada inquilino es un recurso totalmente independiente: un recurso del mismo nivel, que es lógicamente independiente de otros inquilinos que administre. No hay ninguna relación de elementos primarios y secundarios entre los inquilinos. Esta independencia entre inquilinos incluye la de los recursos, la independencia administrativa y la de sincronización.

## <a name="resource-independence"></a>Independencia de recursos
* Si crea o elimina un recurso en un inquilino, no tiene efecto en ningún recurso de los demás inquilinos, con la excepción parcial de los usuarios externos. 
* Si usa uno de los nombres de dominio con un inquilino, no se puede usar con ningún otro.

## <a name="administrative-independence"></a>Independencia administrativa
Si un usuario no administrador del inquilino "Contoso" crea un directorio de prueba "Prueba", en ese caso:

* De forma predeterminada, el usuario que crea un inquilino se agrega como un usuario externo en ese nuevo inquilino y se le asigna el rol de administrador global en ese inquilino.
* Los administradores del inquilino "Contoso" no tienen privilegios administrativos directos en el inquilino "Prueba", a menos que el administrador de "Prueba" se los otorgue específicamente. Sin embargo, los administradores de "Contoso" pueden controlar el acceso al inquilino "Prueba" si controlan la cuenta de usuario que creó "Prueba".
* Si se agrega o quita un rol de administrador para un usuario en un inquilino, el cambio no afecta a los roles de administrador que tenga el usuario en otro inquilino.

## <a name="synchronization-independence"></a>Independencia de sincronización
Puede configurar cada inquilino de Azure AD de manera independiente para que los datos se sincronicen desde una sola instancia de:

* La herramienta Azure AD Connect, para sincronizar los datos con un único bosque de AD.
* El conector de inquilinos de Azure Active Directory para Forefront Identity Manager, para sincronizar datos con uno o varios bosques locales u orígenes de datos distintos de Azure AD.

## <a name="add-an-azure-ad-tenant"></a>Adición de un inquilino de Azure AD
Para agregar un inquilino de Azure AD en Azure Portal, inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que sea un administrador global de Azure AD y, a la izquierda, seleccione **Nuevo**.

> [!NOTE]
> A diferencia de otros recursos de Azure, los inquilinos no son recursos secundarios de una suscripción a Azure. Aunque su suscripción a Azure se cancele o expire, puede acceder a los datos de su inquilino mediante Azure PowerShell, Microsoft Graph API o el Centro de administración de Microsoft 365. También puede [asociar otra suscripción al inquilino](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Pasos siguientes
Para una amplia visión general de los problemas de licencias de Azure AD y los procedimientos recomendados, consulte [¿Qué es la licencia de inquilinos de Azure Active Directory?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
