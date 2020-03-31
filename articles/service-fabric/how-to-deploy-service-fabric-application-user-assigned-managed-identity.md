---
title: Creación de una identidad administrada asignada por el usuario
description: En este artículo se muestra cómo implementar una aplicación de Service Fabric con una identidad administrada asignada por el usuario.
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: a5eeaf0d6420fa36c0a78f7553ddfd82197d8ec4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610342"
---
# <a name="deploy-service-fabric-application-with-a-user-assigned-managed-identity-preview"></a>Implementación de una aplicación de Service Fabric con una identidad administrada asignada por el usuario (versión preliminar)

Para implementar una aplicación de Service Fabric con una identidad administrada, la aplicación debe implementarse a través de Azure Resource Manager, normalmente con una plantilla de Azure Resource Manager. Para obtener más información sobre cómo implementar la aplicación de Service Fabric a través de Azure Resource Manager, vea [Administración de aplicaciones y servicios como recursos de Azure Resource Manager](service-fabric-application-arm-resource.md).

> [!NOTE] 
> 
> Las aplicaciones que no se implementan como un recurso de Azure **no pueden** tener identidades administradas. 
>
> La implementación de la aplicación de Service Fabric con identidad administrada es compatible con la versión `"2019-06-01-preview"` de la API. También puede usar la misma versión de API para el tipo de aplicación, la versión del tipo de aplicación y los recursos de servicio.
>

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

Para habilitar la aplicación con una identidad asignada por el usuario, agregue primero la propiedad de **identidad** al recurso de aplicación de tipo **userAssigned** y las identidades asignadas por el usuario a las que se hace referencia. A continuación, agregue una sección **managedIdentities** dentro de la sección **properties** del recurso **application** que contiene una lista de nombres descriptivos para la asignación del objeto principalId para cada una de las identidades asignadas por el usuario. Para más información sobre las identidades asignadas por el usuario, consulte [Creación, enumeración o eliminación de una identidad administrada asignada por el usuario](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell).

### <a name="application-template"></a>Plantilla de la aplicación

Para habilitar la aplicación con la identidad asignada por el usuario, primero agregue la propiedad **identity** al recurso de la aplicación con el tipo **userAssigned** y las identidades asignadas por el usuario a las que se hace referencia y, después, agregue un objeto **managedIdentities** dentro de la sección **properties** que contiene una lista de nombres descriptivos para la asignación del objeto principalId para cada una de las identidades asignadas por el usuario.

    {
      "apiVersion": "2019-06-01-preview",
      "type": "Microsoft.ServiceFabric/clusters/applications",
      "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]",
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]"
      ],
      "identity": {
        "type" : "userAssigned",
        "userAssignedIdentities": {
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]": {}
        }
      },
      "properties": {
        "typeName": "[parameters('applicationTypeName')]",
        "typeVersion": "[parameters('applicationTypeVersion')]",
        "parameters": {
        },
        "managedIdentities": [
          {
            "name" : "[parameters('userAssignedIdentityName')]",
            "principalId" : "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName')), '2018-11-30').principalId]"
          }
        ]
      }
    }

En el ejemplo anterior, se usa el nombre del recurso de la identidad asignada por el usuario como nombre descriptivo de la identidad administrada para la aplicación. En los ejemplos siguientes se da por hecho que el nombre descriptivo real es "AdminUser".

### <a name="application-package"></a>Paquete de aplicación

1. Para cada identidad definida en la sección `managedIdentities` de la plantilla de Azure Resource Manager, agregue una etiqueta `<ManagedIdentity>` en el manifiesto de aplicación, en la sección **Principals**. El atributo `Name` debe coincidir con la propiedad `name` definida en la sección `managedIdentities`.

    **ApplicationManifest.xml**

    ```xml
      <Principals>
        <ManagedIdentities>
          <ManagedIdentity Name="AdminUser" />
        </ManagedIdentities>
      </Principals>
    ```

2. En la sección **ServiceManifestImport**, agregue **IdentityBindingPolicy** para el servicio que usa la identidad administrada. Esta directiva asigna la identidad `AdminUser` a un nombre de identidad específico del servicio que se debe agregar en el manifiesto de servicio más adelante.

    **ApplicationManifest.xml**

    ```xml
      <ServiceManifestImport>
        <Policies>
          <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="AdminUser" />
        </Policies>
      </ServiceManifestImport>
    ```

3. Actualice el manifiesto de servicio para agregar **ManagedIdentity** dentro de la sección **Resources** con el nombre que coincide con `ServiceIdentityRef` en `IdentityBindingPolicy` del manifiesto de aplicación:

    **ServiceManifest.xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Cómo usar la identidad administrada en el código de la aplicación de Service Fabric](how-to-managed-identity-service-fabric-app-code.md)
* [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](how-to-grant-access-other-resources.md)
