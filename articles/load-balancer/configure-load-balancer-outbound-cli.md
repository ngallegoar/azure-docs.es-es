---
title: Configurar el equilibrio de carga y las reglas de salida con la CLI de Azure
titleSuffix: Azure Load Balancer
description: En este artículo se muestra cómo configurar el equilibrio de carga y las reglas de salida en Standard Load Balancer mediante la CLI de Azure.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: allensu
ms.openlocfilehash: 2b326c6c58b4685b6e73d0f9a641a2f90807d705
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84803744"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-cli"></a>Configurar el equilibrio de carga y las reglas de salida en Standard Load Balancer mediante la CLI de Azure

En este inicio rápido se muestra cómo configurar las reglas de salida en Standard Load Balancer con la CLI de Azure.  

Al terminar, el recurso de Load Balancer contiene dos servidores front-end y las reglas asociadas a estos: una de entrada y otra de salida.  Cada front-end tiene una referencia a una dirección IP pública, y este escenario usa una dirección IP pública diferente para la entrada y la salida.   La regla de equilibrio de carga proporciona solo equilibrio de carga de entrada y la regla de salida controla el proceso NAT de salida proporcionada para la máquina virtual.  En esta guía de inicio rápido se usan dos grupos de back-end independientes, uno para la entrada y otro para salida, a fin de ilustrar las funcionalidades y permitir la flexibilidad para este escenario.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Si decide instalar y usar la CLI localmente, para este tutorial es preciso que ejecute la CLI de Azure versión 2.0.28 o versiones posteriores. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="create-resource-group"></a>Creación de un grupo de recursos

Cree un grupo de recursos con [az group create](https://docs.microsoft.com/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

En el ejemplo siguiente se crea un grupo de recursos denominado *myresourcegroupoutbound* en la ubicación *eastus2*:

```azurecli-interactive
  az group create \
    --name myresourcegroupoutbound \
    --location eastus2
```
## <a name="create-virtual-network"></a>Creación de una red virtual
Cree una red virtual denominada *myvnetoutbound* con una subred llamada *mysubnetoutbound* en *myresourcegroupoutbound* con el comando [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet).

```azurecli-interactive
  az network vnet create \
    --resource-group myresourcegroupoutbound \
    --name myvnetoutbound \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mysubnetoutbound \
    --subnet-prefix 192.168.0.0/24
```

## <a name="create-inbound-public-ip-address"></a>Creación de una dirección IP pública de entrada 

Para obtener acceso a la aplicación web en Internet, necesita una dirección IP pública para el equilibrador de carga. La versión Estándar de Load Balancer solo admite direcciones IP públicas estándar. Use [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) para crear una dirección IP pública estándar denominada *mypublicipinbound* en *myresourcegroupoutbound*.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipinbound --sku standard
```

## <a name="create-outbound-public-ip-address"></a>Creación de una dirección IP pública de salida 

Cree una dirección IP estándar para el comando [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) de configuración de salida del front-end de Load Balancer.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipoutbound --sku standard
```

## <a name="create-azure-load-balancer"></a>Creación de una instancia de Azure Load Balancer

En esta sección se detalla cómo se pueden crear y configurar los componentes siguientes del equilibrador de carga:
  - Una dirección IP de front-end que recibe el tráfico de red entrante en el equilibrador de carga.
  - Un grupo de back-end al que la dirección IP de front-end envía el tráfico de red de carga equilibrada.
  - Un grupo de back-end para la conectividad de salida. 
  - Un sondeo de estado que determina el estado de las instancias de máquina virtual de back-end.
  - Una regla de entrada de equilibrador de carga que define cómo se distribuye el tráfico a las máquinas virtuales.
  - Una regla de salida de equilibrador de carga que define cómo se distribuye el tráfico de las máquinas virtuales.

### <a name="create-load-balancer"></a>Cree un equilibrador de carga

Use [az network lb create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) para crear con la dirección IP de entrada una instancia de Load Balancer denominada *lb* que incluya una configuración de IP de front-end de entrada y un grupo de back-end *bepoolinbound* que esté asociado a la dirección IP pública *mypublicipinbound* que creó en el paso anterior.

```azurecli-interactive
  az network lb create \
    --resource-group myresourcegroupoutbound \
    --name lb \
    --sku standard \
    --backend-pool-name bepoolinbound \
    --frontend-ip-name myfrontendinbound \
    --location eastus2 \
    --public-ip-address mypublicipinbound   
  ```

### <a name="create-outbound-pool"></a>Creación del grupo de salida

Cree un grupo adicional de direcciones de back-end denominado *bepooloutbound* para definir la conectividad de salida para un grupo de máquinas virtuales con [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest).  La creación de un grupo de salida independiente proporciona una flexibilidad máxima, pero también puede omitir este paso y usar solo el grupo *bepoolinbound* de entrada.

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name bepooloutbound
```

### <a name="create-outbound-frontend-ip"></a>Creación de una dirección IP de front-end de salida
Cree la configuración de IP de front-end de salida de Load Balancer con [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) que incluya una configuración de IP de front-end de salida denominada *myfrontendoutbound*, es decir que esté asociada a la dirección IP pública *mypublicipoutbound*

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myresourcegroupoutbound \
    --name myfrontendoutbound \
    --lb-name lb \
    --public-ip-address mypublicipoutbound 
  ```

### <a name="create-health-probe"></a>Creación de un sondeo de estado

Los sondeos de estado comprueban todas las instancias de máquina virtual para asegurarse de que pueden enviar tráfico de red. La instancia de máquina virtual con comprobaciones de sondeo incorrectas se elimina del equilibrador de carga hasta que vuelve a estar en línea y una comprobación de sondeo determina que es correcta. Cree un sondeo de estado con [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) para supervisar el estado de las máquinas virtuales. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name http \
    --protocol http \
    --port 80 \
    --path /  
```

### <a name="create-load-balancing-rule"></a>Creación de una regla de equilibrio de carga

Una regla de equilibrador de carga define la configuración de la dirección IP de front-end para el tráfico entrante y el grupo de back-end para recibir el tráfico, junto con los puertos de origen y destino requeridos. Cree una regla de equilibrador de carga *myinboundlbrule* con [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) para escuchar el puerto 80 en el grupo de servidores front-end *myfrontendinbound* y enviar tráfico de red con equilibrio de carga al conjunto de direcciones de back-end *bepool*, que también usan el puerto 80. 

>[!NOTE]
>Esta regla de equilibrio de carga deshabilita la (S)NAT de salida automática como resultado de esta regla con el parámetro --disable-outbound-snat. La NAT de salida solo la proporciona la regla de salida.

```azurecli-interactive
az network lb rule create \
--resource-group myresourcegroupoutbound \
--lb-name lb \
--name inboundlbrule \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--probe http \
--frontend-ip-name myfrontendinbound \
--backend-pool-name bepoolinbound \
--disable-outbound-snat
```

### <a name="create-outbound-rule"></a>Creación de una regla de salida

Una regla de salida define la IP pública de front-end, representada por el front-end *myfrontendoutbound*, que se usará para todo el tráfico NAT de salida, así como el grupo de back-end al que se aplica esta regla.  Cree una regla de salida *myoutboundrule* para la traducción de red de salida de todas las máquinas virtuales (configuraciones de IP de la NIC) en el grupo de back-end *bepool*.  El comando siguiente también cambia el tiempo de espera de inactividad de salida de 4 a 15 minutos y asigna puertos SNAT 10000 en lugar de 1024.  Consulte las [reglas de salida](https://aka.ms/lboutboundrules) para obtener más información.

```azurecli-interactive
az network lb outbound-rule create \
 --resource-group myresourcegroupoutbound \
 --lb-name lb \
 --name outboundrule \
 --frontend-ip-configs myfrontendoutbound \
 --protocol All \
 --idle-timeout 15 \
 --outbound-ports 10000 \
 --address-pool bepooloutbound
```

Si no quiere usar un grupo de salida independiente, puede cambiar el argumento del grupo de direcciones en el comando anterior y especificar *bepoolinbound*.  Se recomienda usar grupos independientes para garantizar la flexibilidad y la legibilidad de la configuración resultante.

En este momento, puede pasar a agregar la máquina virtual al grupo de back-end *bepoolinbound*__y__*bepooloutbound* mediante la actualización de la configuración de IP de los recursos de la NIC correspondientes con [az network nic ip-config address-pool add](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group#az-group-delete) para quitar el grupo de recursos, el equilibrador de carga y todos los recursos relacionados.

```azurecli-interactive 
  az group delete --name myresourcegroupoutbound
```

## <a name="next-steps"></a>Pasos siguientes
En este artículo, creó su Standard Load Balancer, configuró ambas reglas de tráfico del equilibrador de carga de entrada, configuró las máquinas virtuales y las sometió a pruebas de sondeo en el grupo de back-end. Para más información acerca de Azure Load Balancer, diríjase a los tutoriales correspondientes.

> [!div class="nextstepaction"]
> [Tutoriales de Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
