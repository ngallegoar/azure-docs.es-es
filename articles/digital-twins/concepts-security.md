---
title: Seguridad para las soluciones de Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Procedimientos recomendados de seguridad con Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 6784ca9dbc32811a02f4454be94d220c634318f5
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92503324"
---
# <a name="secure-azure-digital-twins"></a>Protección de Azure Digital Twins

Por seguridad, Azure Digital Twins permite el control de acceso preciso sobre datos, recursos y acciones específicos en su implementación. Para ello, se usa una estrategia pormenorizada de roles y permisos que se conoce como **Control de acceso basado en roles de Azure (Azure RBAC)** . Puede leer sobre los principios generales de Azure RBAC [aquí](../role-based-access-control/overview.md).

Azure Digital Twins también admite el cifrado de datos en reposo.

## <a name="roles-and-permissions-with-azure-rbac"></a>Roles y permisos con RBAC de Azure

Azure RBAC se proporciona en Azure Digital Twins mediante la integración con [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD).

Puede usar Azure RBAC para conceder permisos a una *entidad de seguridad* , que puede ser un usuario, un grupo o una entidad de servicio de aplicación. Azure AD autentica la entidad de seguridad y recibe un token de OAuth 2.0 a cambio. Este token se puede usar para autorizar una solicitud de acceso a una instancia de Azure Digital Twins.

### <a name="authentication-and-authorization"></a>Autenticación y autorización

Con Azure AD, el acceso es un proceso de dos pasos. Cuando una entidad de seguridad (un usuario, un grupo o una aplicación) intenta acceder a Azure Digital Twins, la solicitud se debe *autenticar* y *autorizar*. 

1. En primer lugar, se *autentica* la identidad de la entidad de seguridad y se devuelve un token de OAuth 2.0.
2. Luego, el token se pasa como parte de una solicitud al servicio Azure Digital Twins para *autorizar* el acceso al recurso especificado.

El paso de autenticación exige que cualquier solicitud de aplicación contenga un token de acceso de OAuth 2.0 en tiempo de ejecución. Si una aplicación se ejecuta dentro de una entidad de Azure, como una aplicación de [Azure Functions](../azure-functions/functions-overview.md), puede usar una **identidad administrada** para acceder a los recursos. Para obtener más información sobre las identidades administradas, consulte la siguiente sección.

El paso de autorización requiere que se asigne un rol de Azure a la entidad de seguridad. Los roles que se asignan a una entidad de seguridad determinan los permisos que tiene esa entidad de seguridad. Azure Digital Twins proporciona roles de Azure que abarcan conjuntos de permisos para los recursos de Azure Digital Twins. Estos roles se describen más adelante en este artículo.

Para más información sobre los roles y las asignaciones de roles que se admiten en Azure, consulte la [*Descripción de los distintos roles*](../role-based-access-control/rbac-and-directory-admin-roles.md) en la documentación de RBAC de Azure.

#### <a name="authentication-with-managed-identities"></a>Autenticación con identidades administradas

[Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) es una característica de Azure que permite crear una identidad segura asociada con la implementación en la que se ejecuta el código de la aplicación. A continuación, puede asociar esa identidad con los roles de control de acceso para conceder permisos personalizados de acceso a recursos específicos de Azure que la aplicación necesita.

Con las identidades administradas, la plataforma Azure administra esta identidad en tiempo de ejecución. No es necesario almacenar y proteger las claves de acceso en la configuración o el código de la aplicación, ya sea para la propia identidad o para los recursos a los que necesita acceder. Una aplicación cliente de Azure Digital Twins que se ejecuta dentro de una aplicación de Azure App Service no necesita controlar las reglas y claves SAS ni cualquier otro token de acceso. La aplicación cliente solo necesita la dirección del punto de conexión del espacio de nombres de Azure Digital Twins. Cuando se conecta la aplicación, Azure Digital Twins enlaza el contexto de la entidad administrada con el cliente. Una vez creada la asociación con una identidad administrada, el cliente de Azure Digital Twins puede realizar todas las operaciones autorizadas. Para conceder la autorización, se asociará una entidad administrada con un rol de Azure Digital Twins (los roles se describen a continuación).

#### <a name="authorization-azure-roles-for-azure-digital-twins"></a>Autorización: Roles de Azure para Azure Digital Twins

Azure proporciona **dos roles integrados de Azure** para autorizar el acceso a las [API del plano de datos](how-to-use-apis-sdks.md#overview-data-plane-apis) de Azure Digital Twins. Puede hacer referencia a los roles por nombre o por identificador:

| Rol integrado | Descripción | ID | 
| --- | --- | --- |
| Propietario de datos de Azure Digital Twins | Proporciona acceso completo a los recursos de Azure Digital Twins. | bcd981a7-7f74-457b-83e1-cceb9e632ffe |
| Lector de datos de Azure Digital Twins | Proporciona acceso de solo lectura a los recursos de Azure Digital Twins. | d57506d4-4c8d-48b1-8587-93c323f6a5a3 |

>[!NOTE]
> Se ha cambiado el nombre de estos roles recientemente respecto a los nombres anteriores en la versión preliminar:
> * *Propietario de datos de Azure Digital Twins* era anteriormente *Propietario de Azure Digital Twins (versión preliminar)* .
> * *Lector de datos de Azure Digital Twins* era anteriormente *Lector de Azure Digital Twins (versión preliminar)* .

Puede asignar roles de dos maneras:
* mediante el panel de control de acceso (IAM) de Azure Digital Twins en Azure Portal (consulte [*Incorporación o eliminación de asignaciones de roles de Azure mediante Azure Portal*](../role-based-access-control/role-assignments-portal.md))
* mediante comandos de la CLI para agregar o quitar un rol

Para obtener pasos más detallados sobre cómo hacerlo, pruébelo en el [*Tutorial de Azure Digital Twins: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md).

Para más información sobre cómo se definen los roles integrados, consulte [*Descripción de definiciones de roles*](../role-based-access-control/role-definitions.md) en la documentación de RBAC de Azure. Para más información acerca de la creación de roles personalizados de Azure, consulte [*Roles personalizados en los recursos de Azure*](../role-based-access-control/custom-roles.md).

##### <a name="automating-roles"></a>Automatización de roles

Al hacer referencia a los roles en escenarios automatizados, se recomienda hacer referencia a ellos por sus **identificadores** en lugar de sus nombres. Los nombres pueden cambiar entre versiones, pero los identificadores no, lo que los convierte en una referencia más estable en la automatización.

> [!TIP]
> Si se asignan roles con un cmdlet, como `New-AzRoleAssignment` ([referencia](/powershell/module/az.resources/new-azroleassignment?view=azps-4.8.0)), puede usar el parámetro `-RoleDefinitionId` en lugar de `-RoleDefinitionName` para pasar un identificador en lugar de un nombre para el rol.

### <a name="permission-scopes"></a>Ámbitos de permiso

Antes de asignar un rol de Azure a una entidad de seguridad, determine el ámbito de acceso que debería tener la entidad de seguridad. Los procedimientos recomendados dictan que siempre es mejor conceder únicamente el ámbito más restringido posible.

En la lista siguiente se describen los niveles en los que puede definir el ámbito de acceso a recursos de Azure Digital Twins.
* Modelos: Las acciones para este recurso dictan el control sobre los [modelos](concepts-models.md) cargados en Azure Digital Twins.
* Consulta del grafo de Digital Twins: Las acciones para este recurso determinan la capacidad de ejecutar [operaciones de consulta](concepts-query-language.md) en gemelos digitales en el grafo de Azure Digital Twins.
* Gemelo digital: Las acciones de este recurso proporcionan control sobre las operaciones de CRUD en [gemelos digitales](concepts-twins-graph.md) en el grafo gemelo.
* Relación de los gemelos digitales: Las acciones de este recurso definen el control sobre las operaciones de CRUD en las [relaciones](concepts-twins-graph.md) entre gemelos digitales en el grafo gemelo.
* Ruta del evento: Las acciones de este recurso determinan los permisos para [redirigir eventos](concepts-route-events.md) de Azure Digital Twins a un servicio de punto de conexión como [Event Hub](../event-hubs/event-hubs-about.md), [Event Grid](../event-grid/overview.md) o [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

### <a name="troubleshooting-permissions"></a>Solución de problemas de permisos

Si un usuario intenta realizar una acción no permitida por su rol, es posible que reciba el siguiente error de la solicitud de servicio: `403 (Forbidden)`. Para obtener más información y pasos para solucionar problemas, vea [*Solución de problemas: Error en la solicitud de Azure Digital Twins con el estado: 403 (Prohibido)*](troubleshoot-error-403.md).

## <a name="encryption-of-data-at-rest"></a>Cifrado de datos en reposo

Azure Digital Twins permite el cifrado de los datos en reposo y en tránsito a medida que se escriben en nuestros centros de datos y los descifra automáticamente mientras accede a ellos. Este cifrado se produce mediante una clave de cifrado administrada de Microsoft.

## <a name="cross-origin-resource-sharing-cors"></a>Uso compartido de recursos entre orígenes

Azure Digital Twins no admite actualmente el **uso compartido de recursos entre orígenes (CORS)** . Como resultado, si está llamando a una API REST desde una aplicación de explorador, una interfaz de [API Management (APIM)](../api-management/api-management-key-concepts.md) o un conector de [Power Apps](https://docs.microsoft.com/powerapps/powerapps-overview), es posible que vea un error de directiva.

Para resolver este error, realice una de las siguientes acciones:
* Quite el encabezado CORS `Access-Control-Allow-Origin` del mensaje. Este encabezado indica si la respuesta se puede compartir. 
* Como alternativa, cree un proxy CORS y solicite la API REST de Azure Digital Twins través de él. 

## <a name="next-steps"></a>Pasos siguientes

* Vea estos conceptos en acción en [*Procedimiento: Configuración de una instancia y autenticación*](how-to-set-up-instance-portal.md).

* Vea cómo interactuar con estos conceptos del código de la aplicación cliente en [*Procedimiento: Escritura de código de autenticación de aplicación*](how-to-authenticate-client.md).

* Obtenga más información sobre [Azure RBAC](../role-based-access-control/overview.md).
