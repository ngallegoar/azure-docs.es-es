---
title: Comandos de la CLI clásica de Azure
description: Comandos de la interfaz de la línea de comandos (CLI) de Azure para administrar recursos.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 04/18/2017
ms.author: cynthn
ms.openlocfilehash: 5b7d6f4efa4a910d3141638602c603d484fac6da
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86181869"
---
# <a name="azure-classic-cli-commands"></a>Comandos de la CLI clásica de Azure 

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Este tema describe cómo instalar la CLI clásica de Azure. La CLI clásica está en desuso y solo debe usarse con el modelo de implementación clásico. Para las demás implementaciones, utilice la [CLI de Azure](https://docs.microsoft.com/cli/azure/).

En este artículo se proporcionan la sintaxis y las opciones de los comandos de la interfaz de la línea de comandos (CLI) clásica de Azure que normalmente se usan para crear y administrar recursos de Azure. Tenga en cuenta que esta no es una referencia completa y que la versión de CLI puede mostrar algunos comandos o parámetros diferentes. 

En primer lugar, [instale la CLI clásica de Azure](../cli-install-nodejs.md) y [conéctese a su suscripción de Azure](/cli/azure/authenticate-azure-cli).

Para las opciones y la sintaxis de comando actuales de la línea de comandos en el modo del Administrador de recursos, escriba `azure help` o, para mostrar la ayuda de un comando específico, `azure help [command]`. También encontrará ejemplos de CLI en la documentación para crear y administrar servicios de Azure concretos.

Se muestran parámetros opcionales entre corchetes (por ejemplo, `[parameter]`). Los demás parámetros son obligatorios.

Además de los parámetros opcionales específicos de los comandos documentados aquí, existen tres parámetros opcionales que pueden usarse para mostrar el resultado detallado como opciones de solicitud y códigos de estado. El parámetro `-v` ofrece un resultado detallado y el parámetro `-vv`, un resultado incluso más detallado. La opción `--json` mostrará el resultado en formato JSON sin procesar.

## <a name="setting-the-resource-manager-mode"></a>Configuración del modo de Resource Manager
Use el siguiente comando para habilitar los comandos de la CLI de Azure en modo de Resource Manager.

```azurecli
azure config mode arm
```

> [!NOTE]
> El modo de Azure Resource Manager de la CLI y el modo de administración de servicios de Azure son mutuamente excluyentes. Es decir, los recursos creados en un modo no se pueden administrar desde el otro.
>


## <a name="account-information"></a>Información de cuenta
La herramienta usa la información de la suscripción de Azure para la conexión a su cuenta.

**Enumerar las suscripciones importadas**

```azurecli
account list [options]
```

**Mostrar detalles acerca de la suscripción**  

```azurecli
account show [options] [subscriptionNameOrId]
```

**Establecer la suscripción actual**
```azurecli
account set [options] <subscriptionNameOrId>
```

**Quitar una suscripción o entorno, o borrar toda la información de cuenta y del entorno**  
```azurecli
account clear [options]
```

**Comandos para administrar el entorno de cuenta**  

```azurecli
account env list [options]
account env show [options] [environment]
account env add [options] [environment]
account env set [options] [environment]
account env delete [options] [environment]
```

## <a name="active-directory-objects"></a>Objetos de Active Directory
**Comandos para mostrar aplicaciones de Active Directory**

```azurecli
ad app create [options]
ad app delete [options] <object-id>
```

**Comandos para mostrar grupos de Active Directory**

```azurecli
ad group list [options]
ad group show [options]
```

**Comandos para proporcionar información de un subgrupo o miembro de Active Directory**

```azurecli
ad group member list [options] [objectId]
```

**Comandos para mostrar entidades de servicio de Active Directory**

```azurecli
ad sp list [options]
ad sp show [options]
ad sp create [options] <application-id>
ad sp delete [options] <object-id>
```

**Comandos para mostrar usuarios de Active Directory**

```azurecli
ad user list [options]
ad user show [options]
```

## <a name="availability-sets"></a>Conjuntos de disponibilidad
**Crea un conjunto de disponibilidad dentro de un grupo de recursos**

```azurecli
availset create [options] <resource-group> <name> <location> [tags]
```

**Enumera los conjuntos de disponibilidad dentro de un grupo de recursos**

```azurecli
availset list [options] <resource-group>
```

**Obtiene un conjunto de disponibilidad dentro de un grupo de recursos**

```azurecli
availset show [options] <resource-group> <name>
```

**Elimina un conjunto de disponibilidad dentro de un grupo de recursos**

```azurecli
availset delete [options] <resource-group> <name>
```

## <a name="local-settings"></a>Configuración local
**Mostrar ajustes de configuración de la CLI de Azure**

```azurecli
config list [options]
```

**Eliminar un valor de configuración**

```azurecli
config delete [options] <name>
```

**Actualizar un valor de configuración**

```azurecli
config set <name> <value>
```

**Establece el modo de trabajo de la CLI de Azure como `arm` o `asm`**

```azurecli
config mode [options] <modename>
```


## <a name="account-features"></a>Características de la cuenta
**Enumera todas las características disponibles para su suscripción**

```azurecli
feature list [options]
```

**Muestra una característica**

```azurecli
feature show [options] <providerName> <featureName>
```

**Registra una característica de vista previa de un proveedor de recursos**

```azurecli
feature register [options] <providerName> <featureName>
```

## <a name="resource-groups"></a>Grupos de recursos
**Crea un grupo de recursos**

```azurecli
group create [options] <name> <location>
```

**Establece etiquetas para un grupo de recursos**

```azurecli
group set [options] <name> <tags>
```

**Elimina un grupo de recursos**

```azurecli
group delete [options] <name>
```

**Enumera los grupos de recursos para la suscripción**

```azurecli
group list [options]
```

**Muestra un grupo de recursos para la suscripción**

```azurecli
group show [options] <name>
```

**Comandos para administrar registros de grupo de recursos**

```azurecli
group log show [options] [name]
```

**Comandos para administrar la implementación en un grupo de recursos**

```azurecli
group deployment create [options] [resource-group] [name]
group deployment list [options] <resource-group> [state]
group deployment show [options] <resource-group> [deployment-name]
group deployment stop [options] <resource-group> [deployment-name]
```

**Comandos para administrar la plantilla de grupos de recursos local o de la galería**

```azurecli
group template list [options]
group template show [options] <name>
group template download [options] [name] [file]
group template validate [options] <resource-group>
```

## <a name="hdinsight-clusters"></a>Clústeres de HDInsight
**Comandos para crear o agregar a un archivo de configuración de clúster**

```azurecli
hdinsight config create [options] <configFilePath> <overwrite>
hdinsight config add-config-values [options] <configFilePath>
hdinsight config add-script-action [options] <configFilePath>
```


Ejemplo: Crear un archivo de configuración que contenga una acción de sript para ejecutarlo al crear un clúster.

```azurecli
hdinsight config create "C:\myFiles\configFile.config"
hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"
```

**Comando para crear un clúster en un grupo de recursos**

```azurecli
hdinsight cluster create [options] <clusterName>
```

Ejemplo: Crear una clúster de Linux o Storm

```azurecli
azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

info:    Executing command hdinsight cluster create
+ Submitting the request to create cluster...
info:    hdinsight cluster create command OK
```

Ejemplo: Crear un clúster con una acción de script

```azurecli
azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

info:    Executing command hdinsight cluster create
+ Submitting the request to create cluster...
info:    hdinsight cluster create command OK
```

Opciones de parámetro:

```txt
-h, --help                                                 output usage information
-v, --verbose                                              use verbose output
-vv                                                        more verbose with debug output
--json                                                     use json output
-g --resource-group <resource-group>                       The name of the resource group
-c, --clusterName <clusterName>                            HDInsight cluster name
-l, --location <location>                                  Data center location for the cluster
-y, --osType <osType>                                      HDInsight cluster operating system
'Windows' or 'Linux'
--version <version>                                        HDInsight cluster version
--clusterType <clusterType>                                HDInsight cluster type.
Hadoop | HBase | Spark | Storm
--defaultStorageAccountName <storageAccountName>           Storage account url to use for default HDInsight storage
--defaultStorageAccountKey <storageAccountKey>             Key to the storage account to use for default HDInsight storage
--defaultStorageContainer <storageContainer>               Container in the storage account to use for HDInsight default storage
--headNodeSize <headNodeSize>                              (Optional) Head node size for the cluster
--workerNodeCount <workerNodeCount>                        Number of worker nodes to use for the cluster
--workerNodeSize <workerNodeSize>                          (Optional) Worker node size for the cluster)
--zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for the cluster
--userName <userName>                                      Cluster username
--password <password>                                      Cluster password
--sshUserName <sshUserName>                                SSH username (only for Linux clusters)
--sshPassword <sshPassword>                                SSH password (only for Linux clusters)
--sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
--rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
--rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
--rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
For example 12/12/2015 (only for Windows clusters)
--virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for the cluster.
Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
--subnetName <subnetName>                                  (Optional) Subnet for the cluster
--additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
Can be multiple.
In the format of 'accountName#accountKey'.
For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
--hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for the external metastore for Hive
--hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for the external metastore for Hive
--hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for the external metastore for Hive
--hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for the external metastore for Hive
--oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for the external metastore for Oozie
--oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for the external metastore for Oozie
--oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for the external metastore for Oozie
--oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for the external metastore for Oozie
--configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
-s, --subscription <id>                                    The subscription id
--tags <tags>                                              Tags to set to the cluster.
Can be multiple.
In the format of 'name=value'.
Name is required and value is optional.
For example, --tags tag1=value1;tag2
```


**Comando para eliminar un clúster**

```azurecli
hdinsight cluster delete [options] <clusterName>
```

**Comandos para mostrar los detalles del clúster**

```azurecli
hdinsight cluster show [options] <clusterName>
```

**Comando para enumerar todos los clústeres (en un grupo de recursos específico, si se ofrece)**

```azurecli
hdinsight cluster list [options]
```

**Comando para cambiar el tamaño de un clúster**

```azurecli
hdinsight cluster resize [options] <clusterName> <targetInstanceCount>
```

**Comando para habilitar el acceso HTTP para un clúster**

```azurecli
hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>
```

**Comando para deshabilitar el acceso HTTP para un clúster**

```azurecli
hdinsight cluster disable-http-access [options] <clusterName>
```

**Comando para habilitar el acceso RDP para un clúster**

```azurecli
hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>
```

**Comando para deshabilitar el acceso HTTP para un clúster**

```azurecli
hdinsight cluster disable-rdp-access [options] <clusterName>
```

## <a name="insights-events-alert-rules-autoscale-settings-metrics"></a>Insights (eventos, reglas de alerta, configuración de escalado automático, métricas)
**Recuperar registros de operaciones de una suscripción, un correlationId, un grupo de recursos, un recurso o un proveedor de recursos**

```azurecli
insights logs list [options]
```

## <a name="locations"></a>Ubicaciones 
**Enumerar las ubicaciones disponibles**

```azurecli
location list [options]
```

## <a name="network-resources"></a>Recursos de red
**Comandos para administrar redes virtuales**

```azurecli
network vnet create [options] <resource-group> <name> <location>
```
Crea una red virtual. En el ejemplo siguiente, crearemos una red virtual denominada newvnet para myresourcegroup de grupo de recursos en la región Oeste de EE. UU.

```azurecli
azure network vnet create myresourcegroup newvnet "west us"
info:    Executing command network vnet create
+ Looking up virtual network "newvnet"
+ Creating virtual network "newvnet"
    Loading virtual network state
data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
data:    Name:                 newvnet
data:    Type:                 Microsoft.Network/virtualNetworks
data:    Location:             westus
data:    Tags:
data:    Provisioning state:   Succeeded
data:    Address prefixes:
data:     10.0.0.0/8
data:    DNS servers:
data:    Subnets:
data:
info:    network vnet create command OK
```


Opciones de parámetro:

```txt
-h, --help                                 output usage information
-v, --verbose                              use verbose output
--json                                     use json output
-g, --resource-group <resource-group>      the name of the resource group
-n, --name <name>                          the name of the virtual network
-l, --location <location>                  the location
-a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network
For example -a 10.0.0.0/24,10.0.1.0/24.
Default value is 10.0.0.0/8

-d, --dns-servers <dns-servers>            the comma separated list of DNS servers IP addresses
-t, --tags <tags>                          the tags set on this virtual network.
Can be multiple. In the format of "name=value".
Name is required and value is optional.
For example, -t tag1=value1;tag2
-s, --subscription <subscription>          the subscription identifier
```
<BR>

```azurecli
network vnet set [options] <resource-group> <name>
```

Actualiza una configuración de red virtual dentro de un grupo de recursos.

```azurecli
azure network vnet set myresourcegroup newvnet

info:    Executing command network vnet set
+ Looking up virtual network "newvnet"
+ Updating virtual network "newvnet"
+ Loading virtual network state
data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
data:    Name:                 newvnet
data:    Type:                 Microsoft.Network/virtualNetworks
data:    Location:             westus
data:    Tags:
data:    Provisioning state:   Succeeded
data:    Address prefixes:
data:     10.0.0.0/8
data:    DNS servers:
data:    Subnets:
data:
info:    network vnet set command OK
```

Opciones de parámetro:

```txt
-h, --help                                 output usage information
-v, --verbose                              use verbose output
--json                                     use json output
-g, --resource-group <resource-group>      the name of the resource group
-n, --name <name>                          the name of the virtual network
-a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network.
For example -a 10.0.0.0/24,10.0.1.0/24.
This list will be appended to the current list of address prefixes.
The address prefixes in this list should not overlap between them.
The address prefixes in this list should not overlap with existing address prefixes in the vnet.

-d, --dns-servers [dns-servers]            the comma separated list of DNS servers IP addresses.
This list will be appended to the current list of DNS server IP addresses.

-t, --tags <tags>                          the tags set on this virtual network.
Can be multiple. In the format of "name=value".
Name is required and value is optional. For example, -t tag1=value1;tag2.
This list will be appended to the current list of tags

--no-tags                                  remove all existing tags
-s, --subscription <subscription>          the subscription identifier
```
<BR>

```azurecli
network vnet list [options] <resource-group>
```

El comando enumera todas las redes virtuales en un grupo de recursos.

```azurecli
C:\>azure network vnet list myresourcegroup

info:    Executing command network vnet list
+ Listing virtual networks
    data:    ID
    Name      Location  Address prefixes  DNS servers
data:    -------------------------------------------------------------------
------  --------  --------  ----------------  -----------
data:    /subscriptions/###############################/resourceGroups/
wvnet   newvnet   westus    10.0.0.0/8
info:    network vnet list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-s, --subscription <subscription>      the subscription identifier
```

<BR>

```azurecli
network vnet show [options] <resource-group> <name>
```
El comando muestra las propiedades de red virtual en un grupo de recursos.

```azurecli
azure network vnet show -g myresourcegroup -n newvnet

info:    Executing command network vnet show
+ Looking up virtual network "newvnet"
data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
data:    Name:                 newvnet
data:    Type:                 Microsoft.Network/virtualNetworks
data:    Location:             westus
data:    Tags:
data:    Provisioning state:   Succeeded
data:    Address prefixes:
data:     10.0.0.0/8
data:    DNS servers:
data:    Subnets:
data:
info:    network vnet show command OK
```
<BR>

```azurecli
network vnet delete [options] <resource-group> <name>
```
El comando quita una red virtual.

```azurecli
azure network vnet delete myresourcegroup newvnetX

info:    Executing command network vnet delete
+ Looking up virtual network "newvnetX"
Delete virtual network newvnetX? [y/n] y
+ Deleting virtual network "newvnetX"
info:    network vnet delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-n, --name <name>                      the name of the virtual network
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```


**Comandos para administrar subredes virtuales**

```azurecli
network vnet subnet create [options] <resource-group> <vnet-name> <name>
```

Agrega otra subred a una red virtual existente.

```azurecli
azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

info:    Executing command network vnet subnet create
+ Looking up the subnet "subnet"
+ Creating subnet "subnet"
+ Looking up the subnet "subnet"
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
data:    Name:                      subnet
data:    Type:                      Microsoft.Network/virtualNetworks/subnets
data:    Provisioning state:        Succeeded
data:    Address prefix:            10.0.1.0/24
info:    network vnet subnet create command OK
```

Opciones de parámetro:

```txt
-h, --help                                                       output usage information
-v, --verbose                                                    use verbose output
    --json                                                           use json output
-g, --resource-group <resource-group>                            the name of the resource group
-e, --vnet-name <vnet-name>                                      the name of the virtual network
-n, --name <name>                                                the name of the subnet
-a, --address-prefix <address-prefix>                            the address prefix
-w, --network-security-group-id <network-security-group-id>      the network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
-o, --network-security-group-name <network-security-group-name>  the network security group name
-s, --subscription <subscription>                                the subscription identifier
```

<BR>

```azurecli
network vnet subnet set [options] <resource-group> <vnet-name> <name>
```

Establece una subred de red virtual específica dentro de un grupo de recursos.

```azurecli
C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

info:    Executing command network vnet subnet set
+ Looking up the subnet "subnet1"
+ Setting subnet "subnet1"
+ Looking up the subnet "subnet1"
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
data:    Name:                      subnet1
data:    Type:                      Microsoft.Network/virtualNetworks/subnets
data:    Provisioning state:        Succeeded
data:    Address prefix:            10.0.1.0/24
info:    network vnet subnet set command OK
```
<BR>

```azurecli
network vnet subnet list [options] <resource-group> <vnet-name>
```

Enumera todas las subredes de red virtual para una red virtual específica dentro de un grupo de recursos.

```azurecli
azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

info:    Executing command network vnet subnet set
+ Looking up the subnet "subnet1"
+ Setting subnet "subnet1"
+ Looking up the subnet "subnet1"
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
data:    Name:                      subnet1
data:    Type:                      Microsoft.Network/virtualNetworks/subnets
data:    Provisioning state:        Succeeded
data:    Address prefix:            10.0.1.0/24
info:    network vnet subnet set command OK
```
<BR>

```azurecli
network vnet subnet show [options] <resource-group> <vnet-name> <name>
```
Muestra las propiedades de la subred de red virtual.

```azurecli
azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

info:    Executing command network vnet subnet show
+ Looking up the subnet "subnet1"
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
.Network/virtualNetworks/newvnet/subnets/subnet1
data:    Name:                      subnet1
data:    Type:                      Microsoft.Network/virtualNetworks/subnets
data:    Provisioning state:        Succeeded
data:    Address prefix:            10.0.1.0/24
info:    network vnet subnet show command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-e, --vnet-name <vnet-name>            the name of the virtual network
-n, --name <name>                      the name of the subnet
-s, --subscription <subscription>      the subscription identifier
```
<BR>

```azurecli
network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
```
Quita una subred de una red virtual existente.

```azurecli
azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

info:    Executing command network vnet subnet delete
+ Looking up the subnet "subnet1"
Delete subnet "subnet1"? [y/n] y
+ Deleting subnet "subnet1"
info:    network vnet subnet delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-e, --vnet-name <vnet-name>            the name of the virtual network
-n, --name <name>                      the subnet name
-s, --subscription <subscription>      the subscription identifier
-q, --quiet                            quiet mode, do not ask for delete confirmation
```

**Comandos para administrar los equilibradores de carga**

```azurecli
network lb create [options] <resource-group> <name> <location>
```
Crea un conjunto de equilibrador de carga.

```azurecli
azure network lb create -g myresourcegroup -n mylb -l westus

info:    Executing command network lb create
+ Looking up the load balancer "mylb"
+ Creating load balancer "mylb"
+ Looking up the load balancer "mylb"
data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
data:    Name:                         mylb
data:    Type:                         Microsoft.Network/loadBalancers
data:    Location:                     westus
data:    Provisioning state:           Succeeded
info:    network lb create command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-n, --name <name>                      the name of the load balancer
-l, --location <location>              the location
-t, --tags <tags>                      the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional. For example, -t tag1=value1;tag2
-s, --subscription <subscription>      the subscription identifier
```
<BR>

```azurecli
network lb list [options] <resource-group>
```
Enumera los recursos de Equilibrador de carga dentro de un grupo de recursos.

```azurecli
azure network lb list myresourcegroup

info:    Executing command network lb list
+ Getting the load balancers
data:    Name  Location
data:    ----  --------
data:    mylb  westus
info:    network lb list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-s, --subscription <subscription>      the subscription identifier
```
<BR>

```azurecli
network lb show [options] <resource-group> <name>
```

Muestra información del equilibrador de un equilibrador de carga específico dentro de un grupo de recursos de carga.

```azurecli
azure network lb show myresourcegroup mylb -v

info:    Executing command network lb show
verbose: Looking up the load balancer "mylb"
data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
data:    Name:                         mylb
data:    Type:                         Microsoft.Network/loadBalancers
data:    Location:                     westus
data:    Provisioning state:           Succeeded
info:    network lb show command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-n, --name <name>                      the name of the load balancer
-s, --subscription <subscription>      the subscription identifier
```

<BR>

```azurecli
network lb delete [options] <resource-group> <name>
```

Elimina recursos de equilibrador de carga.

```azurecli
azure network lb delete  myresourcegroup mylb

info:    Executing command network lb delete
+ Looking up the load balancer "mylb"
Delete load balancer "mylb"? [y/n] y
+ Deleting load balancer "mylb"
info:    network lb delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-n, --name <name>                      the name of the load balancer
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```

**Comandos para administrar los sondeos de un equilibrador de carga**

```azurecli
network lb probe create [options] <resource-group> <lb-name> <name>
```

Crear la configuración de sondeo de estado de mantenimiento en el equilibrador de carga. Tenga en cuenta que, para ejecutar este comando, el equilibrador de carga requiere un recurso frontend-ip (consulte el comando "azure network frontend-ip" para asignar una dirección ip al equilibrador de carga).

```azurecli
azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

info:    Executing command network lb probe create
+ Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
info:    network lb probe create command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the probe
-p, --protocol <protocol>              the probe protocol
-o, --port <port>                      the probe port
-f, --path <path>                      the probe path
-i, --interval <interval>              the probe interval in seconds
-c, --count <count>                    the number of probes
-s, --subscription <subscription>      the subscription identifier
```

<BR>

```azurecli
network lb probe set [options] <resource-group> <lb-name> <name>
```

Actualiza un sondeo de equilibrador de carga existente con los nuevos valores.

```azurecli
azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

info:    Executing command network lb probe set
    + Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
info:    network lb probe set command OK
```

Opciones de parámetro

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the probe
-e, --new-probe-name <new-probe-name>  the new name of the probe
-p, --protocol <protocol>              the new value for probe protocol
-o, --port <port>                      the new value for probe port
-f, --path <path>                      the new value for probe path
-i, --interval <interval>              the new value for probe interval in seconds
-c, --count <count>                    the new value for number of probes
-s, --subscription <subscription>      the subscription identifier
```
<BR>

```azurecli
network lb probe list [options] <resource-group> <lb-name>
```

Enumera las propiedades de sondeo para un conjunto de equilibrador de carga.

```azurecli
C:\>azure network lb probe list -g myresourcegroup -l mylb

info:    Executing command network lb probe list
+ Looking up the load balancer "mylb"
data:    Name       Protocol  Port  Path  Interval  Count
data:    ---------  --------  ----  ----  --------  -----
data:    mylbprobe  Tcp       443         300       2
info:    network lb probe list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-s, --subscription <subscription>      the subscription identifier
```

```azurecli
network lb probe delete [options] <resource-group> <lb-name> <name>
```
Quita el sondeo creado para el equilibrador de carga.

```azurecli
azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

info:    Executing command network lb probe delete
+ Looking up the load balancer "mylb"
Delete a probe "mylbprobe?" [y/n] y
+ Updating load balancer "mylb"
info:    network lb probe delete command OK
```

**Comandos para administrar las configuraciones de frontend ip de un equilibrador de carga**

```azurecli
network lb frontend-ip create [options] <resource-group> <lb-name> <name>
```
Crea una configuración de frontend IP a un conjunto existente de equilibrador de carga.

```azurecli
azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "mylb"
+ Looking up the subnet "subnet"
+ Creating frontend IP configuration "myfrontendip"
+ Looking up the load balancer "mylb"
data:    Id:                           /subscriptions/###############################/resourceGroups/Myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
/frontendIPConfigurations/myfrontendip
data:    Name:                         myfrontendip
data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
data:    Provisioning state:           Succeeded
data:    Private IP allocation method: Dynamic
data:    Private IP address:           10.0.1.4
data:    Subnet:                       id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
data:    Public IP address:
data:    Inbound NAT rules
data:    Outbound NAT rules
data:    Load balancing rules
data:
info:    network lb frontend-ip create command OK
```

<BR>

```azurecli
network lb frontend-ip set [options] <resource-group> <lb-name> <name>
```

Actualiza una configuración existente de una dirección IP de servidor frontend. El siguiente comando agrega una IP pública denominada mypubip5 a una IP de frontend de equilibrador de carga existente denominada myfrontendip.

```azurecli
azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

info:    Executing command network lb frontend-ip set
+ Looking up the load balancer "mylb"
+ Looking up the public ip "mypubip5"
+ Updating load balancer "mylb"
+ Looking up the load balancer "mylb"
data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
data:    Name:                         myfrontendip
data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
data:    Provisioning state:           Succeeded
data:    Private IP allocation method: Dynamic
data:    Private IP address:
data:    Subnet:
data:    Public IP address:            id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypubip5
data:    Inbound NAT rules
data:    Outbound NAT rules
data:    Load balancing rules
data:
info:    network lb frontend-ip set command OK
```

Opciones de parámetro:

```txt
-h, --help                                                         output usage information
-v, --verbose                                                      use verbose output
--json                                                             use json output
-g, --resource-group <resource-group>                              the name of the resource group
-l, --lb-name <lb-name>                                            the name of the load balancer
-n, --name <name>                                                  the name of the frontend ip configuration
-a, --private-ip-address <private-ip-address>                      the private ip address
-o, --private-ip-allocation-method <private-ip-allocation-method>  the private ip allocation method [Static, Dynamic]
-u, --public-ip-id <public-ip-id>                                  the public ip identifier.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
-i, --public-ip-name <public-ip-name>                              the public ip name.
This public ip must exist in the same resource group as the lb.
Please use public-ip-id if that is not the case.
-b, --subnet-id <subnet-id>                                        the subnet id.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
-e, --subnet-name <subnet-name>                                    the subnet name
-m, --vnet-name <vnet-name>                                        the virtual network name.
This virtual network must exist in the same resource group as the lb.
Please use subnet-id if that is not the case.
-s, --subscription <subscription>                                  the subscription identifier
```

<BR>

```azurecli
network lb frontend-ip list [options] <resource-group> <lb-name>
```

Enumera todos los recursos IP de front-end configurados para el equilibrador de carga.

```azurecli
azure network lb frontend-ip list -g myresourcegroup -l mylb

info:    Executing command network lb frontend-ip list
+ Looking up the load balancer "mylb"
data:    Name         Provisioning state  Private IP allocation method  Subnet
data:    -----------  ------------------  ----------------------------  ------
data:    myprivateip  Succeeded           Dynamic
info:    network lb frontend-ip list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-s, --subscription <subscription>      the subscription identifier
```
<BR>

```azurecli
network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
```
Elimina el objeto IP frontend asociado para el equilibrador de carga

```azurecli
network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
info:    Executing command network lb frontend-ip delete
+ Looking up the load balancer "mylb"
Delete frontend ip configuration "myfrontendip"? [y/n] y
+ Updating load balancer "mylb"
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the frontend ip configuration
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```

**Comandos para administrar grupos de direcciones de backend de un equilibrador de carga**

```azurecli
network lb address-pool create [options] <resource-group> <lb-name> <name>
```

Crea un grupo de direcciones de back-end para un equilibrador de carga.

```azurecli
azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

info:    Executing command network lb address-pool create
+ Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
+ Looking up the load balancer "mylb"
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
data:    Name:                      myaddresspool
data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
data:    Provisioning state:        Succeeded
data:    Backend IP configurations:
data:    Load balancing rules:
data:
info:    network lb address-pool create command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the backend address pool
-s, --subscription <subscription>      the subscription identifier
```

<BR>

```azurecli
network lb address-pool list [options] <resource-group> <lb-name>
```

Enumerar el intervalo de grupo de direcciones IP de backend para un grupo de recursos específicos

```azurecli
azure network lb address-pool list -g myresourcegroup -l mylb

info:    Executing command network lb address-pool list
+ Looking up the load balancer "mylb"
data:    Name           Provisioning state
data:    -------------  ------------------
data:    mybackendpool  Succeeded
info:    network lb address-pool list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-s, --subscription <subscription>      the subscription identifier
```

<BR>

```azurecli
network lb address-pool delete [options] <resource-group> <lb-name> <name>
```

Quita el recurso de intervalo de grupo IP de backend de equilibrador de carga.

```azurecli
azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

info:    Executing command network lb address-pool delete
+ Looking up the load balancer "mylb"
Delete backend address pool "mybackendpool"? [y/n] y
+ Updating load balancer "mylb"
info:    network lb address-pool delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the backend address pool
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```

**Comandos para administrar reglas de equilibradores de carga**

```azurecli
network lb rule create [options] <resource-group> <lb-name> <name>
```
Crea reglas de equilibrador de carga.

Puede crear una regla de equilibrador de carga configurando el punto de conexión de front-end para el equilibrador de carga y el intervalo de grupo de direcciones de back-end para recibir el tráfico de red entrante. La configuración también incluye los puertos para el extremo IP de frontend y los puertos para el intervalo de grupo de direcciones de backend.

En el siguiente ejemplo se muestra cómo crear una regla de equilibrador de carga, el punto de conexión de front-end que escucha el puerto TCP 80 y el tráfico de red de equilibrio de carga que se envía al puerto 8080 para el intervalo de grupo de direcciones de back-end.

```azurecli
azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


info:    Executing command network lb rule create
+ Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
+ Loading rule state
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mylbrule
data:    Name:                      mylbrule
data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
data:    Provisioning state:        Succeeded
data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
data:    Protocol:                  Tcp
data:    Frontend port:             80
data:    Backend port:              8080
data:    Enable floating IP:        false
data:    Idle timeout in minutes:   10
data:    Probes
data:
info:    network lb rule create command OK
```

<BR>

```azurecli
network lb rule set [options] <resource-group> <lb-name> <name>
```

Actualiza una regla existente del equilibrador de carga establecida en un grupo de recursos específico. En el siguiente ejemplo se cambió el nombre de la regla de mylbrule a mynewlbrule.

```azurecli
azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

info:    Executing command network lb rule set
+ Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
+ Loading rule state
data:    Id:                        /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mynewlbrule
data:    Name:                      mynewlbrule
data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
data:    Provisioning state:        Succeeded
data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
data:    Protocol:                  Tcp
data:    Frontend port:             80
data:    Backend port:              8080
data:    Enable floating IP:        false
data:    Idle timeout in minutes:   10
data:    Probes
data:
info:    network lb rule set command OK
```

Opciones de parámetro:

```txt
-h, --help                                         output usage information
-v, --verbose                                      use verbose output
--json                                             use json output
-g, --resource-group <resource-group>              the name of the resource group
-l, --lb-name <lb-name>                            the name of the load balancer
-n, --name <name>                                  the name of the rule
-r, --new-rule-name <new-rule-name>                new rule name
-p, --protocol <protocol>                          the rule protocol
-f, --frontend-port <frontend-port>                the frontend port
-b, --backend-port <backend-port>                  the backend port
-e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
-i, --idle-timeout <idle-timeout>                  the idle timeout in minutes
-a, --probe-name [probe-name]                      the name of the probe defined in the same load balancer
-t, --frontend-ip-name <frontend-ip-name>          the name of the frontend ip configuration in the same load balancer
-o, --backend-address-pool <backend-address-pool>  name of the backend address pool defined in the same load balancer
-s, --subscription <subscription>                  the subscription identifier
```


```azurecli
network lb rule list [options] <resource-group> <lb-name>
```

Enumera todas las reglas configuradas para un equilibrador de carga de un grupo de recursos específico.

```azurecli
azure network lb rule list -g myresourcegroup -l mylb

info:    Executing command network lb rule list
+ Looking up the load balancer "mylb"
data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
info:    network lb rule list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-s, --subscription <subscription>      the subscription identifier
```

```azurecli
network lb rule delete [options] <resource-group> <lb-name> <name>
```

Elimina una regla de equilibrador de carga.

```azurecli
azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

info:    Executing command network lb rule delete
+ Looking up the load balancer "mylb"
Delete load balancing rule mynewlbrule? [y/n] y
+ Updating load balancer "mylb"
info:    network lb rule delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the rule
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```

**Comandos para administrar reglas NAT de entrada de equilibradores de carga**

```azurecli
network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
```
Crea una regla NAT de entrada para el equilibrador de carga.

En el ejemplo siguiente se crea una regla NAT de IP de front-end (que se definió anteriormente mediante el comando "azure network frontend-ip") con un puerto de escucha entrante y el puerto de salida que el equilibrador de carga usa para enviar el tráfico de red.

```azurecli
azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
+ Looking up the load balancer "mylb"
data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
data:    Name:                      myinboundnat
data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
data:    Provisioning state:        Succeeded
data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
data:    Backend IP configuration
data:    Protocol                   Tcp
data:    Frontend port              80
data:    Backend port               8080
data:    Enable floating IP         false
info:    network lb inbound-nat-rule create command OK
```

Opciones de parámetro:

```txt
-h, --help                                     output usage information
-v, --verbose                                  use verbose output
--json                                         use json output
-g, --resource-group <resource-group>          the name of the resource group
-l, --lb-name <lb-name>                        the name of the load balancer
-n, --name <name>                              the name of the inbound NAT rule
-p, --protocol <protocol>                      the rule protocol [tcp,udp]
-f, --frontend-port <frontend-port>            the frontend port [0-65535]
-b, --backend-port <backend-port>              the backend port [0-65535]
-e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
-i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
-m, --vm-id <vm-id>                            the VM id.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
-a, --vm-name <vm-name>                        the VM name.This VM must exist in the same resource group as the lb.
Please use vm-id if that is not the case.
this parameter will be ignored if --vm-id is specified
-s, --subscription <subscription>              the subscription identifier
```
<BR>

```azurecli
network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
```
Actualiza una regla NAT de entrada existente. En el siguiente ejemplo se cambió el puerto de escucha entrante de 80 a 81.

```azurecli
azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

info:    Executing command network lb inbound-nat-rule set
+ Looking up the load balancer "mylb"
+ Updating load balancer "mylb"
+ Looking up the load balancer "mylb"
data:    Id:                        /subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
data:    Name:                      myinboundnat
data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
data:    Provisioning state:        Succeeded
data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
data:    Backend IP configuration
data:    Protocol                   Tcp
data:    Frontend port              81
data:    Backend port               8080
data:    Enable floating IP         false
info:    network lb inbound-nat-rule set command OK
```

Opciones de parámetro:

```txt
-h, --help                                     output usage information
-v, --verbose                                  use verbose output
--json                                         use json output
-g, --resource-group <resource-group>          the name of the resource group
-l, --lb-name <lb-name>                        the name of the load balancer
-n, --name <name>                              the name of the inbound NAT rule
-p, --protocol <protocol>                      the rule protocol [tcp,udp]
-f, --frontend-port <frontend-port>            the frontend port [0-65535]
-b, --backend-port <backend-port>              the backend port [0-65535]
-e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
-i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
-m, --vm-id [vm-id]                            the VM id.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
-a, --vm-name <vm-name>                        the VM name.
This virtual machine must exist in the same resource group as the lb.
Please use vm-id if that is not the case
-s, --subscription <subscription>              the subscription identifier
```
<BR>

```azurecli
network lb inbound-nat-rule list [options] <resource-group> <lb-name>
```

Enumera todas las reglas NAT de entrada para el equilibrador de carga.

```azurecli
azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

info:    Executing command network lb inbound-nat-rule list
+ Looking up the load balancer "mylb"
data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
---------------------
data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

info:    network lb inbound-nat-rule list command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-s, --subscription <subscription>      the subscription identifier
```
<BR>

```azurecli
network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>
```

Elimina la regla NAT para el equilibrador de carga de un grupo de recursos específico.

```azurecli
azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

info:    Executing command network lb inbound-nat-rule delete
+ Looking up the load balancer "mylb"
Delete inbound NAT rule "myinboundnat?" [y/n] y
+ Updating load balancer "mylb"
info:    network lb inbound-nat-rule delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-l, --lb-name <lb-name>                the name of the load balancer
-n, --name <name>                      the name of the inbound NAT rule
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```

**Comandos para administrar direcciones ip públicas**

```azurecli
network public-ip create [options] <resource-group> <name> <location>
```
Crea un recurso de dirección IP pública. Creará el recurso de dirección ip pública y lo asociará a un nombre de dominio.

```azurecli
azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
info:    Executing command network public-ip create
+ Looking up the public ip "mytestpublicip1"
+ Creating public ip address "mytestpublicip1"
+ Looking up the public ip "mytestpublicip1"
data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
data:    Name:                 mytestpublicip1
data:    Type:                 Microsoft.Network/publicIPAddresses
data:    Location:             eastus
data:    Provisioning state:   Succeeded
data:    Allocation method:    Dynamic
data:    Idle timeout:         4
data:    Domain name label:    azureclitest
data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
info:    network public-ip create command OK
```


Opciones de parámetro:

```txt
-h, --help                                   output usage information
-v, --verbose                                use verbose output
--json                                       use json output
-g, --resource-group <resource-group>        the name of the resource group
-n, --name <name>                            the name of the public ip
-l, --location <location>                    the location
-d, --domain-name-label <domain-name-label>  the domain name label.
This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
-a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
-i, --idletimeout <idletimeout>              the idle timeout in minutes
-f, --reverse-fqdn <reverse-fqdn>            the reverse fqdn
-t, --tags <tags>                            the list of tags.
Can be multiple. In the format of "name=value".
Name is required and value is optional.
For example, -t tag1=value1;tag2
-s, --subscription <subscription>            the subscription identifier
```
<br>

```azurecli
network public-ip set [options] <resource-group> <name>
```
Actualiza las propiedades de un recurso de dirección ip pública existente. En el siguiente ejemplo hemos cambiado la dirección IP pública de dinámica a estática.

```azurecli
azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
info:    Executing command network public-ip set
+ Looking up the public ip "mytestpublicip1"
+ Updating public ip address "mytestpublicip1"
+ Looking up the public ip "mytestpublicip1"
data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
data:    Name:                 mytestpublicip1
data:    Type:                 Microsoft.Network/publicIPAddresses
data:    Location:             eastus
data:    Provisioning state:   Succeeded
data:    Allocation method:    Static
data:    Idle timeout:         4
data:    IP Address:           (static IP address)
data:    Domain name label:    azureclitest
data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
info:    network public-ip set command OK
```

Opciones de parámetro:

```txt
-h, --help                                   output usage information
-v, --verbose                                use verbose output
--json                                       use json output
-g, --resource-group <resource-group>        the name of the resource group
-n, --name <name>                            the name of the public ip
-d, --domain-name-label [domain-name-label]  the domain name label.
This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
-a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
-i, --idletimeout <idletimeout>              the idle timeout in minutes
-f, --reverse-fqdn [reverse-fqdn]            the reverse fqdn
-t, --tags <tags>                            the list of tags.
Can be multiple. In the format of "name=value".
Name is required and value is optional.
For example, -t tag1=value1;tag2
--no-tags                                    remove all existing tags
-s, --subscription <subscription>            the subscription identifier
```

<br>

```azurecli
network public-ip list [options] <resource-group>
```
Enumera todos los recursos de dirección IP pública dentro de un grupo de recursos.

```azurecli
azure network public-ip list -g myresourcegroup

info:    Executing command network public-ip list
+ Getting the public ip addresses
data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-s, --subscription <subscription>      the subscription identifier
```

<BR>

```azurecli
network public-ip show [options] <resource-group> <name>
```

Muestra las propiedades de la dirección IP pública de un recurso con IP pública dentro de un grupo de recursos.

```azurecli
azure network public-ip show -g myresourcegroup -n mytestpublicip

info:    Executing command network public-ip show
+ Looking up the public ip "mytestpublicip1"
data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip
data:    Name:                 mytestpublicip
data:    Type:                 Microsoft.Network/publicIPAddresses
data:    Location:             eastus
data:    Provisioning state:   Succeeded
data:    Allocation method:    Static
data:    Idle timeout:         4
data:    IP Address:           (static IP address)
data:    Domain name label:    azureclitest
data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
info:    network public-ip show command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-n, --name <name>                      the name of the public IP
-s, --subscription <subscription>      the subscription identifier
```


```azurecli
network public-ip delete [options] <resource-group> <name>
```

Elimina un recurso de dirección IP pública.

```azurecli
azure network public-ip delete -g group-1 -n mypublicipname
info:    Executing command network public-ip delete
+ Looking up the public ip "mypublicipname"
Delete public ip address "mypublicipname"? [y/n] y
+ Deleting public ip address "mypublicipname"
info:    network public-ip delete command OK
```

Opciones de parámetro:

```txt
-h, --help                             output usage information
-v, --verbose                          use verbose output
--json                                 use json output
-g, --resource-group <resource-group>  the name of the resource group
-n, --name <name>                      the name of the public IP
-q, --quiet                            quiet mode, do not ask for delete confirmation
-s, --subscription <subscription>      the subscription identifier
```


**Comandos para administrar las interfaces de red**

```azurecli
network nic create [options] <resource-group> <name> <location>
```
Crea un recurso denominado interfaz de red (NIC) que puede utilizarse para equilibradores de carga o para asociarlo a una máquina virtual.

```azurecli
azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

info:    Executing command network nic create
+ Looking up the network interface "testnic1"
+ Looking up the subnet "subnet-1"
+ Creating network interface "testnic1"
+ Looking up the network interface "testnic1"
data:    Id:                     /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/networkInterfaces/testnic1
data:    Name:                   testnic1
data:    Type:                   Microsoft.Network/networkInterfaces
data:    Location:               eastus
data:    Provisioning state:     Succeeded
data:    IP configurations:
data:       Name:                         NIC-config
data:       Provisioning state:           Succeeded
data:       Private IP address:           10.0.0.5
data:       Private IP Allocation Method: Dynamic
data:       Subnet:                       /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/virtualNetworks/myVNET/subnets/Subnet-1
```

Opciones de parámetro:

```txt
-h, --help                                                       output usage information
-v, --verbose                                                    use verbose output
--json                                                           use json output
-g, --resource-group <resource-group>                            the name of the resource group
-n, --name <name>                                                the name of the network interface
-l, --location <location>                                        the location
-w, --network-security-group-id <network-security-group-id>      the network security group identifier.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
-o, --network-security-group-name <network-security-group-name>  the network security group name.
This network security group must exist in the same resource group as the nic.
Please use network-security-group-id if that is not the case.
-i, --public-ip-id <public-ip-id>                                the public IP identifier.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
-p, --public-ip-name <public-ip-name>                            the public IP name.
This public ip must exist in the same resource group as the nic.
Please use public-ip-id if that is not the case.
-a, --private-ip-address <private-ip-address>                    the private IP address
-u, --subnet-id <subnet-id>                                      the subnet identifier.
e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
--subnet-name <subnet-name>                                      the subnet name
-m, --subnet-vnet-name <subnet-vnet-name>                        the vnet name under which subnet-name exists
-t, --tags <tags>                                                the comma separated list of tags.
Can be multiple. In the format of "name=value".
Name is required and value is optional.
For example, -t tag1=value1;tag2
-s, --subscription <subscription>                                the subscription identifier
data:
info:    network nic create command OK
```

<BR>

```azurecli
network nic set [options] <resource-group> <name>
network nic list [options] <resource-group>
network nic show [options] <resource-group> <name>
network nic delete [options] <resource-group> <name>
```

**Comandos para administrar grupos de seguridad de red**

```azurecli
network nsg create [options] <resource-group> <name> <location>
network nsg set [options] <resource-group> <name>
network nsg list [options] <resource-group>
network nsg show [options] <resource-group> <name>
network nsg delete [options] <resource-group> <name>
```

**Comandos para administrar reglas de grupos de seguridad de red**

```azurecli
network nsg rule create [options] <resource-group> <nsg-name> <name>
network nsg rule set [options] <resource-group> <nsg-name> <name>
network nsg rule list [options] <resource-group> <nsg-name>
network nsg rule show [options] <resource-group> <nsg-name> <name>
network nsg rule delete [options] <resource-group> <nsg-name> <name>
```

**Comandos para administrar perfiles del Administrador de tráfico**

```azurecli
network traffic-manager profile create [options] <resource-group> <name>
network traffic-manager profile set [options] <resource-group> <name>
network traffic-manager profile list [options] <resource-group>
network traffic-manager profile show [options] <resource-group> <name>
network traffic-manager profile delete [options] <resource-group> <name>
network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>
```

**Comandos para administrar extremos del Administrador de tráfico**

```azurecli
network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>
```

**Comandos para administrar puertas de enlace de redes virtuales**

```azurecli
network gateway list [options] <resource-group>
```

## <a name="resource-provider-registrations"></a>Registros del proveedor de recursos
**Enumerar los proveedores registrados actualmente en Resource Manager**

```azurecli
provider list [options]
```

**Mostrar detalles sobre el espacio de nombres de proveedor solicitado**

```azurecli
provider show [options] <namespace>
```

**Registrar el proveedor con la suscripción**

```azurecli
provider register [options] <namespace>
```

**Anular el registro del proveedor con la suscripción**

```azurecli
provider unregister [options] <namespace>
```

## <a name="resources"></a>Recursos
**Crea un recurso en un grupo de recursos.**

```azurecli
resource create [options] <resource-group> <name> <resource-type> <location> <api-version>
```

**Actualiza un recurso en un grupo de recursos sin parámetros ni plantillas.**

```azurecli
resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>
```

**Enumera los recursos.**

```azurecli
resource list [options] [resource-group]
```

**Obtiene un recurso dentro de un grupo de recursos o la suscripción.**

```azurecli
resource show [options] <resource-group> <name> <resource-type> <api-version>
```

**Elimina un recurso en un grupo de recursos.**

```azurecli
resource delete [options] <resource-group> <name> <resource-type> <api-version>
```

## <a name="azure-roles"></a>Roles de Azure
**Obtener todas las definiciones de rol disponibles**

```azurecli
role list [options]
```

**Obtener una definición de rol disponible**

```azurecli
role show [options] [name]
```

**Comandos para administrar la asignación de su rol**

```azurecli
role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
```

## <a name="storage-objects"></a>Objetos de almacenamiento
**Comandos para administrar cuentas de Almacenamiento**

```azurecli
storage account list [options]
storage account show [options] <name>
storage account create [options] <name>
storage account set [options] <name>
storage account delete [options] <name>
```

**Comandos para administrar claves de cuentas de Almacenamiento**

```azurecli
storage account keys list [options] <name>
storage account keys renew [options] <name>
```

**Comandos para mostrar la cadena de conexión de Storage**

```azurecli
storage account connectionstring show [options] <name>
```

**Comandos para administrar contenedores de Almacenamiento**

```azurecli
storage container list [options] [prefix]
storage container show [options] [container]
storage container create [options] [container]
storage container delete [options] [container]
storage container set [options] [container]
```

**Comandos para administrar firmas de acceso compartido de su contenedor de almacenamiento**

```azurecli
storage container sas create [options] [container] [permissions] [expiry]
```

**Comandos para administrar políticas de acceso compartido de su contenedor de Almacenamiento**

```azurecli
storage container policy create [options] [container] [name]
storage container policy show [options] [container] [name]
storage container policy list [options] [container]
storage container policy set [options] [container] [name]
storage container policy delete [options] [container] [name]
```

**Comandos para administrar blobs de Almacenamiento**

```azurecli
storage blob list [options] [container] [prefix]
storage blob show [options] [container] [blob]
storage blob delete [options] [container] [blob]
storage blob upload [options] [file] [container] [blob]
storage blob download [options] [container] [blob] [destination]
```

**Comandos para administrar sus operaciones de copia de blobs**

```azurecli
storage blob copy start [options] [sourceUri] [destContainer]
storage blob copy show [options] [container] [blob]
storage blob copy stop [options] [container] [blob] [copyid]
```

**Comandos para administrar la firma de acceso compartido de su blob de Almacenamiento**

```azurecli
storage blob sas create [options] [container] [blob] [permissions] [expiry]
```

**Comandos para administrar sus archivos compartidos de Almacenamiento**

```azurecli
storage share create [options] [share]
storage share show [options] [share]
storage share delete [options] [share]
storage share list [options] [prefix]
```

**Comandos para administrar sus archivos de Storage**

```azurecli
storage file list [options] [share] [path]
storage file delete [options] [share] [path]
storage file upload [options] [source] [share] [path]
storage file download [options] [share] [path] [destination]
```

**Comandos para administrar su directorio de archivos de Almacenamiento**

```azurecli
storage directory create [options] [share] [path]
storage directory delete [options] [share] [path]
```

**Comandos para administrar sus colas de Almacenamiento**

```azurecli
storage queue create [options] [queue]
storage queue list [options] [prefix]
storage queue show [options] [queue]
storage queue delete [options] [queue]
```

**Comandos para administrar firmas de acceso compartido de su cola de Almacenamiento**

```azurecli
storage queue sas create [options] [queue] [permissions] [expiry]
```

**Comandos para administrar políticas de acceso compartido de su cola de Almacenamiento**

```azurecli
storage queue policy create [options] [queue] [name]
storage queue policy show [options] [queue] [name]
storage queue policy list [options] [queue]
storage queue policy set [options] [queue] [name]
storage queue policy delete [options] [queue] [name]
```

**Comandos para administrar propiedades de registro de Almacenamiento**

```azurecli
storage logging show [options]
storage logging set [options]
```

**Comandos para administrar propiedades de métricas de Almacenamiento**

```azurecli
storage metrics show [options]
storage metrics set [options]
```

**Comandos para administrar tablas de Almacenamiento**

```azurecli
storage table create [options] [table]
storage table list [options] [prefix]
storage table show [options] [table]
storage table delete [options] [table]
```

**Comandos para administrar firmas de acceso compartido de su tabla de Almacenamiento**

```azurecli
storage table sas create [options] [table] [permissions] [expiry]
```

**Comandos para administrar políticas de acceso compartido de su tabla de Almacenamiento**

```azurecli
storage table policy create [options] [table] [name]
storage table policy show [options] [table] [name]
storage table policy list [options] [table]
storage table policy set [options] [table] [name]
storage table policy delete [options] [table] [name]
```

## <a name="tags"></a>Etiquetas
**Agregar una etiqueta**

```azurecli
tag create [options] <name> <value>
```

**Quitar una etiqueta o un valor de etiqueta**

```azurecli
tag delete [options] <name> <value>
```

**Enumerar la información de la etiqueta**

```azurecli
tag list [options]
```

**Obtener una etiqueta**

```azurecli
tag show [options] [name]
```

## <a name="virtual-machines"></a>Virtual Machines
**Crear una máquina virtual**

```azurecli
vm create [options] <resource-group> <name> <location> <os-type>
```

**Crear una máquina virtual con los recursos predeterminados**

```azurecli
vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password
```

> [!TIP]
> A partir de CLI versión 0.10, puede proporcionar un alias corto como UbuntuLTS o Win2012R2Datacenter como `image-urn` para algunas imágenes populares de Marketplace. Ejecute `azure help vm quick-create` para ver las opciones. Además, desde la versión 0.10, `azure vm quick-create` utiliza Almacenamiento premium de forma predeterminada, si está disponible en la región seleccionada.
> 
> 

**Mostrar las máquinas virtuales dentro de una cuenta**

```azurecli
vm list [options]
```

**Obtener una máquina virtual en un grupo de recursos**

```azurecli
vm show [options] <resource-group> <name>
```

**Eliminar una máquina virtual en un grupo de recursos**

```azurecli
vm delete [options] <resource-group> <name>
```

**Cerrar una máquina virtual en un grupo de recursos**

```azurecli
vm stop [options] <resource-group> <name>
```

**Reiniciar una máquina virtual en un grupo de recursos**

```azurecli
vm restart [options] <resource-group> <name>
```

**Iniciar una máquina virtual en un grupo de recursos**

```azurecli
vm start [options] <resource-group> <name>
```

**Cerrar una máquina virtual en un grupo de recursos y liberar recursos de proceso**

```azurecli
vm deallocate [options] <resource-group> <name>
```

**Enumerar los tamaños disponibles de máquina virtual**

```azurecli
vm sizes [options]
```

**Capturar la máquina virtual como imagen del sistema operativo o como imagen de VM**

```azurecli
vm capture [options] <resource-group> <name> <vhd-name-prefix>
```

**Establecer el estado de la máquina virtual como Generalized**

```azurecli
vm generalize [options] <resource-group> <name>
```

**Obtener la vista de instancia de la máquina virtual**

```azurecli
vm get-instance-view [options] <resource-group> <name>
```

**Restablecer la configuración de acceso de escritorio remoto o SSH en una máquina virtual y restablecer la contraseña para la cuenta que tiene como administrador o autoridad sudo**

```azurecli
vm reset-access [options] <resource-group> <name>
```

**Actualizar la máquina virtual con nuevos datos**

```azurecli
vm set [options] <resource-group> <name>
```

**Comandos para administrar discos de datos de máquinas virtuales**

```azurecli
vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
vm disk detach [options] <resource-group> <vm-name> <lun>
vm disk attach [options] <resource-group> <vm-name> [vhd-url]
```

**Comandos para administrar las extensiones de recursos de máquina virtual**

```azurecli
vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
vm extension get [options] <resource-group> <vm-name>
```

**Comandos para administrar sus máquinas virtuales Docker**

```azurecli
vm docker create [options] <resource-group> <name> <location> <os-type>
```

**Comandos para administrar imágenes de máquina virtual**

```azurecli
vm image list-publishers [options] <location>
vm image list-offers [options] <location> <publisher>
vm image list-skus [options] <location> <publisher> <offer>
vm image list [options] <location> <publisher> [offer] [sku]
```
