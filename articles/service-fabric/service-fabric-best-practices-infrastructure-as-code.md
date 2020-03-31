---
title: Procedimientos recomendados de infraestructura como código de Azure Service Fabric
description: Procedimientos recomendados y consideraciones de diseño para administrar la infraestructura de Azure Service Fabric como código.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 1c044d5fd973d3c577088a887f2fac413d2ab79d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551835"
---
# <a name="infrastructure-as-code"></a>Infraestructura como código

En un escenario de producción, cree clústeres de Azure Service Fabric con plantillas de Resource Manager. Las plantillas de Resource Manager proporcionan un mayor control de las propiedades de los recursos y garantizan que disponga de un modelo coherente de recursos.

Las plantillas de ejemplo de Resource Manager están disponibles para Windows y Linux en los [ejemplos de Azure de GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Estas plantillas se pueden usar como punto de partida para crear la plantilla de clúster. Descargue las plantillas `azuredeploy.json` y `azuredeploy.parameters.json` y modifíquelas para satisfacer sus requisitos personalizados.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para implementar las plantillas `azuredeploy.json` y `azuredeploy.parameters.json` que descargó anteriormente, use los siguientes comandos de la CLI de Azure:

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az group deployment create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

Creación de un recurso con PowerShell

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzResourceGroup -Name $ResourceGroupName -Location $Location
New-AzResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Recursos de Azure Service Fabric

Puede implementar aplicaciones y servicios en el clúster de Service Fabric mediante Azure Resource Manager. Consulte [Administración de aplicaciones y servicios como recursos de Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource). Los siguientes son recursos específicos de la aplicación Service Fabric de procedimientos recomendados que incluir en los recursos de la plantilla de Resource Manager.

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Para implementar la aplicación con Azure Resource Manager, primero debe [crear un archivo sfpkg](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg) (paquete de aplicación de Service Fabric). El siguiente script de Python es un ejemplo de cómo crear un sfpkg:

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(
    self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(
            root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="azure-virtual-machine-operating-system-automatic-upgrade-configuration"></a>Configuración de actualizaciones automáticas del sistema operativo de máquinas virtuales de Azure 
La actualización de las máquinas virtuales es una operación iniciada por el usuario y lo recomendable es usar la [actualización automática del sistema operativo del conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) para la administración de revisiones del host de clústeres de Azure Service Fabric; la aplicación de orquestación de revisiones (POA) es una solución alternativa que está pensada para el hospedaje fuera de Azure, y aunque se puede usar en Azure, la sobrecarga de hospedar POA en Azure es un motivo habitual para preferir la actualización automática del sistema operativo de máquina virtual sobre POA. Estas son las propiedades de plantilla de Resource Manager del conjunto de escalado de máquinas virtuales para permitir la actualización automática del sistema operativo:

```json
"upgradePolicy": {
   "mode": "Automatic",
   "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade": true,
        "disableAutomaticRollback": false
    }
},
```
Al usar actualizaciones automáticas del sistema operativo con Service Fabric, la nueva imagen del sistema operativo se implementa en un dominio de actualización cada vez para mantener la alta disponibilidad de los servicios que se ejecutan en Service Fabric. Para utilizar las actualizaciones automáticas del sistema operativo en Service Fabric, el clúster debe estar configurado para utilizar el nivel de durabilidad Silver o superior.

Asegúrese de que la siguiente clave del Registro se establece en false para impedir que las máquinas del host de Windows se inicien a partir de actualizaciones no coordinadas: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU.

Estas son las propiedades de plantilla de Resource Manager del conjunto de escalado de máquinas virtuales para establecer la clave del Registro de WindowsUpdate en false:
```json
"osProfile": {
        "computerNamePrefix": "{vmss-name}",
        "adminUsername": "{your-username}",
        "secrets": [],
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "enableAutomaticUpdates": false
        }
      },
```

## <a name="azure-service-fabric-cluster-upgrade-configuration"></a>Configuración de actualizaciones del clúster de Azure Service Fabric
Esta es la propiedad de la plantilla de Resource Manager del clúster de Service Fabric para habilitar la actualización automática:
```json
"upgradeMode": "Automatic",
```
Para actualizar manualmente el clúster, descargue la distribución cab/deb en una máquina virtual de clúster y, luego, invoque el siguiente comando de PowerShell:
```powershell
Copy-ServiceFabricClusterPackage -Code -CodePackagePath <"local_VM_path_to_msi"> -CodePackagePathInImageStore ServiceFabric.msi -ImageStoreConnectionString "fabric:ImageStore"
Register-ServiceFabricClusterPackage -Code -CodePackagePath "ServiceFabric.msi"
Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <"msi_code_version">
```

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)
