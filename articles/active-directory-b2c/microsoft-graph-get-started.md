---
title: Administración de recursos con Microsoft Graph
titleSuffix: Azure AD B2C
description: Prepárese para la administración de recursos de Azure AD B2C con Microsoft Graph mediante el registro de una aplicación que tenga concedidos los permisos de Graph API necesarios.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 32117d4bfcf0c0af94eced095b94ab0c1b6f88af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184368"
---
# <a name="manage-azure-ad-b2c-with-microsoft-graph"></a>Administración de Azure AD B2C con Microsoft Graph

[Microsoft Graph][ms-graph] le permite administrar muchos de los recursos de su inquilino de Azure AD B2C, incluidas las cuentas de usuario del cliente y las directivas personalizadas. Al escribir scripts o aplicaciones que llaman a [Microsoft Graph API][ms-graph-api], puede automatizar las tareas de administración de inquilinos como:

* Migrar un almacén de usuario existente a un inquilino de Azure AD B2C
* Implementar directivas personalizadas con una canalización de Azure en Azure DevOps y administrar claves de directivas personalizadas
* Hospedar un registro de usuarios en su propia página y crear cuentas de usuario en su directorio de Azure AD B2C en segundo plano
* Automatizar el registro de aplicación
* Obtener los registros de auditoría

Las secciones siguientes le ayudarán a preparar el uso de Microsoft Graph API para automatizar la administración de recursos en el directorio de Azure AD B2C.

## <a name="microsoft-graph-api-interaction-modes"></a>Modos de interacción de Microsoft Graph API

Existen dos modos de comunicación que puede usar al trabajar con Microsoft Graph API para administrar los recursos del inquilino de Azure AD B2C:

* **Interactivo**: adecuado para las tareas que se ejecutan una sola vez, se usa una cuenta de administrador en el inquilino de B2C para realizar las tareas de administración. Este modo requiere que un administrador inicie sesión con sus credenciales antes de llamar a Microsoft Graph API.

* **Automatizado**: para tareas programadas o ejecutadas continuamente, este método usa una cuenta de servicio que se configura con los permisos necesarios para realizar tareas de administración. Crea la "cuenta de servicio" en Azure AD B2C registrando una aplicación que las aplicaciones y los scripts usan para la autenticación mediante su *identificador de aplicación (cliente)* y la concesión de credenciales de cliente de OAuth 2.0. En este caso, la aplicación actúa como tal para llamar a Microsoft Graph API, no al usuario administrador como en el método interactivo descrito anteriormente.

Puede habilitar el escenario de interacción **Automatizado** mediante la creación de un registro de aplicación que se muestra en las secciones siguientes.

## <a name="register-management-application"></a>Registro de una aplicación de administración

Antes de que los scripts y las aplicaciones puedan interactuar con [Microsoft Graph API][ms-graph-api] para administrar recursos de Azure AD B2C, debe crear un registro de aplicación en el inquilino de Azure AD B2C que conceda los permisos de API necesarios.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="grant-api-access"></a>Concesión de acceso de API

A continuación, conceda los permisos de aplicación registrados para manipular los recursos de inquilino mediante llamadas a Microsoft Graph API.

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>Creación de un secreto de cliente

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Ahora tiene una aplicación con permisos para *crear*, *leer*, *actualizar* y *eliminar* usuarios en el inquilino de Azure AD B2C. Continúe en la siguiente sección para agregar permisos de *actualización de contraseñas*.

## <a name="enable-user-delete-and-password-update"></a>Habilitación de eliminación de usuarios y de actualización de contraseñas

El permiso *Leer y escribir datos de directorio* **NO** incluye la capacidad de eliminar usuarios o actualizar contraseñas de cuentas de usuarios.

Si la aplicación o el script debe eliminar usuarios o actualizar sus contraseñas, asigne el rol *Administrador de usuarios* a la aplicación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y utilice el filtro **Directorio + suscripción** para cambiar al inquilino de Azure AD B2C.
1. Busque y seleccione **Azure AD B2C**.
1. En **Administrar**, seleccione **Roles y administradores**.
1. Seleccione el rol **Administrador de usuarios**.
1. Seleccione **Agregar asignaciones**.
1. En el cuadro de texto **Seleccionar**, escriba el nombre de la aplicación que registró antes; por ejemplo, *managementapp1*. Seleccione su aplicación cuando aparezca en los resultados de búsqueda.
1. Seleccione **Agregar**. Los permisos pueden tardar unos minutos en propagarse por completo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha registrado su aplicación de administración y le ha concedido los permisos necesarios, las aplicaciones y los servicios (por ejemplo, Azure Pipelines) pueden usar sus credenciales y permisos para interactuar con Microsoft Graph API.

* [Operaciones de B2C compatibles con Microsoft Graph](microsoft-graph-operations.md)
* [Administrar cuentas de usuario de Azure AD B2C con Microsoft Graph](manage-user-accounts-graph-api.md)
* [Obtención de registros de auditoría con la API de generación de informes de Azure AD](view-audit-logs.md#get-audit-logs-with-the-azure-ad-reporting-api)

<!-- LINKS -->
[ms-graph]: https://docs.microsoft.com/graph/
[ms-graph-api]: https://docs.microsoft.com/graph/api/overview
