---
title: 'Inicio rápido: Creación de una instancia pública de Load Balancer con la CLI de Azure'
titleSuffix: Azure Load Balancer
description: Este inicio rápido muestra cómo crear un equilibrador de carga público mediante la CLI de Azure
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
tags: azure-resource-manager
Customer intent: I want to create a Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 1a2d0322436bd91e92a7018552c5827e021ee74e
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85851515"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-using-azure-cli"></a>Inicio rápido: Creación de una instancia de Standard Load Balancer para equilibrar la carga de máquinas virtuales mediante la CLI de Azure

En este tutorial rápido se muestra cómo crear una instancia pública de Load Balancer. Para probar el equilibrador de carga, implemente dos máquinas virtuales (VM) que ejecutan un servidor Ubuntu y equilibre la carga de una aplicación web entre ellas.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Si decide instalar y usar la CLI localmente, para este tutorial es preciso que ejecute la CLI de Azure versión 2.0.28 o versiones posteriores. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con [az group create](https://docs.microsoft.com/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroupSLB* en la ubicación *eastus*:

```azurecli-interactive
  az group create \
    --name myResourceGroupSLB \
    --location eastus
```

## <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

Para obtener acceso a la aplicación web en Internet, necesita una dirección IP pública para el equilibrador de carga. Use [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) para crear una dirección IP pública estándar y con redundancia de zona denominada *myPublicIP* en *myResourceGroupSLB*.

```azurecli-interactive
  az network public-ip create --resource-group myResourceGroupSLB --name myPublicIP --sku standard
```

Para crear una dirección IP pública en la zona 1, utilice:

```azurecli-interactive
  az network public-ip create --resource-group myResourceGroupSLB --name myPublicIP --sku standard --zone 1
```

Utilice `-SKU Basic` para crear una dirección IP pública básica. Las direcciones IP públicas básicas no son compatibles con el **equilibrador de carga estándar**. Microsoft recomienda el uso del **estándar** para cargas de trabajo de producción.

> [!IMPORTANT]
> En el resto de esta guía de inicio rápido se supone que se elige la SKU **estándar** durante el proceso de selección de la SKU anterior.

## <a name="create-azure-load-balancer"></a>Creación de una instancia de Azure Load Balancer

En esta sección se detalla cómo se pueden crear y configurar los componentes siguientes del equilibrador de carga:
  - Un grupo de direcciones IP de front-end que recibe el tráfico de red entrante en el equilibrador de carga.
  - Un grupo de direcciones IP de back-end al que el grupo de servidores front-end envía el tráfico de red de carga equilibrada.
  - Un sondeo de estado que determina el estado de las instancias de máquina virtual de back-end.
  - Una regla de equilibrador de carga que define cómo se distribuye el tráfico a las máquinas virtuales.

### <a name="create-the-load-balancer"></a>Creación del equilibrador de carga

Cree con [az network lb create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) una instancia de Azure Load Balancer pública llamada **myLoadBalancer** que incluya un grupo de servidores front-end llamado **myFrontEnd** y un grupo de servidores back-end llamado **myBackEndPool** que esté asociado a la dirección IP pública **myPublicIP** que creó en el paso anterior. Utilice `--sku basic` para crear una instancia de Load Balancer Básico. Microsoft recomienda la SKU estándar para cargas de trabajo de producción.

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupSLB \
    --name myLoadBalancer \
    --sku standard \
    --public-ip-address myPublicIP \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool       
```

> [!IMPORTANT]
> En el resto de esta guía de inicio rápido se supone que se elige la SKU **estándar** durante el proceso de selección de la SKU anterior.

### <a name="create-the-health-probe"></a>Creación del sondeo de estado

Los sondeos de estado comprueban todas las instancias de máquina virtual para asegurarse de que pueden enviar tráfico de red. La instancia de máquina virtual con comprobaciones de sondeo incorrectas se elimina del equilibrador de carga hasta que vuelve a estar en línea y una comprobación de sondeo determina que es correcta. Cree un sondeo de estado con [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) para supervisar el estado de las máquinas virtuales. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupSLB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>Creación de la regla de equilibrador de carga

Una regla de equilibrador de carga define la configuración de la dirección IP de front-end para el tráfico entrante y el grupo de direcciones IP de back-end para recibir el tráfico, junto con el puerto de origen y destino requeridos. Cree una regla de equilibrador de carga *myLoadBalancerRuleWeb* con [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) para escuchar el puerto 80 en el grupo de servidores front-end *myFrontEnd* y enviar tráfico de red con equilibrio de carga al conjunto de direcciones back-end *myBackEndPool*, que también usan el puerto 80. 

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupSLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEnd \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe  
```

## <a name="configure-virtual-network"></a>Configurar la red virtual

Antes de implementar algunas máquinas virtuales y poder probar el equilibrador de carga, cree los recursos de red virtual auxiliares.

### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree una red virtual llamada *myVnet* con una subred llamada *mySubnet* en *myResourceGroup* con el comando [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet).

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupSLB \
    --location eastus \
    --name myVnet \
    --subnet-name mySubnet
```

### <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

En el caso de una instancia de Load Balancer Estándar, las máquinas virtuales de la dirección de back-end deben tener tarjetas NIC que pertenezcan a un grupo de seguridad de red. Cree un grupo de seguridad de red para definir las conexiones entrantes a la red virtual.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupSLB \
    --name myNetworkSecurityGroup
```

### <a name="create-a-network-security-group-rule"></a>Creación de una regla de grupo de seguridad de red

Cree una regla de grupo de seguridad de red para permitir las conexiones entrantes a través del puerto 80.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupSLB \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleHTTP \
    --protocol tcp \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```

### <a name="create-nics"></a>Creación de tarjetas NIC

Cree tres interfaces de red con el comando [az network nic create](/cli/azure/network/nic#az-network-nic-create) y asócielas con la dirección IP pública y el grupo de seguridad de red. 

```azurecli-interactive

  az network nic create \
    --resource-group myResourceGroupSLB \
    --name myNicVM1 \
    --vnet-name myVnet \
    --subnet mySubnet \
    --network-security-group myNetworkSecurityGroup \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool

  az network nic create \
    --resource-group myResourceGroupSLB \
    --name myNicVM2 \
    --vnet-name myVnet \
    --subnet mySubnet \
    --network-security-group myNetworkSecurityGroup \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
  
  az network nic create \
    --resource-group myResourceGroupSLB \
    --name myNicVM3 \
    --vnet-name myVnet \
    --subnet mySubnet \
    --network-security-group myNetworkSecurityGroup \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool

```

## <a name="create-backend-servers"></a>Creación de servidores back-end

En este ejemplo, se crean tres máquinas virtuales que se usarán como servidores back-end para el equilibrador de carga. Para comprobar que el equilibrador de carga se ha creado correctamente, también se debe instalar NGINX en las máquinas virtuales.

Si va a crear una instancia de Load Balancer Básico con una dirección IP pública básica, tendrá que crear un conjunto de disponibilidad mediante [az vm availabilityset create](/cli/azure/network/nic) para agregar las máquinas virtuales. Las instancias de Standard Load Balancer no requieren este paso adicional. Microsoft recomienda usar las instancias Standard.

### <a name="create-three-virtual-machines"></a>Creación de tres máquinas virtuales

Puede usar un archivo de configuración cloud-init para instalar NGINX y ejecutar una aplicación Node.js "Hola mundo" en una máquina virtual Linux. En el shell actual, cree un archivo denominado cloud-init.txt, y copie y pegue la siguiente configuración en el shell. Asegúrese de copiar correctamente todo el archivo cloud-init, especialmente la primera línea:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

Cree las máquinas virtuales con [az vm create](/cli/azure/vm#az-vm-create).

```azurecli-interactive

  az vm create \
    --resource-group myResourceGroupSLB \
    --name myVM1 \
    --availability-set myAvailabilitySet \
    --nics myNicVM1 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --no-wait
   
  az vm create \
    --resource-group myResourceGroupSLB \
    --name myVM2 \
    --availability-set myAvailabilitySet \
    --nics myNicVM2 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --no-wait

   az vm create \
    --resource-group myResourceGroupSLB \
    --name myVM3 \
    --availability-set myAvailabilitySet \
    --nics myNicVM3 \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt \
    --no-wait

```

Es posible que la implementación de las máquinas virtuales tarde unos minutos.

## <a name="test-the-load-balancer"></a>Prueba del equilibrador de carga

Para obtener la dirección IP pública del equilibrador de carga, use [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show). Copie la dirección IP pública y péguela en la barra de direcciones del explorador.

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupSLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

   ![Prueba del equilibrador de carga](./media/load-balancer-standard-public-cli/running-nodejs-app.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group#az-group-delete) para quitar el grupo de recursos, el equilibrador de carga y todos los recursos relacionados.

```azurecli-interactive
  az group delete --name myResourceGroupSLB
```

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado una instancia de Standard Load Balancer, le ha asociado máquinas virtuales, ha configurado la regla de tráfico de Load Balancer y el sondeo de estado y, después, ha probado la instancia de Load Balancer. Para más información sobre Azure Load Balancer, consulte los [tutoriales sobre Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).

Más información sobre [Load Balancer y zonas de disponibilidad](load-balancer-standard-availability-zones.md).
