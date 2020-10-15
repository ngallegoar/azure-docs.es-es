---
title: 'Roles integrados de RBAC local de Managed HSM: Azure Key Vault | Microsoft Docs'
description: Información general de los roles integrados de HSM administrado que se pueden asignar a usuarios, entidades de servicio, grupos e identidades administradas.
services: key-vault
author: amitbapat
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 6b39c1d6fe2e84055e7c94dde076514d04a24f36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90992439"
---
# <a name="managed-hsm-local-rbac-built-in-roles"></a>Roles integrados de RBAC local de Managed HSM

El control de acceso basado en roles (RBAC) local de Managed HSM tiene varios roles integrados que se pueden asignar a usuarios, entidades de servicio, grupos e identidades administradas. Para permitir que una entidad de seguridad realice una operación, debe asignarle un rol que le conceda permiso para llevar a cabo esa operación. Todos estos roles y operaciones solo le permiten administrar el permiso de operaciones del plano de datos. Para administrar permisos del plano de control para el recurso de HSM administrado (por ejemplo, para crear un nuevo HSM administrado o actualizar, trasladar o eliminar uno existente), debe usar el [control de acceso basado en rol (RBAC) de Azure](../../role-based-access-control/overview.md).

## <a name="built-in-roles"></a>Roles integrados

|Nombre de rol|Descripción|ID|
|---|---|---|
|Managed HSM Administrator| Concede acceso total a todas las acciones de datos.|a290e904-7015-4bba-90c8-60543313cdb4|
|Managed HSM Crypto Officer| Concede acceso total a todas las operaciones de administración de claves y de cifrado de claves.|515eb02d-2335-4d2d-92f2-b1cbdf9c3778|
|Managed HSM Crypto User|Concede acceso para crear y usar claves para operaciones de cifrado. No puede eliminar claves de forma permanente.|21dbd100-6940-42c2-9190-5d6cb909625b|
|Managed HSM Policy Administrator| Concede permiso para crear y eliminar asignaciones de roles.|4bd23610-cdcf-4971-bdee-bdc562cc28e4|
|Managed HSM Crypto Auditor|Concede permiso de lectura para leer (pero no usar) claves.|2c18b078-7c48-4d3a-af88-5a3a1b3f82b3|
|Managed HSM Crypto Service Encryption| Concede permiso para usar una clave para el cifrado del servicio. |33413926-3206-4cdd-b39a-83574fe37a17|
|Managed HSM Backup| Concede permiso para realizar una copia de seguridad de HSM completa o de clave única. |7b127d3c-77bd-4e3e-bbe0-dbb8971fa7f8|

## <a name="permitted-operations"></a>Operaciones permitidas
> [!NOTE]  
> - Una "X" indica que un rol puede realizar la acción de datos. Una celda vacía indica que el rol no tiene permiso para realizar esa acción de datos.
> - Todos los nombres de la acción de datos tienen el prefijo "Microsoft.KeyVault/managedHsm", que se omite en las tablas siguientes para abreviar.
> - Todos los nombres de roles tienen el prefijo "Managed HSM", que se omite en la tabla siguiente para abreviar.

|Acción de datos | Administrador | Responsable de cifrado | Usuario de cifrado | Administrador de directivas | Cifrado del servicio criptográfico | Backup | Auditor de cifrado|
|---|---|---|---|---|---|---|---|
|**Administración de dominios de seguridad**|
/securitydomain/download/action|<center>X</center>||||||
/securitydomain/upload/action|<center>X</center>||||||
/securitydomain/upload/read|<center>X</center>||||||
/securitydomain/transferkey/read|<center>X</center>||||||
|**Administración de claves**|
|/keys/read/action|<center>X</center>|<center>X</center>|<center>X</center>||<center>X</center>||<center>X</center>|
|/keys/write/action|<center>X</center>|<center>X</center>|<center>X</center>||||
|/keys/create|<center>X</center>|<center>X</center>|<center>X</center>||||
|/keys/delete|<center>X</center>|<center>X</center>|||||
|/keys/deletedKeys/read/action|<center>X</center>|<center>X</center>|||||
|/keys/deletedKeys/recover/action|<center>X</center>|<center>X</center>|||||
|/keys/deletedKeys/delete|<center>X</center>|<center>X</center>|||||<center>X</center>|
|/keys/backup/action|<center>X</center>|<center>X</center>|<center>X</center>|||<center>X</center>|
|/keys/restore/action|<center>X</center>|<center>X</center>|||||
|/keys/export/action|<center>X</center>|<center>X</center>|||||
|/keys/import/action|<center>X</center>|<center>X</center>|||||
|**Operaciones de cifrado de claves**:|
|/keys/encrypt/action|<center>X</center>|<center>X</center>|<center>X</center>||||
|/keys/decrypt/action|<center>X</center>|<center>X</center>|<center>X</center>||||
|/keys/wrap/action|<center>X</center>|<center>X</center>|<center>X</center>||<center>X</center>||
|/keys/unwrap/action|<center>X</center>|<center>X</center>|<center>X</center>||<center>X</center>||
|/keys/sign/action|<center>X</center>|<center>X</center>|<center>X</center>||||
|/keys/verify/action|<center>X</center>|<center>X</center>|<center>X</center>||||
|**Administración de roles**|
|/roleAssignments/delete/action|<center>X</center>|||<center>X</center>|||
|/roleAssignments/read/action|<center>X</center>|||<center>X</center>|||
|/roleAssignments/write/action|<center>X</center>|||<center>X</center>|||
|/roleDefinitions/read/action|<center>X</center>|||<center>X</center>|||
|**Administración de copia de seguridad y restauración**|
|/backup/start/action|<center>X</center>|||||<center>X</center>|
|/backup/status/action|<center>X</center>|||||<center>X</center>|
|/restore/start/action|<center>X</center>||||||
|/restore/status/action|<center>X</center>||||||
||||||||

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información general del [control de acceso basado en roles (RBAC) de Azure](../../role-based-access-control/overview.md).
- Consulte un tutorial sobre la [Administración de roles de HSM administrado](role-management.md).
