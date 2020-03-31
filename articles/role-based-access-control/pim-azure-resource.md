---
title: Administración del acceso a los recursos de Azure con Azure AD y PIM
description: Obtenga información sobre cómo administrar el acceso a los recursos de Azure con Azure Active Directory Privileged Identity Management (PIM) y el control de acceso basado en rol (RBAC).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 53c6375ce04425eae27f4c935e613cc27d058dc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77138031"
---
# <a name="manage-access-to-azure-resources-with-azure-ad-privileged-identity-management"></a>Administración del acceso a los recursos de Azure con Azure Active Directory Privileged Identity Management

Para proteger las cuentas con privilegios de ciberataques malintencionados, puede usar Azure Active Directory Privileged Identity Management (PIM) para reducir el tiempo de exposición de los privilegios y aumentar la visibilidad de su uso a través de alertas e informes. PIM lo hace limitando a los usuarios a que solo acepten sus privilegios "just-in-time" (JIT) o mediante la asignación de privilegios por un período de tiempo más corto, tras el cual se revocan automáticamente. 

Ya puede usar PIM con control de acceso basado en roles (RBAC) de Azure para administrar, controlar y supervisar el acceso a recursos de Azure. PIM puede administrar la pertenencia de roles integrados y personalizados para ayudarle a: 

- Habilitar el acceso "just-in-time" y a petición a recursos de Azure
- Hacer expirar automáticamente el acceso a los recursos para usuarios y grupos asignados
- Asignar acceso temporal a recursos de Azure para tareas rápidas u horarios de guardia
- Reciba alertas cuando se asigne acceso a los recursos para nuevos usuarios o grupos, así como cuando activen asignaciones aptas

Para más información, vea [¿Qué es Azure AD Privileged Identity Management?](../active-directory/privileged-identity-management/pim-configure.md).