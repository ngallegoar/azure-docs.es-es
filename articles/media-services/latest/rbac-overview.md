---
title: 'Control de acceso basado en roles de Azure para cuentas de Media Services: Azure | Microsoft Docs'
description: En este artículo se describe el control de acceso basado en roles (RBAC de Azure) para las cuentas de Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 8fba3db14c2a950dd230a4721841b4baa9f64636
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92426797"
---
# <a name="azure-role-based-access-control-azure-rbac-for-media-services-accounts"></a>Control de acceso basado en roles de Azure (RBAC de Azure) para cuentas de Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Actualmente, Azure Media Services no define los roles personalizados específicos para el servicio. Para obtener acceso completo a la cuenta de Media Services, los clientes pueden usar las funciones integradas **Propietario** o **Colaborador**. La principal diferencia entre estos roles es que el rol **Propietario** puede controlar quién tiene acceso a un recurso y el rol **Colaborador** no. El rol integrado **Lector** también se puede usar, pero el usuario o la aplicación solo tendrán acceso de lectura a las API de Media Services. 

## <a name="design-principles"></a>Principios de diseño

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. Las API de la versión v3 no devuelven secretos o credenciales en las operaciones **Get** o **List**. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. El usuario debe llamar a un método de acción independiente para obtener los secretos o las credenciales. El rol **Lector** no puede llamar a operaciones como, por ejemplo, Asset.ListContainerSas, StreamingLocator.ListContentKeys o ContentKeyPolicies.GetPolicyPropertiesWithSecrets. Al tener acciones independientes, se pueden establecer permisos de seguridad de RBAC de Azure más granulares en un rol personalizado, si lo desea.

Para obtener una lista de las operaciones que admite Media Services, haga lo siguiente:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

En el artículo sobre [definiciones de roles integrados](../../role-based-access-control/built-in-roles.md) se explica lo que el rol concede. 

Para obtener más información, consulte los siguientes artículos:

- [Roles de administrador de la suscripción clásica, roles de Azure y roles de Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md)
- [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../../role-based-access-control/overview.md)
- [Incorporación o eliminación de asignaciones de roles de Azure mediante la API REST](../../role-based-access-control/role-assignments-rest.md)
- [Operaciones del proveedor de recursos de Media Services](../../role-based-access-control/resource-provider-operations.md#microsoftmedia)

## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo con API de Media Services v3](media-services-apis-overview.md)
- [Obtener la directiva de clave de contenido mediante Media Services .NET](get-content-key-policy-dotnet-howto.md)
