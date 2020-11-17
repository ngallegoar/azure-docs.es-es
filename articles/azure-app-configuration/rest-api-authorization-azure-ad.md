---
title: 'API REST de Azure App Configuration: autorización de Azure Active Directory'
description: Use Azure Active Directory para la autorización en Azure App Configuration mediante API REST
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: bebab7c06062726f7b5c7868f984cadda3b4c98e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423782"
---
# <a name="azure-active-directory-authorization---rest-api-reference"></a>Autorización de Azure Active Directory: referencia de API REST

Cuando se usa la autenticación de Azure Active Directory (Azure AD), la autorización se controla mediante control de acceso basado en rol (RBAC) de Azure. RBAC de Azure exige que los usuarios estén asignados a roles para conceder acceso a los recursos. Cada rol contiene un conjunto de acciones que los usuarios asignados al rol pueden realizar.

## <a name="roles"></a>Roles

Los roles siguientes son roles integrados que están disponibles en las suscripciones de Azure de manera predeterminada:

- **Propietario de los datos de Azure App Configuration**: este rol proporciona acceso completo a todas las operaciones.
- **Lector de los datos de Azure App Configuration**: este rol permite operaciones de lectura.

## <a name="actions"></a>Acciones

Los roles contienen una lista de las acciones que pueden realizar los usuarios asignados a ese rol. Azure App Configuration admite las siguientes acciones:

- `Microsoft.AppConfiguration/configurationStores/keyValues/read`: esta acción permite el acceso de lectura a recursos de clave y valor de App Configuration como /kv y /labels.
- `Microsoft.AppConfiguration/configurationStores/keyValues/write`: esta acción permite el acceso de escritura a recursos de clave y valor de App Configuration.
- `Microsoft.AppConfiguration/configurationStores/keyValues/delete`: esta acción permite que se eliminen recursos de clave y valor de App Configuration. Tenga en cuenta que al eliminar un recurso se devuelve el valor y clave que se ha eliminado.

## <a name="errors"></a>Errors

```http
HTTP/1.1 403 Forbidden
```

**Motivo:** la entidad de seguridad que realiza la solicitud no tiene los permisos necesarios para llevar a cabo la operación solicitada.
**Solución:** asigne el rol necesario para realizar la operación solicitada a la entidad de seguridad que lleva a cabo la solicitud.

## <a name="managing-role-assignments"></a>Administración de asignaciones de roles

La administración de las asignaciones de roles se realiza mediante procedimientos de [RBAC de Azure](https://docs.microsoft.com/azure/role-based-access-control/overview) que son estándar en todos los servicios de Azure. Es posible hacerlo mediante la CLI de Azure, PowerShell, Azure Portal, etc. La documentación oficial sobre cómo realizar asignaciones de roles se puede encontrar [aquí](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
