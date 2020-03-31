---
title: Implementación de un grupo de contenedores en Azure Virtual Network
description: Aprenda a implementar grupos de contenedores en una red virtual de Azure nueva o existente.
ms.topic: article
ms.date: 01/06/2020
ms.author: danlep
ms.openlocfilehash: 318576e9b5c5b32bbc993ea16494c938b74bd2f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77200068"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Implementación de instancias de contenedor en una red virtual de Azure

[Azure Virtual Network](../virtual-network/virtual-networks-overview.md) proporciona acceso de red seguro y privado a los recursos locales y de Azure. Al implementar grupos de contenedores en una red virtual de Azure, los contenedores pueden comunicarse de forma segura con otros recursos de la red virtual.

Los grupos de contenedores implementados en una red virtual de Azure permiten escenarios como:

* Comunicación directa entre grupos de contenedores en la misma subred
* Enviar la salida de la carga de trabajo [basada en tareas](container-instances-restart-policy.md) procedente de las instancias de contenedor a una base de datos en la red virtual
* Recuperar contenido de las instancias de contenedor desde un [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) en la red virtual
* Comunicación de contenedor con máquinas virtuales en la red virtual
* Comunicación de contenedor con recursos locales mediante una [puerta de enlace VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) o [ExpressRoute](../expressroute/expressroute-introduction.md)

> [!IMPORTANT]
> Las implementaciones de grupo de contenedores en una red virtual están disponibles con carácter general para cargas de trabajo de producción únicamente en las regiones siguientes: **Este de EE. UU., Centro y Sur de EE. UU., y Oeste de EE. UU. 2**. En otras regiones en las que la característica está disponible, las implementaciones de red virtual se encuentran actualmente en versión preliminar, con disponibilidad general planificada en un futuro próximo. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. 


## <a name="virtual-network-deployment-limitations"></a>Limitaciones de la implementación de redes virtuales

Se aplican ciertas limitaciones al implementar grupos de contenedores en una red virtual.

* Para implementar grupos de contenedores en una subred, la subred no puede contener otros tipos de recursos. Quite todos los recursos existentes de una subred existente antes de implementar grupos de contenedores en ella o crear una nueva subred.
* No puede usar una [identidad administrada](container-instances-managed-identity.md) en un grupo de contenedores implementado en una red virtual.
* No puede habilitar un [sondeo de ejecución](container-instances-liveness-probe.md) o un [sondeo de preparación](container-instances-readiness-probe.md) en un grupo de contenedores implementado en una red virtual.
* Debido a los recursos de red adicionales implicados, la implementación de un grupo de contenedores en una red virtual suele ser más lenta que la de una instancia de contenedor estándar.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

Los límites de recursos del contenedor pueden diferir de los límites de las instancias de contenedor que no están en red en estas regiones. Actualmente solo se admiten contenedores de Linux para esta característica. Está prevista la compatibilidad con Windows.

### <a name="unsupported-networking-scenarios"></a>Escenarios de red avanzados no admitidos 

* **Azure Load Balancer**: no se admite la colocación de Azure Load Balancer al principio de las instancias de contenedor en un grupo de contenedores en red
* **Emparejamiento de redes virtuales**
  * El emparejamiento de redes virtuales no funcionará para ACI si la red a la que se empareja la red virtual de ACI usa un espacio de direcciones IP públicas. La red emparejada necesita un espacio de direcciones IP privada RFC 1918 para que el emparejamiento de red virtual funcione. 
  * Solo puede emparejar la red virtual a otra red virtual.
* **Enrutamiento de tráfico de red virtual**: no se pueden configurar rutas personalizadas en torno a direcciones IP públicas. Las rutas se pueden configurar en el espacio de direcciones IP privadas de la subred delegada en la que se implementan los recursos de ACI. 
* **Grupos de seguridad de red**: actualmente no se aplican las reglas de seguridad salientes en grupos de seguridad de red aplicados a una subred delegada a Azure Container Instances. 
* **Etiqueta de dirección IP o DNS pública**: los grupos de contenedores implementados en una red virtual no son compatibles actualmente con la exposición directa en Internet de contenedores con una dirección IP pública o un nombre de dominio completo.
* **Resolución de nombres interna**: no se admite la resolución de nombres para los recursos de Azure en la red virtual mediante la instancia interna de Azure DNS.

**La eliminación de recursos de red** requiere [pasos adicionales](#delete-network-resources) cuando se han implementado grupos de contenedores en la red virtual.

## <a name="required-network-resources"></a>Recursos de red necesarios

Hay tres recursos de Azure Virtual Network necesarios para implementar grupos de contenedores en una red virtual: la propia [red virtual](#virtual-network), una [subred delegada](#subnet-delegated) dentro de la red virtual y un [perfil de red](#network-profile). 

### <a name="virtual-network"></a>Virtual network

Una red virtual define el espacio de direcciones en el que crear una o varias subredes. A continuación, implemente los recursos de Azure (por ejemplo, los grupos de contenedores) en las subredes de la red virtual.

### <a name="subnet-delegated"></a>Subred (delegada)

Las subredes segmentan la red virtual en espacios de direcciones independientes utilizables por los recursos de Azure que coloque en ellos. Crea una o varias subredes dentro de una red virtual.

La subred que usó para los grupos de contenedores puede contener solo grupos de contenedores. Cuando implementa por primera vez un grupo de contenedores en una subred, Azure delega esa subred para Azure Container Instances. Una vez delegada, la subred se puede usar solo para grupos de contenedores. Si intenta implementar recursos distintos de grupos de contenedores en una subred delegada, se produce un error en la operación.

### <a name="network-profile"></a>Perfil de red

Un perfil de red es una plantilla de configuración de red para los recursos de Azure. Especifica determinadas propiedades de red para el recurso, por ejemplo, la subred en la que debe implementarse. La primera vez que usa el comando [az container create][az-container-create] para implementar un grupo de contenedores en una subred (y, por tanto, en una red virtual), Azure crea un perfil de red para usted. A continuación, puede usar ese perfil de red para implementaciones futuras en la subred. 

Para usar una plantilla de Resource Manager, el archivo YAML o un método de programación para implementar un grupo de contenedores en una subred, deberá proporcionar el identificador de recurso de Resource Manager completo de un perfil de red. Puede usar un perfil que creó previamente mediante [az container create][az-container-create] o bien crear un perfil mediante una plantilla de Resource Manager (consulte el [ejemplo de plantilla](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet) y la [referencia](https://docs.microsoft.com/azure/templates/microsoft.network/networkprofiles)). Para obtener el identificador de un perfil creado anteriormente, use el comando [az network profile list][az-network-profile-list]. 

En el diagrama siguiente, se han implementado varios grupos de contenedores en una subred delegada en Azure Container Instances. Una vez implementado un grupo de contenedores en una subred, puede implementar grupos de contenedores adicionales en él si especifica el mismo perfil de red.

![Grupos de contenedores dentro de una red virtual][aci-vnet-01]

## <a name="deployment-scenarios"></a>Escenarios de implementación

Puede usar el comando [az container create][az-container-create] para implementar grupos de contenedores en una nueva red virtual y permitir a Azure crear los recursos de red necesarios en su lugar, o bien implementarlos en una red virtual existente. 

### <a name="new-virtual-network"></a>Nueva red virtual

Para implementarlos en una nueva red virtual y hacer que Azure cree automáticamente los recursos de red, especifique lo siguiente al ejecutar [az container create][az-container-create]:

* Nombre de la red virtual
* Prefijo de dirección de red virtual en formato CIDR
* Nombre de subred
* Prefijo de dirección de subred en formato CIDR

Los prefijos de direcciones de subred y red virtual especifican los espacios de direcciones para la subred y la red virtual, respectivamente. Estos valores se representan en notación de Enrutamiento de interdominios sin clases (CIDR), por ejemplo `10.0.0.0/16`. Para más información sobre cómo trabajar con las subredes, consulte [Incorporación, cambio o eliminación de una subred de red virtual](../virtual-network/virtual-network-manage-subnet.md).

Cuando haya implementado el primer grupo de contenedores con este método, puede implementarlo en la misma subred; para ello, especifique los nombres de la red virtual y la subred, o el perfil de red que Azure crea automáticamente para usted. Dado que Azure delega la subred en Azure Container Instances, *solo* puede implementar grupos de contenedores en la subred.

### <a name="existing-virtual-network"></a>Red virtual existente

Para implementar un grupo de contenedores en una red virtual existente:

1. Cree una subred en la red virtual existente, use una subred existente en la que ya se haya implementado un grupo de contenedores, o bien use una subred existente vacía de *todos* los demás recursos
1. Implemente un grupo de contenedores con [az container create][az-container-create] y especifique uno de los siguientes:
   * Nombre de red virtual y nombre de subred
   * El identificador de recurso de red virtual y el identificador de recurso de subred, lo permite usar una red virtual desde otro grupo de recursos
   * El identificador o el nombre del perfil de red, que puede obtener mediante el comando [az network profile list][az-network-profile-list]

Una vez implemente su primer grupo de contenedores en una subred, Azure delega esa subred a Azure Container Instances. Ya no puede implementar recursos distintos de grupos de contenedores para esa subred.

## <a name="deployment-examples"></a>Ejemplos de implementación

Las secciones siguientes describen cómo implementar grupos de contenedores en una red virtual con la CLI de Azure. En los ejemplos de comandos se da formato para el shell de **Bash**. Si prefiere otro shell como PowerShell o el símbolo del sistema, ajuste los caracteres de continuación de línea según corresponda.

### <a name="deploy-to-a-new-virtual-network"></a>Implementación en una nueva red virtual

En primer lugar, implemente un grupo de contenedores y especifique los parámetros para una nueva red virtual y subred. Cuando especifique estos parámetros, Azure crea la red virtual y la subred, delega la subred en Azure Container Instances y también crea un perfil de red. Una vez creados estos recursos, el grupo de contenedores se implementa en la subred.

Ejecute el siguiente comando [az container create][az-container-create] que especifica valores para una nueva red virtual y subred. Tendrá que proporcionar el nombre de un grupo de recursos que se haya creado en una región en la que haya [disponibles](#virtual-network-deployment-limitations) implementaciones de grupo de contenedores en una red virtual. Este comando implementa el contenedor [aci-helloworld][aci-helloworld] público de Microsoft que ejecuta un pequeño servidor web de Node.js que sirve una página web estática. En la siguiente sección, implementará un segundo grupo de contenedores en la misma subred y probará la comunicación entre las dos instancias de contenedor.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --vnet aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Al implementar en una nueva red virtual mediante este método, la implementación puede tardar unos minutos mientras se crean los recursos de red. Tras la implementación inicial, las implementaciones de grupos de contenedores adicionales se completan más rápido.

### <a name="deploy-to-existing-virtual-network"></a>Implementación en una red virtual existente

Ahora que ha implementado un grupo de contenedores en una nueva red virtual, implemente un segundo grupo en la misma subred y compruebe la comunicación entre las dos instancias.

En primer lugar, obtenga la dirección IP del primer grupo de contenedores que implementó, *appcontainer*:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

La salida debe mostrar la dirección IP del grupo de contenedores en la subred privada:

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Ahora, establezca `CONTAINER_GROUP_IP` en la dirección IP que recuperó con el comando `az container show` y ejecute el siguiente comando `az container create`. Este segundo contenedor, *commchecker*, ejecuta una imagen basada en Linux de Alpine y `wget` con la dirección IP de la subred privada del primer grupo de contenedores.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet aci-vnet \
    --subnet aci-subnet
```

Una vez completada esta segunda implementación de contenedor, extraiga sus registros para que pueda ver la salida del comando `wget` que ejecutó:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

Si el segundo contenedor se comunicó correctamente con el primero, la salida debe ser similar a la siguiente:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

La salida del registro debe mostrar que `wget` fue capaz de conectarse y descargar el archivo de índice del primer contenedor usando su dirección IP privada en la subred local. El tráfico de red entre los dos grupos de contenedores permanece dentro de la red virtual.

### <a name="deploy-to-existing-virtual-network---yaml"></a>Implementación en una red virtual existente: YAML

También puede implementar un grupo de contenedores en una red virtual existente mediante un archivo YAML, una plantilla de Resource Manager u otro método de programación como con el SDK de Python. Para implementar en una subred de una red virtual, especifique varias propiedades adicionales en el archivo YAML:

* `ipAddress`: la configuración de direcciones IP para el grupo de contenedores.
  * `ports`: los puertos que deben abrirse, si hay alguno.
  * `protocol`: el protocolo (TCP o UDP) para el puerto abierto.
* `networkProfile`: especifica la configuración de red, como la red virtual y la subred para un recurso de Azure.
  * `id`: el identificador de recurso del Administrador de recursos completo de `networkProfile`.

Para implementar un grupo de contenedores en una red virtual con un archivo YAML, primero deberá obtener el identificador del perfil de red. Ejecute el comando [az network profile list][az-network-profile-list], especificando el nombre del grupo de recursos que contenga la red virtual y la subred delegada.

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

La salida del comando muestra el identificador de recurso completo para el perfil de red:

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

Una vez que tenga el identificador del perfil de red, copie el siguiente YAML en un nuevo archivo denominado *vnet-deploy-aci.yaml*. En `networkProfile`, reemplace el valor de `id` con el identificador recién recuperado y, después, guarde el archivo. Este YAML crea un grupo de contenedores denominado *appcontaineryaml* en la red virtual.

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Implemente el grupo de contenedores con el comando [az container create][az-container-create] y especifique el nombre del archivo YAML para el parámetro `--file`:

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

Una vez completada la implementación, ejecute el comando [az container show][az-container-show] para mostrar su estado:

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                                       IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   mcr.microsoft.com/azuredocs/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>Limpieza de recursos

### <a name="delete-container-instances"></a>Eliminación de instancias de contenedor

Cuando termine de trabajar con las instancias de contenedor que creó, elimínelas con los siguientes comandos:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>Eliminación de recursos de red

Actualmente, esta característica necesita varios comandos adicionales para eliminar los recursos de red que ha creado antes. Si ha usado los comandos de ejemplo en las secciones anteriores de este artículo para crear la red virtual y la subred, puede usar el siguiente script para eliminar esos recursos de red. El script presupone que el grupo de recursos contiene una sola red virtual con un único perfil de red.

Antes de ejecutar el script, establezca la variable `RES_GROUP` en el nombre del grupo de recursos que contenga la red virtual y la subred que se deben eliminar. Actualice el nombre de la red virtual si no usó el nombre `aci-vnet` que se sugirió anteriormente. El script tiene el formato adecuado para el shell de Bash. Si prefiere otro shell como PowerShell o el símbolo del sistema, deberá ajustar los descriptores de acceso y la asignación de variables según corresponda.

> [!WARNING]
> Este script elimina los recursos. Elimina la red virtual y todas las subredes que contiene. Asegúrese de que ya no necesita *ninguno* de los recursos en la red virtual, incluidas las subredes que contiene, antes de ejecutar este script. Una vez eliminado, **estos recursos son irrecuperables**.

```azurecli
# Replace <my-resource-group> with the name of your resource group
# Assumes one virtual network in resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
# Assumes one profile in virtual network
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>Pasos siguientes

Para implementar una nueva red virtual, subred, perfil de red y grupo de contenedores mediante una plantilla de Resource Manager, consulte el artículo sobre la [creación de un grupo de contenedores de Azure con VNet](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
).

En este artículo se trataron varios recursos de red virtual y características, aunque de forma breve. La documentación de Azure Virtual Network trata estos temas ampliamente:

* [Red virtual](../virtual-network/manage-virtual-network.md)
* [Subred](../virtual-network/virtual-network-manage-subnet.md)
* [Puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list
