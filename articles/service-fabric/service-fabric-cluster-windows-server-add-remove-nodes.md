---
title: Adición o eliminación de nodos de un clúster de Service Fabric independiente
description: Obtenga información sobre cómo agregar o quitar nodos en un clúster de Azure Service Fabric en una máquina virtual o física con Windows Server, la que podría estar en el entorno local o en alguna nube.
author: dkkapur
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: f9bee35ee8e82070b4cf601139b471562ba5e10b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934206"
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Incorporación o eliminación de nodos de un clúster de Service Fabric independiente con Windows Server
Una vez que [cree su clúster de Service Fabric independiente en máquinas con Windows Server](service-fabric-cluster-creation-for-windows-server.md), puede que las necesidades (empresariales) cambien y que deba agregar o eliminar nodos del clúster. En este artículo, se muestran los pasos detallados para llevarlo a cabo. Tenga en cuenta que no se permite agregar o eliminar nodos en los clústeres de desarrollo local.

## <a name="add-nodes-to-your-cluster"></a>Incorporación de nodos al clúster

1. Prepare la máquina virtual o el equipo que quiera agregar al clúster; para ello, siga los pasos descritos en [Planeamiento y preparación de la implementación de un clúster de Service Fabric](service-fabric-cluster-standalone-deployment-preparation.md).

2. Identifique a qué dominio de error y de actualización se va a agregar este equipo o esta máquina virtual.

   Si usa certificados para proteger el clúster, se espera que estén instalados en los almacenes de certificados locales para preparar el nodo para unirse al clúster. Lo mismo se aplica cuando se usan otras formas de seguridad.

3. Abra una conexión de Escritorio remoto (RDP) en la máquina o VM que desea agregar al clúster.

4. Copie o [descargue el paquete independiente de Service Fabric para Windows Server](https://go.microsoft.com/fwlink/?LinkId=730690) en el equipo o la máquina virtual, y descomprímalo.

5. Ejecute PowerShell con privilegios elevados y vaya a la ubicación del paquete descomprimido.

6. Ejecute el script *AddNode.ps1* con los parámetros que describen el nuevo nodo que se va a agregar. En el ejemplo siguiente se agrega un nuevo nodo denominado VM5, con el tipo NodeType0 y la dirección IP 182.17.34.52, en UD1 y fd:/dc1/r0. `ExistingClusterConnectionEndPoint` es un punto de conexión para un nodo que ya está presente en el clúster existente, y que puede ser la dirección IP de *cualquier* nodo del clúster. 

   No seguro (prototipos):

   ```
   .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
   ```

   Seguro (basado en certificados):

   ```  
   $CertThumbprint= "***********************"
    
   .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -X509Credential -ServerCertThumbprint $CertThumbprint  -AcceptEULA

   ```

   Cuando finalice la ejecución del script, puede comprobar si se ha agregado el nuevo nodo mediante la ejecución del cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps).

7. Para garantizar la coherencia entre los distintos nodos del clúster, debe iniciar una actualización de la configuración. Ejecute [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) para obtener el archivo de configuración más reciente y agregar el nodo recién agregado a la sección "Nodes". También se recomienda tener siempre la configuración del clúster más reciente disponible en caso de que deba volver a implementar un clúster que tenga la misma configuración.

   ```
    {
        "nodeName": "vm5",
        "iPAddress": "182.17.34.52",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD1"
    }
   ```

8. Ejecute [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) para comenzar la actualización.

   ```
   Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
   ```

   Puede supervisar el progreso de la actualización en Service Fabric Explorer. Como alternativa, puede ejecutar [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps).

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>Agregar nodos a clústeres configurados con seguridad de Windows mediante gMSA
En clústeres configurados con cuentas de servicio administradas de grupo (gMSA) (https://technet.microsoft.com/library/hh831782.aspx) ), se puede agregar un nuevo nodo mediante una actualización de configuración:
1. Ejecute [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) en cualquiera de los nodos existentes para obtener el archivo de configuración más reciente y agregue los detalles del nodo que desea agregar en la sección "Nodes". Asegúrese de que el nuevo nodo forma parte de la misma cuenta administrada de grupo. Esta cuenta debe ser un administrador en todos los equipos.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Ejecute [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) para comenzar la actualización.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    Puede supervisar el progreso de la actualización en Service Fabric Explorer. Como alternativa, puede ejecutar [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-to-your-cluster"></a>Agregar tipos de nodo al clúster
Para agregar un nuevo tipo de nodo, modifique la configuración para incluir el nuevo tipo de nodo en la sección "NodeTypes" en "Properties" e inicie una actualización de configuración con [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Una vez completada la actualización, puede agregar nuevos nodos al clúster con este tipo de nodo.

## <a name="remove-nodes-from-your-cluster"></a>Eliminación de nodos del clúster
Es posible eliminar un nodo de un clúster con una actualización de la configuración de la siguiente manera:

1. Ejecute [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) para obtener el archivo de configuración más reciente y *elimine* el nodo de la sección "Nodes".
Agregue el parámetro "NodesToBeRemoved" a la sección "Setup" dentro de la sección "FabricSettings". El "valor" debe ser una lista separada por comas de nombres de nodo de los nodos que deben eliminarse.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Ejecute [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) para comenzar la actualización.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    Puede supervisar el progreso de la actualización en Service Fabric Explorer. Como alternativa, puede ejecutar [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps).

> [!NOTE]
> La eliminación de nodos puede iniciar varias actualizaciones. Algunos nodos están marcados con la etiqueta `IsSeedNode=”true”` y se pueden identificar consultando el manifiesto del clúster mediante `Get-ServiceFabricClusterManifest`. La eliminación de estos nodos puede tardar más que la de otros, ya que se tendrán que mover los nodos de inicialización en estos escenarios. El clúster debe tener un mínimo de 3 nodos de tipo nodo primario.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Eliminación de tipos de nodo del clúster
Antes de eliminar un tipo de nodo, compruebe de nuevo si no hay algún nodo que haga referencia a dicho tipo. Elimine estos nodos antes de quitar el tipo de nodo correspondiente. Una vez se han eliminado todos los nodos correspondientes, puede eliminar el valor de NodeType de la configuración del clúster y comenzar una actualización de la configuración mediante [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Sustitución de los nodos principales del clúster
Los nodos principales se deben sustituir uno por uno, en lugar de quitarlos y agregarlos de forma masiva.


## <a name="next-steps"></a>Pasos siguientes
* [Opciones de configuración de clústeres de Windows independientes](service-fabric-cluster-manifest.md)
* [Protección de un clúster de Windows independiente mediante certificados](service-fabric-windows-cluster-x509-security.md)
* [Creación de un clúster de Service Fabric independiente con máquinas virtuales de Azure con Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)

