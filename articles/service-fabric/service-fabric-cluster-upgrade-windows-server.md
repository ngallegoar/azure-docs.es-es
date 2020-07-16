---
title: Actualización de la versión de un clúster independiente
description: Actualice el código de Azure Service Fabric que ejecuta un clúster de Service Fabric independiente.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 31712ce4f661b13802d9a0f2d798c1fe87fdebf3
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261008"
---
# <a name="upgrade-the-service-fabric-version-that-runs-on-your-cluster"></a>Actualización de la versión de Service Fabric que se ejecuta en el clúster 

En cualquier sistema moderno, la capacidad de actualización es clave para el éxito a largo plazo del producto. Un clúster de Azure Service Fabric es un recurso que usted posee. En este artículo se describe cómo actualizar la versión de Service Fabric que se ejecuta en el clúster independiente.

> [!NOTE]
> Asegúrese de que el clúster siempre ejecuta una versión compatible de Service Fabric. Cuando Microsoft anuncia el lanzamiento de una nueva versión de Service Fabric, se marca la versión anterior para que finalice el soporte después de un mínimo de 60 días a partir de la fecha del anuncio. Las versiones nuevas se anuncian [en el blog del equipo de Service Fabric](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric). La nueva versión estará disponible para su elección a partir de ese momento.
>
>

Puede actualizar el clúster a la nueva versión solo si usa una configuración de nodo de estilo de producción, donde cada nodo de Service Fabric se asigna a una máquina virtual o física independiente. Si tiene un clúster de desarrollo, donde hay más de un nodo de Service Fabric en una única máquina física o virtual, debe volver a crear el clúster con la nueva versión.

Hay dos flujos de trabajo distintos para actualizar el clúster a la versión más reciente o a una versión de Service Fabric compatible. Uno de los flujos es para los clústeres que tienen conectividad para descargar automáticamente la versión más reciente. El otro es para los clústeres que no tienen conectividad para descargar la versión más reciente de Service Fabric.

## <a name="enable-auto-upgrade-of-the-service-fabric-version-of-your-cluster"></a>Habilitación de la actualización automática de la versión de Service Fabric del clúster
Para establecer el clúster para que descargue actualizaciones de Service Fabric cuando Microsoft publica una nueva versión, establezca la configuración del clúster `fabricClusterAutoupgradeEnabled` en *true*. Para seleccionar manualmente una versión compatible de Service Fabric en la que quiera que esté el clúster, establezca la configuración del clúster `fabricClusterAutoupgradeEnabled` en *false*.

## <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Actualización de los clústeres con conectividad para descargar el código y la configuración más recientes
Siga estos pasos para actualizar el clúster a una versión compatible, si los nodos del clúster tienen conectividad a Internet al [Centro de descarga de Microsoft](https://download.microsoft.com).

Para los clústeres con conectividad al [Centro de descarga de Microsoft](https://download.microsoft.com), Microsoft comprueba periódicamente la disponibilidad de nuevas versiones de Service Fabric.

Cuando una nueva versión de Service Fabric está disponible, el paquete se descarga localmente en el clúster y se aprovisiona para la actualización. Además, para informar al cliente de esta nueva versión, el sistema muestra una advertencia de estado de clúster explícita similar a la siguiente:

"La compatibilidad con la versión [versión] de clúster actual finaliza el [Fecha]."

Después de que el clúster ejecuta la versión más reciente, se quita la advertencia.

Cuando vea la advertencia de mantenimiento del clúster, actualícelo:

1. Conéctese al clúster desde cualquier máquina con acceso de administrador a todas las máquinas que se muestran como nodos en el clúster. La máquina donde se ejecuta este script no tiene que formar parte del clúster.

    ```powershell
    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Obtenga la lista de versiones de Service Fabric a las que puede actualizar.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Debería obtener una salida similar a esta:

    ![Obtener versiones de Service Fabric][getfabversions]
3. Inicie una actualización de clúster a una versión disponible mediante el comando de Windows PowerShell [Start-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    ```powershell
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    ```
   Para supervisar el progreso de la actualización, puede usar Service Fabric Explorer o ejecutar el siguiente comando de PowerShell:

    ```powershell
    Get-ServiceFabricClusterUpgrade
    ```

    Si no se cumplen las directivas de mantenimiento del clúster, la actualización se revierte. Para especificar las directivas de mantenimiento personalizado para el comando Start-ServiceFabricClusterUpgrade consulte la documentación de [Start-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Después de corregir los problemas que provocaron la reversión, debe iniciar la actualización de nuevo, siguiendo los mismos pasos descritos anteriormente.

## <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Actualización de los clústeres *sin conectividad* para descargar el código y la configuración más recientes
Siga estos pasos para actualizar el clúster a una versión compatible, si los nodos del clúster no tienen conectividad a Internet al [Centro de descarga de Microsoft](https://download.microsoft.com).

> [!NOTE]
> Si está ejecutando un clúster que no está conectado a Internet, tendrá que supervisar el [blog del equipo de Service Fabric](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) para conocer más información acerca de nuevas versiones. El sistema no muestra ninguna advertencia de mantenimiento de clúster para informarle de una nueva versión.  
>
>

### <a name="auto-provisioning-vs-manual-provisioning"></a>Aprovisionamiento automático frente a aprovisionamiento manual
Para habilitar la descarga y el registro automáticos de la última versión del código, configure el servicio de actualización de Service Fabric. Consulte el archivo *Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt* en el [paquete independiente](service-fabric-cluster-standalone-package-contents.md).

Para el proceso manual, siga estas instrucciones.

Modifique la configuración del clúster para establecer la siguiente propiedad en *false* antes de iniciar una actualización de la configuración:

```json
"fabricClusterAutoupgradeEnabled": false,
```

Consulte el [comando Start-ServiceFabricClusterConfigurationUpgrade de PowerShell ](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) para conocer los detalles de uso. Asegúrese de actualizar 'clusterConfigurationVersion' en JSON antes de iniciar la actualización de la configuración.

```powershell
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

### <a name="cluster-upgrade-workflow"></a>Flujo de trabajo de actualización de clúster

1. Ejecute [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade) desde uno de los nodos del clúster y apunte el valor de *TargetCodeVersion*.

2. Ejecute lo siguiente en un equipo conectado a Internet para enumerar todas las versiones de actualización compatibles con la versión actual y descargar el paquete correspondiente de los vínculos de descarga asociados:

    ```powershell
    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Conéctese al clúster desde cualquier máquina con acceso de administrador a todas las máquinas que se muestran como nodos en el clúster. La máquina donde se ejecuta este script no tiene que formar parte del clúster.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"
    ```
4. Copie el paquete descargado en el almacén de imágenes del clúster.

5. Registre el paquete copiado.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab
    ```
6. Inicie una actualización del clúster a una versión disponible.

    ```powershell
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    ```
    Puede supervisar el progreso de la actualización en Service Fabric Explorer o puede ejecutar el siguiente comando de PowerShell:

    ```powershell
    Get-ServiceFabricClusterUpgrade
    ```

    Si no se cumplen las directivas de mantenimiento del clúster, la actualización se revierte. Para especificar las directivas de mantenimiento personalizado para el comando Start-ServiceFabricClusterUpgrade consulte la documentación de [Start-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Después de corregir los problemas que provocaron la reversión, debe iniciar la actualización de nuevo, siguiendo los mismos pasos descritos anteriormente.

## <a name="next-steps"></a>Pasos siguientes
* [Actualización de la configuración de un clúster independiente](service-fabric-cluster-config-upgrade-windows-server.md)
* Personalización de algunas de las opciones de [configuración del clúster de Service Fabric](service-fabric-cluster-fabric-settings.md)
* [Escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-in-out.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
