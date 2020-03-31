---
title: Configuración de la compatibilidad con la identidad administrada en un clúster nuevo de Service Fabric
description: A continuación, se indica cómo habilitar la compatibilidad con las identidades administradas en un clúster nuevo de Azure Service Fabric
ms.topic: article
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: 0e35d2192fdcdb294b349105f3f0158564cec86b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76930459"
---
# <a name="configure-managed-identity-support-for-a-new-service-fabric-cluster-preview"></a>Configuración de la compatibilidad con la identidad administrada en un clúster nuevo de Service Fabric (versión preliminar)

Para utilizar las [identidades administradas de los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) en las aplicaciones de Service Fabric, primero debe habilitar el *servicio de token de identidad administrada* en el clúster. Este servicio es responsable de la autenticación de aplicaciones de Service Fabric que usan sus identidades administradas y de la obtención de los tokens de acceso en su nombre. Una vez habilitado el servicio, puede verlo en Service Fabric Explorer, bajo la sección **Sistema** del panel izquierdo. Se ejecuta con el nombre **fabric:/System/ManagedIdentityTokenService** junto a otros servicios del sistema.

> [!NOTE]
> Se requiere un runtime de Service Fabric versión 6.5.658.9590 o posterior para habilitar el **servicio de token de identidad administrada**.  

## <a name="enable-the-managed-identity-token-service"></a>Habilitación del servicio de token de identidad administrada

Para habilitar el servicio de token de identidad administrada durante la creación del clúster, agregue el siguiente fragmento de código en su plantilla de Azure Resource Manager del clúster:

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>Errors

Si se produce un error en la implementación con este mensaje, significa que el clúster no está en la versión de Service Fabric necesaria (el entorno de ejecución mínimo admitido es 6.5 CU2):


```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="related-articles"></a>Artículos relacionados

* Revisión de la [compatibilidad con la identidad administrada](./concepts-managed-identity.md) en Azure Service Fabric

* [Habilitación de la compatibilidad con la identidad administrada en un clúster de Azure Service Fabric existente](./configure-existing-cluster-enable-managed-identity-token-service.md)

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el sistema](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el usuario](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Aprovechamiento de la identidad administrada de una aplicación de Service Fabric desde el código de servicio](./how-to-managed-identity-service-fabric-app-code.md)
* [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](./how-to-grant-access-other-resources.md)