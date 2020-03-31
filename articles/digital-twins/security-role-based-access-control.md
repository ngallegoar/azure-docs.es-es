---
title: 'Descripción del control de acceso basado en rol: Azure Digital Twins | Microsoft Docs'
description: Aprenda sobre el control de acceso basado en rol y la administración de permisos en Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: feda4b3a7f21b581fb4f08aec013f87c0fabb7e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76044929"
---
# <a name="role-based-access-control-in-azure-digital-twins"></a>Control de acceso basado en rol en Azure Digital Twins

Azure Digital Twins permite el control de acceso preciso sobre datos, recursos y acciones específicos en un grafo espacial. Para ello, se usa la administración pormenorizada de roles y permisos que se conoce como [control de acceso basado en rol](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC). RBAC consta de _roles_ y _asignaciones de roles_. Los roles identifican el nivel de permiso. Las asignaciones de roles asocian un rol a un usuario o dispositivo.

Con RBAC, se puede conceder permiso a:

- Usuario.
- Un dispositivo
- Una entidad de servicio
- Una función definida por el usuario
- Todos los usuarios que pertenezcan a un dominio
- Un inquilino

También puede ajustarse el grado de acceso.

El control de acceso basado en rol es único en el sentido de que los permisos se heredan hacia abajo del grafo espacial.

## <a name="what-can-i-do-with-rbac"></a>¿Qué puedo hacer con RBAC?

Un programador puede usar RBAC para:

- Conceder a un usuario la capacidad de administrar los dispositivos de todo un edificio, o de una sala o planta determinadas.
- Conceder a un administrador acceso global a todos los nodos del grafo espacial para un grafo completo o solo para una sección del grafo.
- Conceder a especialista en soporte técnico acceso de lectura al grafo, excepto a las claves de acceso.
- Conceder a todos los miembros de un dominio acceso de lectura a todos los objetos del grafo.

## <a name="rbac-best-practices"></a>Procedimientos recomendados de RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roles

### <a name="role-definitions"></a>Definiciones de roles

Una definición de rol es una colección de permisos y otros atributos que constituyen un rol. Una definición de roles enumera las operaciones permitidas, como *CREAR*, *LEER*, *ACTUALIZAR* y *ELIMINAR* que cualquier objeto con ese rol puede realizar. También especifica a qué tipos de objeto se aplican estos permisos.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

>[!NOTE]
> Para recuperar las definiciones completas de los roles anteriores, consulte la API de sistema o roles.
> Para obtener más información, lea [Creación y administración de asignaciones de roles](./security-create-manage-role-assignments.md#retrieve-all-roles).

### <a name="object-identifier-types"></a>Tipo de identificador de objeto

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

>[!TIP]
> Descubra cómo conceder permisos a la entidad de servicio leyendo [Creación y administración de asignaciones de roles](./security-create-manage-role-assignments.md#grant-permissions-to-your-service-principal).

Estos artículos de documentación de referencia describen lo siguiente:

- Cómo [consultar el identificador de objeto para un usuario](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0).
- Cómo [obtener el identificador de objeto para una entidad de servicio](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal).
- C7 [recuperar el identificador de objeto de un inquilino de Azure AD](../active-directory/develop/quickstart-create-new-tenant.md).

## <a name="role-assignments"></a>Asignaciones de roles

Una asignación de roles de Azure Digital Twins asocia un objeto, como un usuario o un inquilino de Azure AD, a un rol y un espacio. Los permisos se conceden a todos los objetos que pertenecen a dicho espacio. El espacio incluye todo el grafo espacial que hay debajo.

Por ejemplo, un usuario recibe una asignación de roles con el rol `DeviceInstaller` para el nodo raíz de un grafo espacial, que representa un edificio. Así que el usuario puede leer y actualizar los dispositivos de ese nodo y todos los demás espacios secundarios del edificio.

Para conceder permisos a un destinatario, cree una asignación de roles. Para revocar permisos, quite la asignación de roles.

>[!IMPORTANT]
> Para obtener más información sobre las asignaciones de roles, lea [Creación y administración de asignaciones de roles](./security-create-manage-role-assignments.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la creación y administración de asignaciones de roles de Azure Digital Twins, lea [Creación y administración de asignaciones de roles](./security-create-manage-role-assignments.md).

- Obtenga más información sobre [RBAC para Azure](https://docs.microsoft.com/azure/role-based-access-control/).
