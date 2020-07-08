---
title: 'Diagnosticar problemas de enrutamiento en una red de VM: CLI de Azure'
titleSuffix: Azure Network Watcher
description: En este artículo, aprenderá a diagnosticar un problema de enrutamiento en una red de máquinas virtuales mediante la funcionalidad de Próximo salto de Azure Network Watcher.
services: network-watcher
documentationcenter: network-watcher
author: damendo
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: damendo
ms.custom: ''
ms.openlocfilehash: 889db5cdcb1807b859339eaf326e3cec7ea64b84
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84738811"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Diagnosticar problemas de enrutamiento en una red de máquinas virtuales: CLI de Azure

En este artículo implementará una máquina virtual (VM) y comprobará las comunicaciones hacia una dirección IP y una dirección URL. Además, va a determinar la causa de un error de comunicación y cómo puede resolverlo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI de Azure localmente, para este artículo es preciso que ejecute la versión 2.0.28 o posterior de la CLI de Azure. Ejecute `az --version` para ver cuál es la versión instalada. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). Después de verificar la versión de la CLI de Azure, ejecute `az login` para crear una conexión con Azure. Los comandos de la CLI de Azure que aparecen en este artículo tienen un formato que permite ejecutarlos en un shell de Bash.

## <a name="create-a-vm"></a>Crear una VM

Para poder crear una máquina virtual, debe crear un grupo de recursos que contenga la máquina virtual. Cree un grupo de recursos con [az group create](/cli/azure/group#az-group-create). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Cree la máquina virtual con [az vm create](/cli/azure/vm#az-vm-create). Si las claves SSH no existen en la ubicación de claves predeterminada, el comando las crea. Para utilizar un conjunto específico de claves, utilice la opción `--ssh-key-value`. En el ejemplo siguiente, se crea una máquina virtual llamada *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

La máquina virtual tarda en crearse unos minutos. No continúe con los pasos restantes hasta que la máquina virtual se haya creado y la CLI de Azure devuelva la salida.

## <a name="test-network-communication"></a>Prueba de la comunicación de red

Para probar la comunicación de red con Network Watcher, primero debe habilitar un monitor de red en la región en la que quiere probar la máquina virtual y, a continuación, usar la funcionalidad de próximo salto de Network Watcher para probar la comunicación.

### <a name="enable-network-watcher"></a>Habilitación de Network Watcher

Si ya dispone de un monitor de red habilitado en la región Este de EE. UU., pase a la sección [Use el siguiente salto](#use-next-hop). Utilice el comando [az network watcher configure](/cli/azure/network/watcher#az-network-watcher-configure) para crear un monitor de red en la región Este de EE. UU.:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Use el próximo salto

Azure crea rutas automáticamente a los destinos predeterminados. Es posible crear rutas personalizadas que reemplacen las rutas predeterminadas. En ocasiones, las rutas personalizadas pueden provocar errores de comunicación. Para probar el enrutamiento desde una máquina virtual, use [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) para determinar el siguiente salto de enrutamiento cuando el tráfico esté destinado a una dirección específica.

Pruebe la comunicación de salida entre la máquina virtual y una de las direcciones IP de www.bing.com:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Después de unos segundos, el resultado le informa que el valor de **nextHopType** es **Internet** y que el valor de **routeTableId** es **Ruta del sistema**. Este resultado le permite saber que hay una ruta válida para el destino.

Pruebe la comunicación de salida entre la máquina virtual y 172.31.0.100:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

El resultado que se devuelve le informa que **Ninguno** es el valor de **nextHopType** y que el valor de **routeTableId** también es **Ruta del sistema**. Este resultado le permite saber que, aunque no hay una ruta de sistema válida para el destino, no hay ningún próximo salto para enrutar el tráfico al destino.

## <a name="view-details-of-a-route"></a>Vista de los detalles de una ruta

Para analizar el enrutamiento detenidamente, revise las rutas efectivas para la interfaz de red con el comando [az network nic show-effective-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table):

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

El siguiente texto se incluye en el resultado devuelto:

```
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
  },
  "addressPrefix": [
    "0.0.0.0/0"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "Internet",
  "source": "Default",
  "state": "Active"
},
```

Cuando usó el comando `az network watcher show-next-hop` para probar la comunicación saliente a 13.107.21.200 en [Use el próximo salto](#use-next-hop), la ruta con el valor **addressPrefix** 0.0.0.0/0** se usó para enrutar el tráfico a la dirección, ya que ninguna otra ruta en el resultado incluía la dirección. De forma predeterminada, todas las direcciones que no se especifican en el prefijo de dirección de otra ruta se enrutan a internet.

Sin embargo, cuando usó el comando `az network watcher show-next-hop` para probar la comunicación saliente a 172.31.0.100, el resultado le informó de que no había ningún tipo de salto siguiente. En el resultado devuelto puede ver el siguiente texto:

```
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
      },
  "addressPrefix": [
    "172.16.0.0/12"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "None",
  "source": "Default",
  "state": "Active"
},
```

Como puede ver en el resultado del comando `az network watcher nic show-effective-route-table`, aunque hay una ruta predeterminada hacia el prefijo 172.16.0.0/12 (que incluye la dirección 172.31.0.100) el valor de **nextHopType** es **Ninguno**. Azure crea una ruta predeterminada a 172.16.0.0/12, pero no especifica un tipo de próximo salto hasta que haya un motivo para hacerlo. Por ejemplo, si se agrega el intervalo de direcciones 172.16.0.0/12 al espacio de direcciones de la red virtual, Azure cambia el valor de **nextHopType** a **Red Virtual** para la ruta. A continuación, se muestra una marca de verificación de **Red virtual** como valor de **nextHopType**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede utilizar [az group delete](/cli/azure/group#az-group-delete) para eliminar el grupo de recursos y todos los recursos que contenga:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo creó una máquina virtual y diagnosticó el enrutamiento de red a partir de esa máquina virtual. Se informó que Azure crear varias rutas predeterminadas y prueba el enrutamiento a dos destinos diferentes. Obtenga más información sobre [enrutamiento en Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) y cómo [crear rutas personalizadas](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Para las conexiones de máquinas virtuales salientes, también puede determinar la latencia y el tráfico de red permitido o denegado entre la máquina virtual y un punto de conexión mediante la funcionalidad de [resolución de problemas de conexión](network-watcher-connectivity-cli.md) de Network Watcher. Puede supervisar la comunicación entre una máquina virtual y un punto de conexión, como una dirección IP o una URL, gracias a la funcionalidad de supervisión de conexiones de Network Watcher. Para obtener información sobre cómo puede hacer esto, consulte [Supervisar la comunicación de red ](connection-monitor.md).
