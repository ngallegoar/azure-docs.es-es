---
title: 'Modificación de la configuración del ciclo de vida para un paquete de acceso en la administración de derechos de Azure AD: Azure Active Directory'
description: Aprenda a modificar la configuración del ciclo de vida de un paquete de acceso en la administración de derechos de Azure Active Directory.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 959d85f496a4a573a969bf736aba137d5b86154a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79230396"
---
# <a name="change-lifecycle-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>Modificación de la configuración del ciclo de vida para un paquete de acceso en la administración de derechos de Azure AD

Como administrador de paquetes de acceso, puede cambiar la configuración del ciclo de vida de un paquete de acceso en cualquier momento si edita una directiva existente. Si cambia la fecha de expiración para una directiva, la fecha de expiración para las solicitudes que ya están aprobadas o en un estado de aprobación pendiente no cambiará.

En este artículo, se explica cómo se puede modificar la configuración del ciclo de vida de un paquete de acceso existente.

## <a name="open-lifecycle-settings"></a>Apertura de la configuración del ciclo de vida

Para cambiar la configuración del ciclo de vida de un paquete de acceso, debe abrir la directiva correspondiente. Siga estos pasos para abrir la configuración del ciclo de vida de un paquete de acceso.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Directivas** y, a continuación, haga clic en la directiva que contenga la configuración del ciclo de vida que desee editar.

    Se abre el panel Detalles de directiva en la parte inferior de la página.

    ![Paquete de acceso - Panel Detalles de directiva](./media/entitlement-management-shared/policy-details.png)

1. Haga clic en **Editar** para editar la directiva.

    ![Paquete de acceso - Editar directiva](./media/entitlement-management-shared/policy-edit.png)

1. Haga clic en la pestaña **Ciclo de vida** para abrir la configuración del ciclo de vida.

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Modificación de la configuración de solicitudes y aprobación de un paquete de acceso](entitlement-management-access-package-request-policy.md)