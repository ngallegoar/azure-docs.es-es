---
title: Escalado vertical de un tipo de nodo de Azure Service Fabric
description: Aprenda a escalar un clúster de Service Fabric mediante la adición de un conjunto de escalado de máquinas virtuales.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: 2d700367049e0bf9bf710aad110c850a78c26220
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610700"
---
# <a name="scale-up-a-service-fabric-cluster-primary-node-type"></a>Escalado vertical del tipo de nodo principal de un clúster de Service Fabric
En este artículo, se explica cómo se escala verticalmente el tipo de nodo principal de un clúster Service Fabric aumentando los recursos de las máquinas virtuales. Un clúster de Service Fabric es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios. Un equipo o máquina virtual que forma parte de un clúster se denomina nodo. Los conjuntos de escalado de máquinas virtuales son un recurso de proceso de Azure que se puede usar para implementar y administrar una colección de máquinas virtuales de forma conjunta. Cada tipo de nodo que se define en un clúster de Azure está [configurado como un conjunto de escalado independiente](service-fabric-cluster-nodetypes.md). Cada tipo de nodo, a continuación, se puede administrar por separado. Después de crear un clúster de Service Fabric, puede escalar el tipo de nodo del clúster verticalmente (cambiar los recursos de los nodos) o actualizar el sistema operativo de las máquinas virtuales del tipo de nodo.  Puede escalar el clúster en cualquier momento, incluso con cargas de trabajo en ejecución en el clúster.  Según se escala el clúster, las aplicaciones se escalan automáticamente.

> [!WARNING]
> No intente un procedimiento de escalado vertical de tipo de nodo principal si el estado del clúster es incorrecto, ya que esto solo desestabilizará el clúster.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="process-to-upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>Proceso de actualización del tamaño y el sistema operativo de las VM del tipo de nodo principal
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

## <a name="set-up-the-test-cluster"></a>Configuración del clúster de prueba

Para empezar, descargue los dos conjuntos de archivos que se necesitarán para este tutorial, la [plantilla]() y los [parámetros]() de antes y la [plantilla]() y los [parámetros]() de después.

A continuación, inicie sesión en la cuenta de Azure.

```powershell
# sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId "<your subscription ID>"
```

Este tutorial le guía por el escenario de creación de un certificado autofirmado. Para usar un certificado existente de Azure Key Vault, omita el paso siguiente y, en su lugar, refleje los pasos descritos en [Uso de un certificado existente para implementar el clúster](https://docs.microsoft.com/azure/service-fabric/upgrade-managed-disks#use-an-existing-certificate-to-deploy-the-cluster).

### <a name="generate-a-self-signed-certificate-and-deploy-the-cluster"></a>Generación de un certificado autofirmado e implementación del clúster

En primer lugar, asigne las variables que necesitará para la implementación del clúster de Service Fabric. Ajuste los valores de `resourceGroupName`, `certSubjectName`, `parameterFilePath` y `templateFilePath` para su entorno y cuenta específicos:

```powershell
# Assign deployment variables
$resourceGroupName = "sftestupgradegroup"
$certOutputFolder = "c:\certificates"
$certPassword = "Password!1" | ConvertTo-SecureString -AsPlainText -Force
$certSubjectName = "sftestupgrade.southcentralus.cloudapp.azure.com"
$templateFilePath = "C:\Deploy-2NodeTypes-2ScaleSets.json"
$parameterFilePath = "C:\Deploy-2NodeTypes-2ScaleSets.parameters.json"
```

> [!NOTE]
> Asegúrese de que la ubicación `certOutputFolder` existe en el equipo local antes de ejecutar el comando para implementar un nuevo clúster de Service Fabric.

A continuación, abra el archivo *Deploy-2NodeTypes-2ScaleSets.parameters.json* y ajuste los valores para `clusterName` y `dnsName` para que se correspondan con los valores dinámicos que establezca en PowerShell, y guarde los cambios.

A continuación, implemente el clúster de prueba de Service Fabric:

```powershell
# Deploy the initial test cluster
New-AzServiceFabricCluster `
    -ResourceGroupName $resourceGroupName `
    -CertificateOutputFolder $certOutputFolder `
    -CertificatePassword $certPassword `
    -CertificateSubjectName $certSubjectName `
    -TemplateFile $templateFilePath `
    -ParameterFile $parameterFilePath
```

Una vez finalizada la implementación, busque el archivo *.pfx* (`$certPfx`) en la máquina local e impórtelo al almacén de certificados:

```powershell
cd c:\certificates
$certPfx = ".\sftestupgradegroup20200312121003.pfx"

Import-PfxCertificate `
     -FilePath $certPfx `
     -CertStoreLocation Cert:\CurrentUser\My `
     -Password (ConvertTo-SecureString Password!1 -AsPlainText -Force)
```

La operación devolverá la huella digital del certificado, que usará para conectarse al nuevo clúster y comprobar su estado de mantenimiento.

### <a name="connect-to-the-new-cluster-and-check-health-status"></a>Conexión al nuevo clúster y comprobación del estado de mantenimiento

Conéctese al clúster y asegúrese de que todos los nodos tengan un estado correcto (reemplazando las variables `clusterName` y `thumb` del clúster):

```powershell
# Connect to the cluster
$clusterName = "sftestupgrade.southcentralus.cloudapp.azure.com:19000"
$thumb = "BB796AA33BD9767E7DA27FE5182CF8FDEE714A70"

Connect-ServiceFabricCluster `
    -ConnectionEndpoint $clusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My

# Check cluster health
Get-ServiceFabricClusterHealth
```

Estamos listos para comenzar el procedimiento de actualización.

## <a name="upgrade-the-primary-node-type-vms"></a>Actualización de las VM del tipo de nodo principal

Después de decidir actualizar las VM del tipo de nodo principal, agregue un nuevo conjunto de escalado al tipo de nodo principal de modo que el tipo de nodo principal tenga ahora dos conjuntos de escalado. Se han proporcionado archivos [plantilla](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) y [parámetros](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) de ejemplo para mostrar los cambios necesarios. Las VM del nuevo conjunto de escalado son de tamaño D4_V2 Estándar y ejecutan Windows Server 2016 Datacenter con contenedores. Se agregan también un nuevo equilibrador de carga y una dirección IP pública con el nuevo conjunto de escalado. 

Para buscar el nuevo conjunto de escalado en la plantilla, busque el recurso "Microsoft.Compute/virtualMachineScaleSets" nombrado por el parámetro vmNodeType2Name. El nuevo conjunto de escalado se agrega al tipo de nodo principal con el valor properties->virtualMachineProfile->extensionProfile->extensions->properties->settings->nodeTypeRef.

### <a name="deploy-the-updated-template"></a>Implementar la plantilla actualizada

Ajuste `parameterFilePath` y `templateFilePath` según sea necesario y, a continuación, ejecute el comando siguiente:

```powershell
# Deploy the new scale set into the primary node type along with a new load balancer and public IP
$templateFilePath = "C:\Deploy-2NodeTypes-3ScaleSets.json"
$parameterFilePath = "C:\Deploy-2NodeTypes-3ScaleSets.parameters.json"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $templateFilePath `
    -TemplateParameterFile $parameterFilePath `
    -CertificateThumbprint $thumb `
    -CertificateUrlValue $certUrlValue `
    -SourceVaultValue $sourceVaultValue `
    -Verbose
```

Una vez finalizada la implementación, vuelva a comprobar el estado del clúster y asegúrese de que todos los nodos (del conjunto de escalado original y del nuevo) tengan un estado correcto.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="migrate-nodes-to-the-new-scale-set"></a>Migración de nodos al nuevo conjunto de escalado

Estamos preparados para empezar a deshabilitar los nodos del conjunto de escalado original. A medida que estos nodos se inhabilitan, los servicios del sistema y los nodos de inicialización se migran a las VM del nuevo conjunto de escalado porque también está marcado como tipo de nodo principal.

Para el escalado vertical de tipos de nodo no principales, en este paso se modificaría la restricción de selección de ubicación del servicio para incluir el nuevo tipo de nodo o conjunto de escalado de máquinas virtuales y luego, se reducirá el número de instancias del conjunto de escalado de máquinas virtuales antiguo a cero, un nodo a la vez (para asegurarse de que la eliminación de nodos no afecte la confiabilidad del clúster).

```powershell
# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Use Service Fabric Explorer para supervisar la migración de los nodos de inicialización al nuevo conjunto de escalado y la progresión de los nodos del conjunto de escalado original del estado *Desactivando* a *Deshabilitado*.

> [!NOTE]
> La operación de deshabilitación puede tardar algún tiempo en completarse en todos los nodos del conjunto de escalado original. Para garantizar la coherencia de los datos, solo se puede cambiar un nodo de inicialización a la vez. Cada cambio de nodo de inicialización requiere una actualización de clúster. Por lo tanto, para reemplazar un nodo de inicialización, se requieren dos actualizaciones de clúster (para la adición y eliminación de nodos). La actualización de los cinco nodos de inicialización en este escenario de muestra dará como resultado diez actualizaciones de clúster.

## <a name="remove-the-original-scale-set"></a>Quitar el conjunto de escalado original

Una vez completada la operación de deshabilitación, quite el conjunto de escalado.

```powershell
# Remove the original scale set
$scaleSetName = "NTvm1"

Remove-AzVmss `
    -ResourceGroupName $resourceGroupName `
    -VMScaleSetName $scaleSetName `
    -Force

Write-Host "Removed scale set $scaleSetName"
```

En Service Fabric Explorer, los nodos quitados (y, por tanto, el *estado de mantenimiento del clúster*) presentarán ahora un estado de *error*.

## <a name="remove-the-old-load-balancer-and-update-dns-settings"></a>Eliminación el equilibrador de carga anterior y actualización de la configuración de DNS

Ahora, podemos quitar los recursos relacionados con el tipo de nodo principal anterior, empezando por el equilibrador de carga y la antigua dirección IP pública. 

```powershell
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
```

A continuación, actualizamos la configuración de DNS de la nueva dirección IP pública para reflejar la configuración de la dirección IP pública del tipo de nodo principal anterior.

```powershell
# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzPublicIpAddress -PublicIpAddress $PublicIP
```

Una vez más, compruebe el estado del clúster.

```powershell
# Check the cluster health
Get-ServiceFabricClusterHealth
```

Por último, quite el estado de nodo de cada uno de los nodos relacionados. Si el nivel de durabilidad del conjunto de escalado anterior era Silver o Gold, esto ocurrirá automáticamente.

```powershell
# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

Ahora se ha actualizado el tipo de nodo principal del clúster. Verifique que las aplicaciones implementadas funcionen correctamente y que el estado del clúster sea correcto.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre la [incorporación de un tipo de nodo a un clúster](virtual-machine-scale-set-scale-node-type-scale-out.md)
* Obtenga información sobre [escalabilidad de aplicaciones](service-fabric-concepts-scalability.md).
* [Escalado horizontal de un clúster de Azure](service-fabric-tutorial-scale-cluster.md).
* [Escalado de un clúster de Azure mediante programación](service-fabric-cluster-programmatic-scaling.md) con el SDK de proceso de Azure.
* [Escalado o reducción horizontal de un clúster independiente](service-fabric-cluster-windows-server-add-remove-nodes.md)

