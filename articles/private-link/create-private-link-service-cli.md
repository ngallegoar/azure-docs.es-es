---
title: Creación de un servicio Private Link de Azure mediante la CLI de Azure
description: Aprenda a crear un servicio Private Link de Azure mediante la CLI de Azure.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: how-to
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 2cfc746d883b565fe7a082a316ce314f385225df
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91358209"
---
# <a name="create-a-private-link-service-using-azure-cli"></a>Creación de un servicio Private Link mediante la CLI de Azure
En este artículo se muestra cómo crear un servicio Private Link en Azure mediante la CLI de Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si, en su lugar, decide instalar y usar la CLI de Azure en un entorno local, para este inicio rápido debe utilizar la versión más reciente de la CLI de Azure. Ejecute `az --version` para buscar la versión instalada. Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli) para obtener información sobre la instalación o actualización.
## <a name="create-a-private-link-service"></a>Creación de un servicio Private Link
### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para poder crear una red virtual, debe crear un grupo de recursos que hospede la red virtual. Cree un grupo de recursos con [az group create](/cli/azure/group). En el ejemplo, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *westcentralus*:

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
```
### <a name="create-a-virtual-network"></a>Creación de una red virtual
Cree la red virtual con el comando [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). En este ejemplo se crea una red virtual predeterminada denominada *myVirtualNetwork* con una subred denominada *mySubnet*:

```azurecli-interactive
az network vnet create --resource-group myResourceGroup --name myVirtualNetwork --address-prefix 10.0.0.0/16  
```
### <a name="create-a-subnet"></a>Creación de una subred
Cree una subred para la red virtual con [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create). En este ejemplo se crea una subred denominada *mySubnet* en la red virtual *myVirtualNetwork*:

```azurecli-interactive
az network vnet subnet create --resource-group myResourceGroup --vnet-name myVirtualNetwork --name mySubnet --address-prefixes 10.0.0.0/24    
```
### <a name="create-a-internal-load-balancer"></a>Creación de un equilibrador de carga interno 
Cree un equilibrador de carga interno con [az network lb create](/cli/azure/network/lb#az-network-lb-create). En este ejemplo se crea un equilibrador de carga interno denominado *myILB* en un grupo de recursos denominado *myResourceGroup*. 

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myILB --sku standard --vnet-name MyVirtualNetwork --subnet mySubnet --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool
```

### <a name="create-a-load-balancer-health-probe"></a>Creación del sondeo de estado de un equilibrador de carga

Un sondeo de estado comprueba todas las instancias de máquina virtual para asegurarse de que pueden recibir tráfico de red. La instancia de máquina virtual con comprobaciones de sondeo incorrectas se elimina del equilibrador de carga hasta que vuelve a estar en línea y una comprobación de sondeo determina que es correcta. Cree un sondeo de estado con [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) para supervisar el estado de las máquinas virtuales. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroup \
    --lb-name myILB \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Una regla de equilibrador de carga define la configuración de la dirección IP de front-end para el tráfico entrante y el grupo de direcciones IP de back-end para recibir el tráfico, junto con el puerto de origen y destino requeridos. Cree una regla de equilibrador de carga *myHTTPRule* con [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) para escuchar el puerto 80 en el grupo de servidores front-end *myFrontEnd* y enviar tráfico de red con equilibrio de carga al conjunto de direcciones back-end *myBackEndPool*, que también usan el puerto 80. 

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroup \
    --lb-name myILB \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe  
```
### <a name="create-backend-servers"></a>Creación de servidores back-end

En este ejemplo no se trata la creación de máquinas virtuales. Puede seguir los pasos descritos [Creación de un equilibrador de carga interno para equilibrar la carga de las máquinas virtuales con la CLI de Azure](../load-balancer/load-balancer-get-started-ilb-arm-cli.md#create-servers-for-the-backend-address-pool) para crear dos máquinas virtuales que se usarán como servidores de back-end para el equilibrador de carga. 


### <a name="disable-private-link-service-network-policies-on-subnet"></a>Deshabilitación de las directivas de red de un servicio Private Link en una subred 
Un servicio Private Link requiere una dirección IP de cualquier subred de su elección dentro de una red virtual. Actualmente, no se admiten directivas de red en estas direcciones IP.  Por lo tanto, tenemos que deshabilitar las directivas de red en la subred. Actualice la subred para deshabilitar las directivas de red del servicio Private Link con [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update).

```azurecli-interactive
az network vnet subnet update --resource-group myResourceGroup --vnet-name myVirtualNetwork --name mySubnet --disable-private-link-service-network-policies true 
```
 
## <a name="create-a-private-link-service-using-standard-load-balancer"></a>Creación de un servicio Private Link mediante Standard Load Balancer 
 
Cree un servicio Private Link con la configuración IP de front-end de una instancia de Standard Load Balancer con [az network private-link-service create](/cli/azure/network/private-link-service#az-network-private-link-service-create). En este ejemplo se crea un servicio Private Link denominado *myPLS* con una instancia de Standard Load Balancer denominada *myLoadBalancer* en el grupo de recursos llamado *myResourceGroup*. 
 
```azurecli-interactive
az network private-link-service create \
--resource-group myResourceGroup \
--name myPLS \
--vnet-name myVirtualNetwork \
--subnet mySubnet \
--lb-name myILB \
--lb-frontend-ip-configs myFrontEnd \
--location westcentralus 
```
Una vez creado, tome nota del identificador de servicio Private Link. Lo necesitará más adelante para solicitar la conexión a este servicio.  
 
En esta fase, el servicio Private Link se ha creado correctamente y está listo para recibir el tráfico. Tenga en cuenta que en el ejemplo anterior solo se muestra la creación de un servicio Private Link mediante la CLI de Azure.  No hemos configurado los grupos de back-end del equilibrador de carga ni ninguna aplicación en los grupos de back-end para escuchar el tráfico. Si desea ver los flujos de tráfico de un extremo a otro, le recomendamos que configure la aplicación detrás de la instancia local de Standard Load Balancer.  
 
A continuación, se mostrará cómo asignar este servicio a un punto de conexión privado en una red virtual diferente mediante la CLI de Azure. De nuevo, el ejemplo se limita a crear el punto de conexión privado y conectarlo al servicio Private Link creado anteriormente mediante la CLI de Azure. Además, puede crear máquinas virtuales en la red virtual para enviar tráfico al punto de conexión privado o recibirlo de él.        
 
## <a name="private-endpoints"></a>Puntos de conexión privados

### <a name="create-the-virtual-network"></a>Crear la red virtual 
Cree la red virtual con  [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). En este ejemplo, se crea una red virtual denominada  *myPEVNet* en el grupo de recursos llamado *myResourcegroup*: 
```azurecli-interactive
az network vnet create \
--resource-group myResourceGroup \
--name myPEVnet \
--address-prefix 10.0.0.0/16  
```
### <a name="create-the-subnet"></a>Creación de la subred 
Cree una subred en una red virtual con [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create). En este ejemplo se crea una subred denominada  *mySubnet* en la red virtual *myPEVnet* del grupo de recursos *myResourcegroup*: 

```azurecli-interactive 
az network vnet subnet create \
--resource-group myResourceGroup \
--vnet-name myPEVnet \
--name myPESubnet \
--address-prefixes 10.0.0.0/24 
```   
## <a name="disable-private-endpoint-network-policies-on-subnet"></a>Deshabilitación de las directivas de red de un punto de conexión privado en una subred 
Se puede crear un punto de conexión privado en cualquier subred de su elección dentro de una red virtual. Actualmente, no se admiten directivas de red en puntos de conexión privados.  Por lo tanto, tenemos que deshabilitar las directivas de red en la subred. Actualice la subred para deshabilitar las directivas de red del punto de conexión privado con [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update). 

```azurecli-interactive
az network vnet subnet update \
--resource-group myResourceGroup \
--vnet-name myPEVnet \
--name myPESubnet \
--disable-private-endpoint-network-policies true 
```
## <a name="create-private-endpoint-and-connect-to-private-link-service"></a>Creación del punto de conexión privado y conexión al servicio de vínculo privado 
Cree un punto de conexión privado para consumir el servicio Private Link creado anteriormente en la red virtual:
  
```azurecli-interactive
az network private-endpoint create \
--resource-group myResourceGroup \
--name myPE \
--vnet-name myPEVnet \
--subnet myPESubnet \
--private-connection-resource-id {PLS_resourceURI} \
--connection-name myPEConnectingPLS \
--location westcentralus 
```
Puede obtener el parámetro *private-connection-resource-id* con `az network private-link-service show` en el servicio Private Link. El identificador tendrá un aspecto similar a este:   
/subscriptions/subID/resourceGroups/*resourcegroupname*/providers/Microsoft.Network/privateLinkServices/**privatelinkservicename** 
 
## <a name="show-private-link-service-connections"></a>Muestra de conexiones del servicio Private Link 
 
Consulte las solicitudes de conexión en el servicio Private Link mediante [az network private-link-service show](/cli/azure/network/private-link-service#az-network-private-link-service-show).    
```azurecli-interactive 
az network private-link-service show --resource-group myResourceGroup --name myPLS 
```
## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre el [servicio Azure Private Link](private-link-service-overview.md).
 
