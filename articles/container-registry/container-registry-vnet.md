---
title: Restricción del acceso con una red virtual
description: Permita el acceso a un registro de contenedor de Azure solo desde recursos que estén en una red virtual de Azure o desde intervalos de direcciones IP públicas.
ms.topic: article
ms.date: 07/01/2019
ms.openlocfilehash: a6b89b074c25ea0948597ede7e5681b100c7f429
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74454329"
---
# <a name="restrict-access-to-an-azure-container-registry-using-an-azure-virtual-network-or-firewall-rules"></a>Restricción del acceso a un registro de contenedor de Azure mediante una red virtual de Azure o reglas de firewall

[Azure Virtual Network](../virtual-network/virtual-networks-overview.md) proporciona acceso de red seguro y privado a los recursos locales y de Azure. Al limitar el acceso a su registro de contenedor de Azure privado desde una red virtual de Azure, se asegura de que solo acceden al registro los recursos de la red virtual. En los escenarios entre locales, también puede configurar reglas de firewall para permitir el acceso al registro solo desde determinadas direcciones IP.

En este artículo se muestran dos escenarios para configurar reglas de acceso a la red de entrada en un registro de contenedor: desde una máquina virtual implementada en una red virtual, o desde la dirección IP pública de una VM.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar y se [aplican algunas limitaciones](#preview-limitations). Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.
>

Si, en su lugar, necesita configurar reglas de acceso para que los recursos lleguen a un registro de contenedor desde detrás de un firewall, consulte [Configure rules to access an Azure container registry behind a firewall](container-registry-firewall-access-rules.md) (Configuración de reglas para tener acceso a un registro de contenedor de Azure detrás de un firewall).


## <a name="preview-limitations"></a>Limitaciones de vista previa

* Solo se puede configurar un registro de contenedor **Premium** con reglas de acceso a la red. Para obtener información acerca de los niveles de servicio de registro, consulte [SKU de Azure Container Registry](container-registry-skus.md). 

* Para acceder a un registro de contenedor en una red virtual solo se pueden usar como host un clúster de [Azure Kubernetes Service](../aks/intro-kubernetes.md) o una [máquina virtual](../virtual-machines/linux/overview.md) Azure. *Actualmente no se admiten otros servicios de Azure, como Azure Container Instances.*

* Actualmente, las operaciones de [ACR Tasks](container-registry-tasks-overview.md) no se admiten en un registro de contenedor al que se accede en una red virtual.

* Cada registro admite un máximo de cien reglas de red virtual.

## <a name="prerequisites"></a>Prerequisites

* Para usar los pasos de la CLI de Azure de este artículo, se requieren la versión 2.0.58 de la CLI de Azure, o cualquier versión posterior. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli].

* Si aún no tiene un registro de contenedor, créelo (se requiere una SKU Premium) e inserte una imagen de ejemplo, como `hello-world`, desde Docker Hub. Por ejemplo, use [Azure Portal][quickstart-portal] o la [CLI de Azure][quickstart-cli] para crear un registro. 

* Si quiere restringir el acceso al Registro mediante una red virtual en otra suscripción de Azure, tiene que registrar el proveedor de recursos para Azure Container Registry en esa suscripción. Por ejemplo:

  ```azurecli
  az account set --subscription <Name or ID of subscription of virtual network>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

## <a name="about-network-rules-for-a-container-registry"></a>Acerca de las reglas de red para un registro de contenedor

De manera predeterminada los registros de contenedor de aceptan las conexiones a través de Internet de hosts de cualquier red. Con una red virtual, puede permitir que solo accedan de forma segura al registro los recursos de Azure, como un clúster de AKS o una máquina virtual de Azure, sin traspasar un límite de red. También puede configurar reglas de firewall de red para permitir solo rangos concretos de direcciones IP de Internet públicas. 

Para limitar el acceso a un registro, en primer lugar debe cambiar la acción predeterminada del registro para que deniegue todas las conexiones de red. Luego, agregue las reglas de acceso a la red. Los clientes a los que se les ha concedido acceso a través de las reglas de red deben seguir [realizando la autenticación en el registro de contenedor](https://docs.microsoft.com/azure/container-registry/container-registry-authentication) y tener autorización para acceder a los datos.

### <a name="service-endpoint-for-subnets"></a>Punto de conexión de servicio para subredes

Para permitir el acceso desde una subred en una red virtual, es preciso agregar un [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) para el servicio Azure Container Registry. 

Los servicios multiinquilino, como Azure Container Registry, utilizan un único conjunto de direcciones IP para todos los clientes. Un punto de conexión de servicio asigna punto de conexión para acceder a un registro. Este punto de conexión proporciona al tráfico una ruta óptima hasta el recurso a través de una red troncal de Azure. Las identidades de la red virtual y la subred también se transmiten con cada solicitud.

### <a name="firewall-rules"></a>Reglas de firewall

En el caso de las reglas de red IP, especifique los intervalos de dirección de Internet permitidos, para lo que usará la notación CIDR, como *16.17.18.0/24* o una dirección IP individual como *16.17.18.19*. Las reglas de red IP solo se permiten para direcciones IP de Internet *público*. Los rangos de direcciones IP reservados para redes privadas (como se define en RFC 1918) no se permiten en las reglas de IP.

## <a name="create-a-docker-enabled-virtual-machine"></a>Creación de una máquina virtual con funcionalidad Docker

Para este artículo, use una máquina virtual Ubuntu con funcionalidad Docker para acceder a un registro de contenedor de Azure. Para usar la autenticación de Azure Active Directory en el registro, instale también la [CLI de Azure][azure-cli] en la máquina virtual. Si ya tiene una máquina virtual de Azure, omita este paso.

Puede usar el mismo grupo de recursos para la máquina virtual y el registro de contenedor. Esta configuración simplifica la limpieza al final, pero no es necesaria. Si elige crear un grupo de recursos independiente para la máquina virtual y la red virtual, ejecute [az group create][az-group-create]. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *westcentralus*:

```azurecli
az group create --name myResourceGroup --location westus
```

Ahora, implemente una máquina virtual de Azure con Ubuntu mediante [az vm create][az-vm-create]. En el ejemplo siguiente, se crea una máquina virtual llamada *myDockerVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

La máquina virtual tarda unos minutos en crearse. Cuando se complete el comando, anote el valor `publicIpAddress` mostrado por la CLI de Azure. Use esta dirección para crear conexiones SSH con la máquina virtual y, opcionalmente, para la posterior configuración de reglas del firewall.

### <a name="install-docker-on-the-vm"></a>Instalación de Docker en la máquina virtual

Después de ejecutar la máquina virtual, establezca una conexión SSH con la máquina virtual. Reemplace *publicIpAddress* por la dirección IP pública de la máquina virtual.

```bash
ssh azureuser@publicIpAddress
```

Ejecute el siguiente comando para instalar Docker en la máquina virtual Ubuntu:

```bash
sudo apt install docker.io -y
```

Después de la instalación, ejecute el siguiente comando para comprobar que Docker se ejecute correctamente en la máquina virtual:

```bash
sudo docker run -it hello-world
```

Salida:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure

Siga los pasos del artículo [Instalación de la CLI de Azure con apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) para instalar la CLI de Azure en la máquina virtual de Ubuntu. Para este artículo, asegúrese de instalar la versión 2.0.58, o cualquier versión posterior.

Salga de la conexión SSH.

## <a name="allow-access-from-a-virtual-network"></a>Concesión de acceso desde una red virtual

En esta sección se configura el registro de contenedor para permitir el acceso desde una subred de una red virtual de Azure. Se proporcionan los pasos equivalentes si se usa mediante la CLI de Azure y Azure Portal.

### <a name="allow-access-from-a-virtual-network---cli"></a>Concesión de acceso desde una red virtual: CLI

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Incorporación de un punto de conexión de servicio a una subred

Al crear una máquina virtual, Azure crea de manera predeterminada crea una red virtual en el mismo grupo de recursos. El nombre de dicha red virtual se basa en el nombre de la máquina virtual. Por ejemplo, si asigna a la máquina virtual el nombre *myDockerVM*, el nombre predeterminado de la red virtual es *myDockerVMVNET*, con una subred llamada *myDockerVMSubnet*. Compruébelo en Azure Portal o mediante el comando [az network vnet list][az-network-vnet-list]:

```azurecli
az network vnet list --resource-group myResourceGroup --query "[].{Name: name, Subnet: subnets[0].name}"
```

Salida:

```console
[
  {
    "Name": "myDockerVMVNET",
    "Subnet": "myDockerVMSubnet"
  }
]
```

Use el comando [az network vnet subnet update][az-network-vnet-subnet-update] para agregar un punto de conexión de servicio **Microsoft.ContainerRegistry** a la subred. Sustituya los nombres de la red virtual y de la subred en el siguiente comando:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Use el comando [az network vnet subnet show][az-network-vnet-subnet-show] para recuperar el identificador de recurso de la subred. Lo necesitará en un paso posterior para configurar una regla de acceso a la red.

```azurecli
az network vnet subnet show \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --query "id"
  --output tsv
``` 

Salida:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="change-default-network-access-to-registry"></a>Cambio del acceso de red predeterminado al registro

De manera predeterminada los registros de contenedor Azure permiten conexiones de hosts de cualquier red. Para limitar el acceso a una red seleccionada, cambie la acción predeterminada para denegar el acceso. Sustituya el nombre del registro en el siguiente comando [az acr update][az-acr-update]:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Incorporación de una regla de red al registro

Use el comando [az acr network-rule add][az-acr-network-rule-add] para agregar una regla de red al registro que permita el acceso desde la subred de la máquina virtual. Sustituya el nombre del registro de contenedor y el identificador de recurso de la subred en el siguiente comando: 

 ```azurecli
az acr network-rule add --name mycontainerregistry --subnet <subnet-resource-id>
```

Pase a [Comprobación del acceso al registro](#verify-access-to-the-registry).

### <a name="allow-access-from-a-virtual-network---portal"></a>Concesión de acceso desde una red virtual: portal

#### <a name="add-service-endpoint-to-subnet"></a>Incorporación de un punto de conexión de servicio a una subred

Al crear una máquina virtual, Azure crea de manera predeterminada crea una red virtual en el mismo grupo de recursos. El nombre de dicha red virtual se basa en el nombre de la máquina virtual. Por ejemplo, si asigna a la máquina virtual el nombre *myDockerVM*, el nombre predeterminado de la red virtual es *myDockerVMVNET*, con una subred llamada *myDockerVMSubnet*.

Para agregar un punto de conexión de servicio para Azure Container Registry a una subred:

1. En el cuadro de búsqueda de la parte superior de [Azure Portal][azure-portal], escriba *redes virtuales*. Cuando aparezca la opción **Redes virtuales** en los resultados de la búsqueda, selecciónela.
1. En la lista de redes virtuales, seleccione la red virtual en la que está implementada la máquina virtual, como *myDockerVMVNET*.
1. En **Configuración**, seleccione **Subredes**.
1. Seleccione la subred en la que está implementada la máquina virtual, como *myDockerVMSubnet*.
1. En **Puntos de conexión de servicio** , seleccione **Microsoft.ContainerRegistry**.
1. Seleccione **Guardar**.

![Incorporación de un punto de conexión de servicio a una subred][acr-subnet-service-endpoint] 

#### <a name="configure-network-access-for-registry"></a>Configuración el acceso de red al registro

De manera predeterminada los registros de contenedor Azure permiten conexiones de hosts de cualquier red. Para limitar el acceso a la red virtual:

1. En el portal, vaya al registro de contenedor.
1. En **Configuración**, seleccione **Firewall y redes virtuales**.
1. Para denegar el acceso de forma predeterminada, elija permitir el acceso desde **Redes seleccionadas**. 
1. Seleccione **Agregar red virtual existente** y seleccione la red virtual y la subred que configuró con un punto de conexión de servicio. Seleccione **Agregar**.
1. Seleccione **Guardar**.

![Configuración de una red virtual para el registro de contenedor][acr-vnet-portal]

Pase a [Comprobación del acceso al registro](#verify-access-to-the-registry).

## <a name="allow-access-from-an-ip-address"></a>Concesión de acceso desde una dirección IP

En esta sección se configura el registro de contenedor para permitir el acceso desde una dirección IP específica o desde un rango de direcciones IP. Se proporcionan los pasos equivalentes si se usa mediante la CLI de Azure y Azure Portal.

### <a name="allow-access-from-an-ip-address---cli"></a>Concesión de acceso desde una dirección IP: CLI

#### <a name="change-default-network-access-to-registry"></a>Cambio del acceso de red predeterminado al registro

Si aún no lo ha hecho, actualice la configuración del registro para denegar el acceso de manera predeterminada. Sustituya el nombre del registro en el siguiente comando [az acr update][az-acr-update]:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="remove-network-rule-from-registry"></a>Eliminación de una regla de red del registro

Si anteriormente agregó una regla de red para permitir el acceso desde la subred de la máquina virtual, quite el punto de conexión de servicio de la subred y la regla de red. Sustituya el nombre del registro de contenedor y el identificador de recurso de la subred que recuperó en el paso anterior en el comando [az acr network-rule remove][az-acr-network-rule-remove]: 

```azurecli
# Remove service endpoint

az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints ""

# Remove network rule

az acr network-rule remove --name mycontainerregistry --subnet <subnet-resource-id>
```

#### <a name="add-network-rule-to-registry"></a>Incorporación de una regla de red al registro

Use el comando [az acr network-rule add][az-acr-network-rule-add] para agregar una regla de red al registro que permita el acceso desde la dirección IP de la máquina virtual. Sustituya el nombre del registro de contenedor y la dirección IP pública de la máquina virtual en el siguiente comando.

```azurecli
az acr network-rule add --name mycontainerregistry --ip-address <public-IP-address>
```

Pase a [Comprobación del acceso al registro](#verify-access-to-the-registry).

### <a name="allow-access-from-an-ip-address---portal"></a>Concesión de acceso desde una dirección IP: portal

#### <a name="remove-existing-network-rule-from-registry"></a>Eliminación de una regla de red existente del registro

Si anteriormente agregó una regla de red para permitir el acceso desde la subred de la máquina virtual, quite la regla existente. Omita esta sección si desea acceder al registro desde otra máquina virtual.

* Actualice la configuración de la subred para quitar el punto de conexión de servicio de la misma para Azure Container Registry. 

  1. En [Azure Portal][azure-portal], navegue a la red virtual en la que está implementada la máquina virtual.
  1. En **Configuración**, seleccione **Subredes**.
  1. Seleccione la subred en la que está implementada la máquina virtual.
  1. En **Puntos de conexión de servicio**, desactive la casilla **Microsoft.ContainerRegistry**. 
  1. Seleccione **Guardar**.

* Quite la regla de red que permite a la subred acceder al registro.

  1. En el portal, vaya al registro de contenedor.
  1. En **Configuración**, seleccione **Firewall y redes virtuales**.
  1. En **Redes virtuales**, seleccione el nombre de la red virtual y, después, seleccione **Quitar**.
  1. Seleccione **Guardar**.

#### <a name="add-network-rule-to-registry"></a>Incorporación de una regla de red al registro

1. En el portal, vaya al registro de contenedor.
1. En **Configuración**, seleccione **Firewall y redes virtuales**.
1. Si aún no lo ha hecho, elija permitir el acceso desde **Redes seleccionadas**. 
1. En **Redes virtuales**, asegúrese de que no hay ninguna red seleccionada.
1. En **Firewall**, escriba la dirección IP pública de una máquina virtual. O bien, escriba un rango de direcciones en notación CIDR que contenga la dirección IP de la máquina virtual.
1. Seleccione **Guardar**.

![Configurar una regla de firewall para el registro de contenedor][acr-vnet-firewall-portal]

Pase a [Comprobación del acceso al registro](#verify-access-to-the-registry).

## <a name="verify-access-to-the-registry"></a>Comprobación del acceso al registro

Después de esperar unos minutos para que se actualice la configuración, compruebe que la máquina virtual puede acceder al registro de contenedor. Cree una conexión SSH con la máquina virtual y ejecute el comando [az acr login][az-acr-login] para iniciar sesión en el registro. 

```bash
az acr login --name mycontainerregistry
```

Puede realizar las operaciones de registro, como ejecutar `docker pull` para extraer una imagen de ejemplo del registro. Sustituya una imagen y el valor de la etiqueta pertinente para el registro, y anteponga el nombre del servidor de inicio de sesión del registro (todo en minúsculas):

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

Docker extrae correctamente la imagen a la máquina virtual.

En este ejemplo se muestra que puede acceder al registro de contenedor privado a través de la regla de acceso a la red. Sin embargo, no se puede acceder al registro desde otro host de inicio de sesión que no tenga configurada una regla de acceso de red. Si intenta iniciar sesión desde otro host mediante el comando `az acr login` o el comando `docker login`, el resultado será similar al siguiente:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Restauración del acceso al registro predeterminado

Para restaurar el registro para permitir el acceso de forma predeterminada, quite todas las reglas de red configuradas. Luego, establezca la acción predeterminado en permitir el acceso. Se proporcionan los pasos equivalentes si se usa mediante la CLI de Azure y Azure Portal.

### <a name="restore-default-registry-access---cli"></a>Restauración del acceso al registro predeterminado: CLI

#### <a name="remove-network-rules"></a>Eliminación de reglas de red

Para ver una lista de reglas de red configuradas para el registro, ejecute el siguiente comando [az acr network-rule list][az-acr-network-rule-list]:

```azurecli
az acr network-rule list--name mycontainerregistry 
```

En cada regla que esté configurada, ejecute el comando [az acr network-rule remove][az-acr-network-rule-remove] para quitarla. Por ejemplo:

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet

# Remove a rule that allows access for an IP address or CIDR range such as 23.45.1.0/24.

az acr network-rule remove \
  --name mycontainerregistry \
  --ip-address 23.45.1.0/24
```

#### <a name="allow-access"></a>Permitir acceso

Sustituya el nombre del registro en el siguiente comando [az acr update][az-acr-update]:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

### <a name="restore-default-registry-access---portal"></a>Restauración del acceso al registro predeterminado: portal


1. En el portal, vaya al registro de contenedor y seleccione **Firewall y redes virtuales**.
1. En **Redes virtuales**, seleccione cada una de las redes virtuales y, después, seleccione **Quitar**.
1. En **Firewall**, seleccione cada rango de direcciones y, después, seleccione el icono Eliminar.
1. En **Permitir acceso desde**, seleccione **Todas las redes**. 
1. Seleccione **Guardar**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha creado todos los recursos de Azure en el mismo grupo de recursos y ya no los necesita, puede eliminarlos con el comando [az group delete](/cli/azure/group):

```azurecli
az group delete --name myResourceGroup
```

Para limpiar los recursos en el portal, vaya al grupo de recursos myResourceGroup. Una vez que se ha cargado el grupo de recursos, haga clic en **Eliminar grupo de recursos** para quitar el grupo de recursos y los recursos almacenados en él.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se trataron varios recursos de red virtual y características, aunque de forma breve. La documentación de Azure Virtual Network trata estos temas ampliamente:

* [Red virtual](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network)
* [Subred](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
* [Puntos de conexión de servicio](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png
[acr-vnet-portal]: ./media/container-registry-vnet/acr-vnet-portal.png
[acr-vnet-firewall-portal]: ./media/container-registry-vnet/acr-vnet-firewall-portal.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
