---
title: 'Creación y administración de las asignaciones de roles: Azure Digital Twins | Microsoft Docs'
description: Aprenda a crear y administrar asignaciones de roles en Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 1c83ca0abfd17db873bec62f0a0d052703862a45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110410"
---
# <a name="create-and-manage-role-assignments-in-azure-digital-twins"></a>Crear y administrar las asignaciones de roles en Azure Digital Twins

Azure Digital Twins usa el control de acceso basado en rol ([RBAC](./security-role-based-access-control.md)) para administrar el acceso a los recursos.

## <a name="role-assignments-overview"></a>Información general sobre asignaciones de roles

Cada asignación de roles se ajusta a la siguiente definición:

```JSON
{
  "roleId": "00e00ad7-00d4-4007-853b-b9968ad000d1",
  "objectId": "be2c6daa-a3a0-0c0a-b0da-c000000fbc5f",
  "objectIdType": "ServicePrincipalId",
  "path": "/",
  "tenantId": "00f000bf-86f1-00aa-91ab-2d7cd000db47"
}
```

En la tabla siguiente se describe cada atributo.

| Atributo | Nombre | Obligatorio | Tipo | Descripción |
| --- | --- | --- | --- | --- |
| roleId | Identificador de definición de rol | Sí | String | El identificador único de la asignación de roles deseada. Busque las definiciones de roles y su identificador consultando la API de sistema o revisando la siguiente tabla. |
| objectId | Identificador de objeto | Sí | String | Un identificador de Azure Active Directory, el identificador de objeto de la entidad de servicio o el nombre de dominio. ¿A qué o a quién se asignan los roles? La asignación de roles debe tener el formato en función del tipo asociado. Para el objectIdType `DomainName`, objectId debe comenzar con el carácter `“@”`. |
| objectIdType | Tipo de identificador de objeto | Sí | String | El tipo del identificador de objeto utilizado. Consulte **ObjectIdTypes admitidos** a continuación. |
| path | Ruta de acceso al espacio | Sí | String | Ruta de acceso completa al objeto `Space`. Un ejemplo es `/{Guid}/{Guid}`. Si un identificador necesita la asignación de roles para todo el grafo, especifique `"/"`. Este carácter designa la raíz, pero se desaconseja su uso. Siga siempre el principio de privilegio mínimo. |
| tenantId | Identificador de inquilino | Varía | String | En la mayoría de los casos, un identificador de inquilino de Azure Active Directory. No se permite para los ObjectIdType `DeviceId` y `TenantId`. Obligatorio para los ObjectIdType `UserId` y `ServicePrincipalId`. Opcional para el ObjectIdType DomainName. |

### <a name="supported-role-definition-identifiers"></a>Identificadores de definición de roles admitidos

Cada asignación de roles asocia una definición de rol a una entidad en el entorno de Azure Digital Twins.

[!INCLUDE [digital-twins-roles](../../includes/digital-twins-roles.md)]

### <a name="supported-object-identifier-types"></a>Tipos de identificadores de objetos admitidos

Anteriormente, se introdujo el atributo **objectIdType**.

[!INCLUDE [digital-twins-object-types](../../includes/digital-twins-object-id-types.md)]

## <a name="role-assignment-operations"></a>Operaciones de asignación de roles

Azure Digital Twins admite operaciones completas *CREATE*, *READ* y *DELETE* para las asignaciones de roles. Las operaciones *UPDATE* se controlan mediante la adición de asignaciones de roles, la eliminación de asignaciones de roles o la modificación de los nodos de [Spatial Intelligence Graph](./concepts-objectmodel-spatialgraph.md) a los que dan acceso las asignaciones de roles.

[![Puntos de conexión de asignación de roles](media/security-roles/role-assignments.png)](media/security-roles/role-assignments.png#lightbox)

La documentación de referencia de Swagger proporcionada contiene información adicional acerca de todos los puntos de conexión de API disponibles, las operaciones de solicitudes y las definiciones.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="grant-permissions-to-your-service-principal"></a>Conceder permisos a la entidad de servicio

Conceder permisos a la entidad de servicio suele ser uno de los primeros pasos que debe realizar cuando trabaja con Azure Digital Twins. Esto implica:

1. Iniciar sesión en la instancia de Azure a mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) o [PowerShell](https://docs.microsoft.com/powershell/azure/).
1. Adquirir la información de la entidad de servicio.
1. Asignar el rol deseado a la entidad de servicio.

El identificador de la aplicación se suministra en Azure Active Directory. Para obtener más información sobre la configuración y el aprovisionamiento de Azure Digital Twins en Active Directory, lea la [Guía de inicio rápido](./quickstart-view-occupancy-dotnet.md).

Una vez que tenga el identificador de la aplicación, ejecute uno de los comandos siguientes. En la CLI de Azure:

```azurecli
az login
az ad sp show --id <ApplicationId>
```

En PowerShell:

```powershell
Login-AzAccount
Get-AzADServicePrincipal -ApplicationId <ApplicationId>
```

Un usuario con el rol de **administrador** puede asignar el rol de administrador de espacio a un usuario mediante una solicitud HTTP POST autenticada a la dirección URL:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments
```

Con el siguiente cuerpo JSON:

```JSON
{
  "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
  "objectId": "YOUR_SERVICE_PRINCIPLE_OBJECT_ID",
  "objectIdType": "ServicePrincipalId",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

### <a name="retrieve-all-roles"></a>Recuperar todos los roles

[![Roles de sistema](media/security-roles/system-api.png)](media/security-roles/system-api.png#lightbox)

Para obtener una lista de todos los roles disponibles (definiciones de roles), realice una solicitud HTTP GET autenticada a:

```URL
YOUR_MANAGEMENT_API_URL/system/roles
```

Una solicitud correcta devolverá una matriz JSON con entradas para cada rol que puede asignarse:

```JSON
[
    {
        "id": "3cdfde07-bc16-40d9-bed3-66d49a8f52ae",
        "name": "DeviceAdministrator",
        "permissions": [
            {
                "notActions": [],
                "actions": [
                    "Read",
                    "Create",
                    "Update",
                    "Delete"
                ],
                "condition": "@Resource.Type Any_of {'Device', 'DeviceBlobMetadata', 'DeviceExtendedProperty', 'Sensor', 'SensorBlobMetadata', 'SensorExtendedProperty'} || ( @Resource.Type == 'ExtendedType' && (!Exists @Resource.Category || @Resource.Category Any_of { 'DeviceSubtype', 'DeviceType', 'DeviceBlobType', 'DeviceBlobSubtype', 'SensorBlobSubtype', 'SensorBlobType', 'SensorDataSubtype', 'SensorDataType', 'SensorDataUnitType', 'SensorPortType', 'SensorType' } ) )"
            },
            {
                "notActions": [],
                "actions": [
                    "Read"
                ],
                "condition": "@Resource.Type == 'Space' && @Resource.Category == 'WithoutSpecifiedRbacResourceTypes' || @Resource.Type Any_of {'ExtendedPropertyKey', 'SpaceExtendedProperty', 'SpaceBlobMetadata', 'SpaceResource', 'Matcher'}"
            }
        ],
        "accessControlPath": "/system",
        "friendlyPath": "/system",
        "accessControlType": "System"
    }
]
```

### <a name="check-a-specific-role-assignment"></a>Comprobar una asignación de roles específica

Para comprobar una asignación de roles específica, realice una solicitud HTTP GET autenticada a:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments/check?userId=YOUR_USER_ID&path=YOUR_PATH&accessType=YOUR_ACCESS_TYPE&resourceType=YOUR_RESOURCE_TYPE
```

| **Valor del parámetro** | **Obligatorio** |  **Tipo** |  **Descripción** |
| --- | --- | --- | --- |
| YOUR_USER_ID |  True | String |   El valor de objectId para el objectIdType UserId. |
| YOUR_PATH | True | String |   La ruta de acceso elegida cuyo acceso se comprobará. |
| YOUR_ACCESS_TYPE |  True | String |   *Read*, *Create*, *Update* o *Delete* |
| YOUR_RESOURCE_TYPE | True | String |  *Device*, *DeviceBlobMetadata*, *DeviceExtendedProperty*, *ExtendedPropertyKey*, *ExtendedType*, *Endpoint*, *KeyStore*, *Matcher*, *Ontology*, *Report*, *RoleDefinition*, *Sensor*, *SensorExtendedProperty*, *Space*, *SpaceBlobMetadata*, *SpaceExtendedProperty*, *SpaceResource*, *SpaceRoleAssignment*, *System*, *UerDefinedFunction*, *User*, *UserBlobMetadata* o *UserExtendedProperty* |

Una solicitud correcta devolverá un valor booleano `true` o `false` para indicar si se ha asignado el tipo de acceso al usuario para el recurso y la ruta de acceso específica.

### <a name="get-role-assignments-by-path"></a>Obtener asignaciones de roles por ruta de acceso

Para obtener todas las asignaciones de roles por ruta de acceso, realice una solicitud HTTP GET autenticada a:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments?path=YOUR_PATH
```

| Value | Reemplazar por |
| --- | --- |
| YOUR_PATH | Ruta de acceso completa al espacio |

Una solicitud correcta devolverá una matriz JSON con cada asignación de roles asociada con el parámetro **path** seleccionado:

```JSON
[
    {
        "id": "0000c484-698e-46fd-a3fd-c12aa11e53a1",
        "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
        "objectId": "0de38846-1aa5-000c-a46d-ea3d8ca8ee5e",
        "objectIdType": "UserId",
        "path": "/"
    }
]
```

### <a name="revoke-a-permission"></a>Revocar un permiso

Para revocar un permiso de un destinatario, elimine la asignación de roles con una solicitud HTTP DELETE autenticada:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments/YOUR_ROLE_ASSIGNMENT_ID
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_ROLE_ASSIGNMENT_ID* | El **id** de la asignación de roles que se quitará |

Una solicitud DELETE correcta devolverá un estado de respuesta 204. Compruebe la eliminación de la asignación de roles [comprobando](#check-a-specific-role-assignment) si la asignación de roles aún sigue siendo válida.

### <a name="create-a-role-assignment"></a>Crear una asignación de rol

Para crear una asignación de roles, realice una solicitud HTTP GET autenticada a la URL:

```URL
YOUR_MANAGEMENT_API_URL/roleassignments
```

Compruebe que el cuerpo JSON se ajuste al esquema siguiente:

```JSON
{
  "roleId": "YOUR_ROLE_ID",
  "objectId": "YOUR_OBJECT_ID",
  "objectIdType": "YOUR_OBJECT_ID_TYPE",
  "path": "YOUR_PATH",
  "tenantId": "YOUR_TENANT_ID"
}
```

Una solicitud correcta devolverá un estado de respuesta 201 junto con el **identificador** de la asignación de roles recién creada:

```JSON
"d92c7823-6e65-41d4-aaaa-f5b32e3f01b9"
```

## <a name="configuration-examples"></a>Ejemplos de configuración

Los siguientes ejemplos muestran cómo configurar el cuerpo JSON en varios escenarios habituales de asignación de roles.

* **Ejemplo**: Un usuario necesita permiso del administrador para acceder a una planta del espacio del inquilino:

   ```JSON
   {
    "roleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
    "objectId" : " 0fc863aa-eb51-4704-a312-7d635d70e000",
    "objectIdType" : "UserId",
    "tenantId": " a0c20ae6-e830-4c60-993d-a00ce6032724",
    "path": "/ 000e349c-c0ea-43d4-93cf-6b00abd23a44/ d84e82e6-84d5-45a4-bd9d-006a000e3bab"
   }
   ```

* **Ejemplo**: Una aplicación ejecuta simulaciones de dispositivos y sensores en escenarios de prueba.

   ```JSON
   {
    "roleId": "98e44ad7-28d4-0007-853b-b9968ad132d1",
    "objectId" : "cabf7aaa-af0b-41c5-000a-ce2f4c20000b",
    "objectIdType" : "ServicePrincipalId",
    "tenantId": " a0c20ae6-e000-4c60-993d-a91ce6000724",
    "path": "/"
   }
    ```

* **Ejemplo**: Todos los usuarios que forman parte de un dominio reciben acceso de lectura para los usuarios, sensores y espacios. Este acceso incluye sus correspondientes objetos relacionados.

   ```JSON
   {
    "roleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
    "objectId" : "@microsoft.com",
    "objectIdType" : "DomainName",
    "path": "/000e349c-c0ea-43d4-93cf-6b00abd23a00"
   }
   ```

## <a name="next-steps"></a>Pasos siguientes

- Para revisar role-based-access-control de Azure Digital Twins, lea [Role-base-access-control](./security-authenticating-apis.md).

- Para más información sobre la autenticación de la API de Azure Digital Twins, consulte el artículo sobre la [autenticación en las API](./security-authenticating-apis.md).
