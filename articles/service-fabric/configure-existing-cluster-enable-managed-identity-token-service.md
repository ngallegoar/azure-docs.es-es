---
title: Configuración de la compatibilidad con la identidad administrada en un clúster de Service Fabric existente
description: A continuación, se indica cómo habilitar la compatibilidad con las identidades administradas en un clúster de Azure Service Fabric existente
ms.topic: article
ms.date: 03/11/2019
ms.custom: sfrev
ms.openlocfilehash: 73c890e960f26b8e0e3fa924d9ff6b7a4cd4a4dc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415685"
---
# <a name="configure-managed-identity-support-in-an-existing-service-fabric-cluster"></a>Configuración de la compatibilidad con la identidad administrada en un clúster de Service Fabric existente

Para utilizar las [identidades administradas de los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) en las aplicaciones de Service Fabric, primero debe habilitar el *servicio de token de identidad administrada* en el clúster. Este servicio es responsable de la autenticación de aplicaciones de Service Fabric que usan sus identidades administradas y de la obtención de los tokens de acceso en su nombre. Una vez habilitado el servicio, puede verlo en Service Fabric Explorer, en la sección **Sistema** del panel izquierdo. Se ejecuta con el nombre **fabric:/System/ManagedIdentityTokenService**.

> [!NOTE]
> Se requiere un runtime de Service Fabric versión 6.5.658.9590 o posterior para habilitar el **servicio de token de identidad administrada**.  
>
> Para encontrar la versión de Service Fabric de un clúster en Azure Portal, abra el recurso de clúster y active la propiedad **Service Fabric version** en la sección **Essentials**.
>
> Si el clúster está en modo de actualización **Manual**, tendrá que actualizarlo primero a 6.5.658.9590 o posterior.

## <a name="enable-managed-identity-token-service-in-an-existing-cluster"></a>Habilitación del *servicio de token de identidad administrada* en un clúster existente

Para habilitar el servicio de token de identidad administrado en un clúster existente, ha de iniciar una actualización de clúster que especifique dos cambios: (1) habilitar el servicio de token de identidad administrado y (2) solicitar un reinicio de cada nodo. En primer lugar, agregue el siguiente fragmento de código a la plantilla de Azure Resource Manager del clúster:

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

Para que los cambios surtan efecto, también tendrá que cambiar la directiva de actualización para especificar un reinicio forzado del runtime de Service Fabric en cada nodo a medida que la actualización avanza a través del clúster. Con este reinicio se garantiza que el servicio de sistema recién habilitado se inicia y se ejecuta en cada uno de los nodos. En el siguiente fragmento de código, `forceRestart` es la configuración esencial para habilitar el reinicio. Para el resto de parámetros, use los valores que se describen a continuación o los valores personalizados existentes ya especificados para el recurso de clúster. Para ver la configuración personalizada de la directiva de actualización de Fabric ("upgradeDescription") desde Azure Portal, seleccione la opción "Actualizaciones de Fabric" en el recurso de Service Fabric o en resources.azure.com. Las opciones predeterminadas de la directiva de actualización ("upgradeDescription") no se pueden ver desde PowerShell ni desde resources.azure.com. Consulte [ClusterUpgradePolicy](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.servicefabric.models.clusterupgradepolicy?view=azure-dotnet) para obtener información adicional.  

```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```

> [!NOTE]
> Tras la finalización correcta de la actualización, no olvide revertir la opción `forceRestart` para minimizar el impacto en posteriores actualizaciones. 

## <a name="errors-and-troubleshooting"></a>Errores y solución de problemas

Si se produce un error en la implementación con el siguiente mensaje, significa que el clúster no se ejecuta en una versión de Service Fabric suficientemente alta:

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>Pasos siguientes
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el sistema](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el usuario](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Aprovechamiento de la identidad administrada de una aplicación de Service Fabric desde el código de servicio](./how-to-managed-identity-service-fabric-app-code.md)
* [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](./how-to-grant-access-other-resources.md)
