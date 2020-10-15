---
title: Migración al control de acceso basado en rol de Azure | Microsoft Docs
description: Obtenga información sobre cómo migrar desde directivas de acceso de almacén a roles de Azure.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 8/30/2020
ms.author: mbaldwin
ms.openlocfilehash: e06a7a759c712b47f3a725a3c49a660226da6a09
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90064089"
---
# <a name="migrate-from-vault-access-policy-to-an-azure-role-based-access-control-preview-permission-model"></a>Migración desde la directiva de acceso de almacén a un modelo de permisos de control de acceso basado en rol de Azure (versión preliminar)

El modelo de directiva de acceso de almacén es un sistema de autorización existente integrado en Key Vault que sirve para proporcionar acceso a claves, secretos y certificados. Puede controlar el acceso mediante la asignación de permisos individuales a la entidad de seguridad (usuario, grupo, entidad de servicio e identidad administrada) en el ámbito de Key Vault. 

El control de acceso basado en rol de Azure es un sistema de autorización basado en [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) que proporciona administración de acceso específico a los recursos de Azure. Azure RBAC para la administración de acceso a certificados, secretos y claves de Key Vault se encuentra actualmente en versión preliminar pública. Con Azure RBAC, puede controlar el acceso a los recursos mediante la creación de asignaciones de roles, que consta de tres elementos: entidad de seguridad, definición de roles (conjunto predefinido de permisos) y ámbito (grupo de recursos o recurso individual). Para más información, consulte [Control de acceso basado en rol de Azure (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview).

Antes de migrar a Azure RBAC, es importante comprender sus ventajas y limitaciones.

Ventajas clave de Azure RBAC en comparación con las directivas de acceso de almacén:
- Proporciona un modelo de control de acceso unificado para los recursos de Azure: la misma API en los servicios de Azure.
- Administración de acceso centralizada para administradores: administrar todos los recursos de Azure en una sola vista.
- Integración con [Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md) para el control de acceso basado en el tiempo.
- Asignaciones de denegación: capacidad de excluir la entidad de seguridad en un ámbito concreto. Para información, vea [Descripción de las asignaciones de denegación de Azure](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments).

Desventajas de Azure RBAC:
- Latencia de las asignaciones de roles: la asignación de roles puede tardar varios minutos en aplicarse. Las directivas de acceso de almacén se asignan al instante.
- Número limitado de asignaciones de roles: 2000 asignaciones de roles por suscripción frente a 1024 directivas de acceso por instancia de Key Vault.

## <a name="access-policies-to-azure-roles-mapping"></a>Directivas de acceso para la asignación de roles de Azure

Azure RBAC tiene varios roles integrados de Azure que se pueden asignar a usuarios, grupos, entidades de servicio e identidades administradas. Si los roles integrados no satisfacen las necesidades específicas de la organización, puede crear [roles personalizados de Azure](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) propios.

Roles integrados de Key Vault para la administración de acceso a secretos, certificados y claves:
- Administrador de almacén de claves (versión preliminar)
- Lector de almacén de claves (versión preliminar)
- Responsable de certificados de almacén de claves (versión preliminar)
- Responsable criptográfico de almacén de claves (versión preliminar)
- Usuario criptográfico de almacén de claves (versión preliminar)
- Responsable de secretos de almacén de claves (versión preliminar)
- Usuario de secretos de almacén de claves (versión preliminar)

Para más información sobre los roles integrados existentes, vea [Roles integrados de Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Se pueden asignar directivas de acceso de almacén con permisos seleccionados individualmente o con plantillas de permisos predefinidas.

Plantillas de permisos predefinidas de directivas de acceso:
- Administración de claves, secretos y certificados
- Administración de claves y secretos
- Administración de secretos y certificados
- Administración de claves
- Administración de secretos
- Administración de certificados
- Conector de SQL Server
- Azure Data Lake Storage o Azure Storage
- Azure Backup
- Clave de cliente de Exchange Online
- Clave de cliente de SharePoint Online
- BYOK para información de Azure

### <a name="access-policies-templates-to-azure-roles-mapping"></a>Plantillas de directivas de acceso para la asignación de roles de Azure
| Plantilla de directiva de acceso | Operations | Rol de Azure |
| --- | --- | --- |
| Administración de claves, secretos y certificados | Claves: todas las operaciones <br>Certificados: todas las operaciones<br>Secretos: todas las operaciones | Administrador de almacén de claves (versión preliminar) |
| Administración de claves y secretos | Claves: todas las operaciones <br>Secretos: todas las operaciones| Responsable criptográfico de almacén de claves (versión preliminar)<br> Responsable de secretos de almacén de claves (versión preliminar)|
| Administración de secretos y certificados | Certificados: todas las operaciones <br>Secretos: todas las operaciones| Responsable de certificados de almacén de claves (versión preliminar)<br> Responsable de secretos de almacén de claves (versión preliminar)|
| Administración de claves | Claves: todas las operaciones| Responsable criptográfico de almacén de claves (versión preliminar)|
| Administración de secretos | Secretos: todas las operaciones| Responsable de secretos de almacén de claves (versión preliminar)|
| Administración de certificados | Certificados: todas las operaciones | Responsable de certificados de almacén de claves (versión preliminar)|
| Conector de SQL Server | Claves: obtener, enumerar, encapsular clave y desencapsular clave | Cifrado de servicio criptográfico de almacén de claves (versión preliminar)|
| Azure Data Lake Storage o Azure Storage | Claves: obtener, enumerar y desencapsular clave | N/D<br> Rol personalizado obligatorio|
| Azure Backup | Claves: obtener, enumerar y copia de seguridad<br> Certificado: obtener, enumerar y copia de seguridad | N/D<br> Rol personalizado obligatorio|
| Clave de cliente de Exchange Online | Claves: obtener, enumerar, encapsular clave y desencapsular clave | Cifrado de servicio criptográfico de almacén de claves (versión preliminar)|
| Clave de cliente de Exchange Online | Claves: obtener, enumerar, encapsular clave y desencapsular clave | Cifrado de servicio criptográfico de almacén de claves (versión preliminar)|
| BYOK para información de Azure | Claves: obtener, descifrar y firmar | N/D<br>Rol personalizado obligatorio|


## <a name="assignment-scopes-mapping"></a>Asignación de ámbitos de asignación  

Azure RBAC para Key Vault permite la asignación de roles en los siguientes ámbitos:
- Grupo de administración
- Subscription
- Resource group
- Recurso de Key Vault
- Clave, secreto y certificado individuales

El modelo de permisos de la directiva de acceso de almacén está limitado a asignar la directiva solo al nivel de recurso de Key Vault, que: 

En general, se recomienda tener un almacén de claves por aplicación y administrar el acceso al nivel de almacén de claves. Hay escenarios en los que la administración del acceso en otros ámbitos puede simplificar la administración del acceso.

- **Infraestructura, operadores y administradores de seguridad: la administración de grupos de almacenes de claves al nivel de grupo de administración, suscripción o grupo de recursos con directivas de acceso de almacén requiere mantener directivas para cada almacén de claves. Azure RBAC permite crear una asignación de roles en el grupo de administración, la suscripción o el grupo de recursos. Esa asignación se aplicará a todos los almacenes de claves creados en el mismo ámbito. En este escenario, se recomienda usar Privileged Identity Management con acceso Just-in-Time a fin de proporcionar acceso permanente.
 
- **Aplicaciones: hay escenarios en los que la aplicación necesitaría compartir el secreto con otra aplicación. Mediante el uso de directivas de acceso de almacén, es necesario crear un almacén de claves independiente para evitar que se conceda acceso a todos los secretos. Azure RBAC permite asignar, en su lugar, un rol con el ámbito de un secreto individual mediante un almacén de claves único.

## <a name="vault-access-policy-to-azure-rbac-migration-steps"></a>Pasos para migrar directivas de acceso de almacén a Azure RBAC
Existen muchas diferencias entre Azure RBAC y el modelo de permisos de la directiva de acceso de almacén. Para evitar interrupciones durante la migración, se recomiendan los pasos siguientes.
 
1. **Identificar y asignar roles**: identifique roles integrados basados en la tabla de asignación anterior y cree roles personalizados cuando sea necesario. Asigne roles en ámbitos, en función de la guía de asignación de ámbitos. Para más información sobre cómo asignar roles al almacén de claves, vea [Proporcionar acceso a Key Vault con un control de acceso basado en rol de Azure (versión preliminar)](rbac-guide.md).
1. **Validar la asignación de roles**: las asignaciones de roles en Azure RBAC pueden tardar varios minutos en propagarse. Para obtener instrucciones sobre cómo comprobar las asignaciones de roles, vea [Lista de las asignaciones de rol en un ámbito](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-list-portal#list-role-assignments-for-a-user-at-a-scope).
1. **Configurar la supervisión y las alertas en el almacén de claves**: es importante habilitar las alertas de registro y configuración de las excepciones de acceso denegado. Para más información, vea [Supervisión y alertas de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/alert).
1. **Establecer el modelo de permisos de control de acceso basado en rol de Azure en Key Vault**: al habilitar el modelo de permisos de Azure RBAC, se invalidarán todas las directivas de acceso existentes. Si se produce un error, se puede volver a cambiar el modelo de permisos con todas las directivas de acceso existentes que queden intactas.

> [!NOTE]
> Cuando el modelo de permisos de Azure RBAC está habilitado, se producirá un error en todos los scripts que intenten actualizar las directivas de acceso. Es importante actualizar esos scripts para que usen Azure RBAC.

## <a name="troubleshooting"></a>Solución de problemas
-  La asignación de roles no funciona después de varios minutos: hay situaciones en las que las asignaciones de roles pueden tardar más tiempo. Es importante escribir lógica de reintento en el código para cubrir esos casos.
- Las asignaciones de roles desaparecieron cuando se eliminó Key Vault (eliminación temporal) y se recuperaron; actualmente, se trata de una limitación de la característica de eliminación temporal en todos los servicios de Azure. Es necesario volver a crear todas las asignaciones de roles después de la recuperación.    

## <a name="learn-more"></a>Más información

- [Información general de Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Tutorial de roles personalizados](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-cli)
- [Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md)
