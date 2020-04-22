---
title: Escalado vertical de un tipo de nodo de Azure Service Fabric
description: Aprenda a escalar un clúster de Service Fabric mediante la adición de un conjunto de escalado de máquinas virtuales.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: 4dbb9e4fbfeb27c5b8b13f70207888cf37bbb0e0
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998938"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Escalado vertical del tipo de nodo principal de un clúster de Service Fabric
En este artículo, se explica cómo se escala verticalmente el tipo de nodo principal de un clúster Service Fabric aumentando los recursos de las máquinas virtuales. Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Un equipo o máquina virtual que forma parte de un clúster se denomina nodo. Los conjuntos de escalado de máquinas virtuales son un recurso de proceso de Azure que se puede usar para implementar y administrar una colección de máquinas virtuales de forma conjunta. Cada tipo de nodo que se define en un clúster de Azure está [configurado como un conjunto de escalado independiente](service-fabric-cluster-nodetypes.md). Cada tipo de nodo, a continuación, se puede administrar por separado. Después de crear un clúster de Service Fabric, puede escalar el tipo de nodo del clúster verticalmente (cambiar los recursos de los nodos) o actualizar el sistema operativo de las máquinas virtuales del tipo de nodo.  Puede escalar el clúster en cualquier momento, incluso con cargas de trabajo en ejecución en el clúster.  Según se escala el clúster, las aplicaciones se escalan automáticamente.

> [!WARNING]
> No comience cambiando el tipo de nodo principal VM SKU, si el estado del clúster no es bueno. En este caso, si intenta cambiar la SKU de las máquinas virtuales, solo desestabilizará aún más el clúster.
>
> Se recomienda no cambiar la SKU de las máquinas virtuales de un tipo de nodo o conjunto de escalado a menos que se esté ejecutando en la [durabilidad Silver o mayor](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Modificar el tamaño de la SKU de VM constituye una operación de infraestructura local de destrucción de datos. Sin la posibilidad de retrasar o supervisar este cambio, es posible que la operación pueda provocar una pérdida de datos en los servicios con estado o bien causar otros problemas de funcionamiento imprevistos, incluso en las cargas de trabajo sin estado. Esto significa el tipo de nodo principal, que ejecuta servicios del sistema de Service Fabric con estado, o cualquier tipo de nodo que ejecute sus cargas de trabajo de aplicaciones con estado.
>


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Actualización del tamaño y el sistema operativo de las máquinas virtuales del tipo de nodo principal
Este es el proceso para actualizar el tamaño de máquina virtual y el sistema operativo de las máquinas virtuales del tipo de nodo principal.  Después de la actualización, las máquinas virtuales del tipo de nodo principal son de tamaño D4_V2 Estándar y ejecutan Windows Server 2016 Datacenter con contenedores.

> [!WARNING]
> Antes de intentar este procedimiento en un clúster de producción, se recomienda que revise atentamente las plantillas de ejemplo y compruebe el proceso en un clúster de prueba. Además, el clúster no está disponible durante un tiempo. NO puede hacer cambios en varias VMSS declaradas con el mismo NodeType en paralelo; deberá realizar operaciones de implementación aparte para aplicar cambios a cada VMSS NodeType individualmente.

1. Implemente el clúster inicial con dos tipos de nodos y dos conjuntos de escalado (un conjunto de escalado por tipo de nodo) mediante estos archivos de [plantilla](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) y [parámetros](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) de ejemplo.  Ambos conjuntos de escalado son de tamaño D2_V2 Estándar y ejecutan Windows Server 2012 R2 Datacenter.  Espere a que el clúster finalice la actualización de referencia.   
2. Opcional: implementación de un ejemplo con estado en el clúster.
3. Después de decidir actualizar las máquinas virtuales del tipo de nodo principal, agregue un nuevo conjunto de escalado al tipo de nodo principal con estos archivos de [plantilla](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) y [parámetros](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) de ejemplo para que el tipo de nodo principal tenga ahora dos conjuntos de escalado.  Los servicios del sistema y las aplicaciones de usuario se pueden migrar entre las máquinas virtuales de los dos conjuntos de escalado distintos.  Las máquinas virtuales del nuevo conjunto de escalado son de tamaño D4_V2 Estándar y ejecutan Windows Server 2016 Datacenter con contenedores.  Se agregan también un nuevo equilibrador de carga y una dirección IP pública con el nuevo conjunto de escalado.  
    Para buscar el nuevo conjunto de escalado en la plantilla, busque el recurso "Microsoft.Compute/virtualMachineScaleSets" nombrado por el parámetro *vmNodeType2Name*.  El nuevo conjunto de escalado se agrega al tipo de nodo principal con el valor properties->virtualMachineProfile->extensionProfile->extensions->properties->settings->nodeTypeRef.
4. Compruebe el mantenimiento del clúster y que todos los nodos están en buen estado.
5. Deshabilite los nodos del conjunto de escalado anterior del tipo de nodo principal con la intención de eliminar el nodo. Puede deshabilitar todos los nodos a la vez y las operaciones se pondrán en cola. Espere hasta que todos los nodos estén deshabilitados, lo que puede llevar algún tiempo.  Como los nodos anteriores del tipo de nodo están deshabilitados, los servicios del sistema y los nodos de inicialización migran a las máquinas virtuales del nuevo conjunto de escalado en el tipo de nodo principal.
6. Elimine el conjunto de escalado anterior del tipo de nodo principal. (Una vez deshabilitados los nodos como en el paso 5, en la hoja del conjunto de escalado de máquinas virtuales de Azure Portal, desasigne los nodos del tipo de nodo anterior uno a uno).
7. Elimine el equilibrador de carga asociado al conjunto de escalado anterior. El clúster no está disponible mientras que la nueva dirección IP pública y el equilibrador de carga se configuran para el nuevo conjunto de escalado.  
8. Almacene la configuración DNS de la dirección IP pública asociada al conjunto de escalado del tipo de nodo principal anterior en una variable y elimine esa dirección IP pública.
9. Reemplace la configuración DNS de la dirección IP pública asociada al conjunto de escalado del tipo de nodo principal nuevo por la configuración DNS de la dirección IP pública eliminada.  El clúster ahora está accesible de nuevo.
10. Elimine el estado de nodo de los nodos del clúster.  Si el nivel de durabilidad del conjunto de escalado anterior era Silver o Gold, el sistema realiza automáticamente este paso.
11. Si ha implementado la aplicación con estado en un paso anterior, compruebe que la aplicación está operativa.

```powershell
# Variables.
$groupname = "sfupgradetestgroup"
$clusterloc="southcentralus"  
$subscriptionID="<your subscription ID>"

# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $subscriptionID 

# Create a new resource group for your deployment and give it a name and a location.
New-AzResourceGroup -Name $groupname -Location $clusterloc

# Deploy the two node type cluster.
New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.json" -Verbose

# Connect to the cluster and check the cluster health.
$ClusterName= "sfupgradetest.southcentralus.cloudapp.azure.com:19000"
$thumb="F361720F4BD5449F6F083DDE99DC51A86985B25B"

Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Get-ServiceFabricClusterHealth

# Deploy a new scale set into the primary node type.  Create a new load balancer and public IP address for the new scale set.
New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.json" -Verbose

# Check the cluster health again. All 15 nodes should be healthy.
Get-ServiceFabricClusterHealth

# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}

Write-Host "Checking node status..."
foreach($name in $nodeNames){
 
    $state = Get-ServiceFabricNode -NodeName $name 

    $loopTimeout = 50

    do{
        Start-Sleep 5
        $loopTimeout -= 1
        $state = Get-ServiceFabricNode -NodeName $name
        Write-Host "$name state: " $state.NodeDeactivationInfo.Status
    }

    while (($state.NodeDeactivationInfo.Status -ne "Completed") -and ($loopTimeout -ne 0))
    

    if ($state.NodeStatus -ne [System.Fabric.Query.NodeStatus]::Disabled)
    {
        Write-Error "$name node deactivation failed with state" $state.NodeStatus
        exit
    }
}

# Remove the scale set
$scaleSetName="NTvm1"
Remove-AzVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force
Write-Host "Removed scale set $scaleSetName"

$lbname="LB-sfupgradetest-NTvm1"
$oldPublicIpName="PublicIP-LB-FE-0"
$newPublicIpName="PublicIP-LB-FE-2"

# Store DNS settings of public IP address related to old Primary NodeType into variable 
$oldprimaryPublicIP = Get-AzPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname

$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel

$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

# Remove Load Balancer related to old Primary NodeType. This will cause a brief period of downtime for the cluster
Remove-AzLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force

# Remove the old public IP
Remove-AzPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP

# Check the cluster health
Get-ServiceFabricClusterHealth

# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre la [incorporación de un tipo de nodo a un clúster](virtual-machine-scale-set-scale-node-type-scale-out.md)
* Obtenga información sobre [escalabilidad de aplicaciones](service-fabric-concepts-scalability.md).
* [Escalado horizontal de un clúster de Azure](service-fabric-tutorial-scale-cluster.md).
* [Escalado de un clúster de Azure mediante programación](service-fabric-cluster-programmatic-scaling.md) con el SDK de proceso de Azure.
* [Escalado o reducción horizontal de un clúster independiente](service-fabric-cluster-windows-server-add-remove-nodes.md)

