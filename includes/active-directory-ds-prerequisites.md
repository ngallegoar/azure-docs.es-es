---
title: archivo de inclusión
description: archivo de inclusión con requisitos previos
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 66b5f8e2-e08d-43c8-b460-6204aecda8f7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.custom: include file
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: f7b1b294e9500ef9e0aadd24cfe3cd4e61fddda1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "68426955"
---
> [!IMPORTANT]
> **Habilite la sincronización de hash de contraseña con Azure AD Domain Services, antes de completar las tareas de este artículo.**
>
> Siga las instrucciones que aparecen a continuación, en función del tipo de usuarios de su directorio de Azure AD. Complete ambos conjuntos de instrucciones si tiene una combinación de cuentas de usuario solo en la nube y sincronizados en su directorio de Azure AD. Es posible que no pueda realizar las siguientes operaciones si intenta usar una cuenta de invitado B2B (por ejemplo, su cuenta de Gmail o MSA de otro proveedor de identidades que permitamos) porque no tenemos la contraseña de estos usuarios sincronizada con el dominio administrado, ya que estas son cuentas de invitado en el directorio. La información completa sobre estas cuentas, incluidas sus contraseñas, queda fuera del alcance de Azure AD y dado que esta información no está en Azure AD, no se sincroniza con el dominio administrado. 
> - [Instrucciones para cuentas de usuario solo en la nube](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
> - [Instrucciones para cuentas de usuario sincronizados desde un directorio local](../articles/active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)
