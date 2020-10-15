---
title: Implementar una aplicación de Service Fabric con una instancia administrada asignada por el sistema
description: En este artículo se muestra cómo asignar una identidad administrada asignada por el sistema a una aplicación de Azure Service Fabric.
ms.topic: article
ms.date: 07/25/2019
ms.openlocfilehash: c5c7a17c51eee18d9b7276f2c57289a5de5c8181
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81415651"
---
# <a name="deploy-service-fabric-application-with-system-assigned-managed-identity"></a>Implementación de una aplicación de Service Fabric con una identidad administrada asignada por el sistema

Para acceder a la característica de identidad administrada para las aplicaciones de Azure Service Fabric, primero debe habilitar el servicio de token de identidad administrada en el clúster. Este servicio es responsable de la autenticación de aplicaciones de Service Fabric que usan sus identidades administradas y de la obtención de los tokens de acceso en su nombre. Una vez habilitado el servicio, puede verlo en Service Fabric Explorer, bajo la sección **Sistema** del panel izquierdo. Se ejecuta con el nombre **fabric:/System/ManagedIdentityTokenService** junto a otros servicios del sistema.

> [!NOTE] 
> La implementación de aplicaciones de Service Fabric con identidades administradas se admite a partir de la versión de API `"2019-06-01-preview"`. También puede usar la misma versión de API para el tipo de aplicación, la versión del tipo de aplicación y los recursos de servicio. La versión mínima admitida del runtime de Service Fabric es 6.5 CU2. Además, el entorno o paquete de compilación también debe tener el SDK de .NET para SF en CU2 o superior.

## <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

### <a name="application-template"></a>Plantilla de la aplicación

Para habilitar una aplicación con una identidad administrada asignada por el sistema, agregue la propiedad **identity** al recurso de aplicación, con el tipo **systemAssigned** como se muestra en el ejemplo siguiente:

```json
    {
      "apiVersion": "2019-06-01-preview",
      "type": "Microsoft.ServiceFabric/clusters/applications",
      "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
      ],
      "identity": {
        "type" : "systemAssigned"
      },
      "properties": {
        "typeName": "[parameters('applicationTypeName')]",
        "typeVersion": "[parameters('applicationTypeVersion')]",
        "parameters": {
        }
      }
    }
```
Esta propiedad declara (para Azure Resource Manager y los proveedores de recursos de identidad administrada y de Service Fabric, respectivamente) que este recurso debe tener una identidad administrada (`system assigned`) implícita.

### <a name="application-and-service-package"></a>Aplicación y paquete de servicio

1. Actualice el manifiesto de aplicación para agregar un elemento **ManagedIdentity** en la sección **Principals** que contenga una sola entrada, como se muestra a continuación:

    **ApplicationManifest.xml**

    ```xml
    <Principals>
      <ManagedIdentities>
        <ManagedIdentity Name="SystemAssigned" />
      </ManagedIdentities>
    </Principals>
    ```
    De este modo, la identidad asignada se asigna a la aplicación como un recurso para un nombre descriptivo, para la posterior asignación a los servicios que componen la aplicación. 

2. En la sección **ServiceManifestImport** correspondiente al servicio al que se está asignando la identidad administrada, agregue un elemento **IdentityBindingPolicy**, como se indica a continuación:

    **ApplicationManifest.xml**

      ```xml
        <ServiceManifestImport>
          <Policies>
            <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="SystemAssigned" />
          </Policies>
        </ServiceManifestImport>
      ```

    Este elemento asigna la identidad de la aplicación al servicio; sin esta asignación, el servicio no podrá acceder a la identidad de la aplicación. En el fragmento de código anterior, la identidad `SystemAssigned` (que es una palabra clave reservada) se asigna a la definición del servicio con el nombre descriptivo `WebAdmin`.

3. Actualice el manifiesto de servicio para agregar un elemento **ManagedIdentity** en la sección **Resources** con el nombre que coincide con el valor de la opción `ServiceIdentityRef` en la definición `IdentityBindingPolicy` del manifiesto de aplicación:

    **ServiceManifest.xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```
    Esta es la asignación equivalente de una identidad a un servicio, tal como se ha descrito anteriormente, pero desde la perspectiva de la definición del servicio. Aquí se hace referencia a la identidad mediante su nombre descriptivo (`WebAdmin`), como se declaró en el manifiesto de aplicación.

## <a name="next-steps"></a>Pasos siguientes
* Revisión de la [compatibilidad con la identidad administrada](./concepts-managed-identity.md) en Azure Service Fabric
* [Implementación de un nuevo](./configure-new-azure-service-fabric-enable-managed-identity.md) clúster de Azure Service Fabric con compatibilidad con la identidad administrada 
* [Habilitación de la identidad administrada](./configure-existing-cluster-enable-managed-identity-token-service.md) en un clúster de Azure Service Fabric existente
* Aprovechamiento de la [identidad administrada de una aplicación de Service Fabric desde el código fuente](./how-to-managed-identity-service-fabric-app-code.md)
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el usuario](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](./how-to-grant-access-other-resources.md)
