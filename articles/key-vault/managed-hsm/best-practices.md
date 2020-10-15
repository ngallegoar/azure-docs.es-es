---
title: Procedimientos recomendados para usar Azure Key Vault Managed HSM
description: En este documento se explican algunos de los procedimientos recomendados para utilizar Key Vault.
services: key-vault
author: amitbapat
tags: azure-key-vault
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: conceptual
ms.date: 09/17/2020
ms.author: ambapat
ms.openlocfilehash: 7a30a7ab6689b602bc9ad4f696a6fe54c80f2151
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993686"
---
# <a name="best-practices-when-using-managed-hsm"></a>Procedimientos recomendados al usar Managed HSM

## <a name="control-access-to-your-managed-hsm"></a>Control de acceso a Managed HSM

Managed HSM es un servicio en la nube que protege las claves de cifrado. Como estas claves son confidenciales y críticas para la empresa, asegúrese de proteger el acceso a los HSM administrados de modo que solo se admitan las aplicaciones y los usuarios autorizados. En este [artículo](access-control.md) se proporciona información general sobre modelo de acceso. Explica la autenticación y autorización, y el control de acceso basado en roles.
- Cree un [grupo de seguridad de Azure Active Directory](../../active-directory/fundamentals/active-directory-manage-groups.md) para los administradores de HMS (en lugar de asignar el rol Administrador a usuarios individuales). Esto impedirá el "bloqueo de administración" en caso de eliminación de una cuenta individual.
- Bloquee el acceso a los grupos de administración, las suscripciones, los grupos de recursos y los HSM administrados: use RBAC de Azure para controlar el acceso a los grupos de administración, las suscripciones y los grupos de recursos.
- Cree asignaciones de roles por clave con [RBAC local de Managed HSM](access-control.md#data-plane-and-managed-hsm-local-rbac).
- Establezca la entidad de seguridad de acceso con privilegios mínimos para asignar los roles.

## <a name="choose-regions-that-support-availability-zones"></a>Elección de las regiones que admiten zonas de disponibilidad

- Para garantizar la mejor resistencia de zona y alta disponibilidad, elija regiones de Azure donde se admita [Availability Zones](../../availability-zones/az-overview.md). Estas regiones aparecen como "Regiones recomendadas" en Azure Portal.

## <a name="backup"></a>Backup

- Asegúrese de hacer copias de seguridad periódicas del HSM. Las copias de seguridad se pueden hacer en el nivel de HSM y para claves específicas. 

## <a name="turn-on-logging"></a>Active el registro.

- [Active el registro](logging.md) para el HSM. De igual modo, configure alertas.

## <a name="turn-on-recovery-options"></a>Activar las opciones de recuperación

- La [eliminación temporal](../general/soft-delete-overview.md) está habilitada de manera predeterminada.
- Active la protección de purgas si quiere tener protección frente a posibles eliminaciones forzadas del HSM, incluso con la eliminación temporal activada.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Copia de seguridad completa/restauración](backup-restore.md) para información sobre la copia de seguridad completa o restauración de HSM.
- Consulte [Registro de Managed HSM](logging.md) para aprender a usar Azure Monitor para configurar el registro.
- Consulte [Administración de claves de HSM administrado](key-management.md) para la administración de claves.
- Consulte [Administración de roles de HSM administrado](role-management.md) para la administración de asignaciones de roles.
