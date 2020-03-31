---
title: Configuración del equilibrio de carga y las reglas de salida con Azure PowerShell
titleSuffix: Azure Load Balancer
description: En este artículo se muestra cómo configurar el equilibrio de carga y las reglas de salida en Standard Load Balancer mediante Azure PowerShell.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 5fd68f4559420ca688b3f4d6f6d66ee52db5191e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225437"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-azure-powershell"></a>Configuración del equilibrio de carga y las reglas de salida en Standard Load Balancer mediante Azure PowerShell

En este artículo se muestra cómo configurar las reglas de salida en Standard Load Balancer mediante Azure PowerShell.  

Al finalizar el escenario de este artículo, el recurso del equilibrador de carga contendrá dos front-ends y las reglas asociadas. Tiene un front-end para el tráfico entrante y otro para el saliente.  

Cada front-end hace referencia a una dirección IP pública. En este escenario, la dirección IP pública para el tráfico entrante difiere de la del tráfico saliente.   La regla de equilibrio de carga solo proporciona equilibrio de carga de entrada. La regla de salida controla la traducción de direcciones de red (NAT) de salida de la máquina virtual.  

En este escenario se usan dos grupos de servidores back-end: uno para el tráfico entrante y otro para el saliente. Estos grupos muestran la funcionalidad y proporcionan flexibilidad para el escenario.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-your-azure-account"></a>Conexión a la cuenta de Azure
Inicie sesión en su suscripción de Azure mediante el comando [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0). Después, siga las instrucciones que aparecen en pantalla.
    
```azurepowershell-interactive
Connect-AzAccount
```
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan los recursos de Azure. Los recursos se administran desde el grupo.

En el ejemplo siguiente se crea un grupo de recursos denominado *myresourcegroupoutbound* en la ubicación *eastus2*:

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroupoutbound -Location eastus
```
## <a name="create-a-virtual-network"></a>Creación de una red virtual
Cree una red virtual llamada *myvnetoutbound*. Asigne un nombre a su subred *mysubnetoutbound*. Colóquela en *myresourcegroupoutbound* mediante [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=azps-2.6.0) y [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=azps-2.6.0).

```azurepowershell-interactive
$subnet = New-AzVirtualNetworkSubnetConfig -Name mysubnetoutbound -AddressPrefix "192.168.0.0/24"

New-AzVirtualNetwork -Name myvnetoutbound -ResourceGroupName myresourcegroupoutbound -Location eastus -AddressPrefix "192.168.0.0/16" -Subnet $subnet
```

## <a name="create-an-inbound-public-ip-address"></a>Creación de una dirección IP pública de entrada 

Para acceder a la aplicación web en Internet, necesita una dirección IP pública para el equilibrador de carga. Una instancia de Standard Load Balancer solo admite direcciones IP públicas estándares. 

Use [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) para crear una dirección IP pública estándar denominada *mypublicipinbound* en *myresourcegroupoutbound*.

```azurepowershell-interactive
$pubIPin = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipinbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-an-outbound-public-ip-address"></a>Creación de una dirección IP pública de salida 

Cree una dirección IP estándar para la configuración de salida del front-end del equilibrador de carga mediante [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0).

```azurepowershell-interactive
$pubIPout = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipoutbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-an-azure-load-balancer"></a>Creación de un equilibrador de carga de Azure

En esta sección se explica cómo crear y configurar los componentes siguientes del equilibrador de carga:
  - Una dirección IP de front-end que recibe el tráfico de red entrante en el equilibrador de carga
  - Un grupo de servidores back-end al que la dirección IP de front-end envía el tráfico de carga equilibrada
  - Un grupo de servidores back-end para la conectividad de salida
  - Un sondeo de estado que determina el estado de las máquinas virtuales de back-end
  - Una regla de entrada de equilibrador de carga que define cómo se distribuye el tráfico a las máquinas virtuales
  - Una regla de salida de equilibrador de carga que define cómo se distribuye el tráfico desde las máquinas virtuales

### <a name="create-an-inbound-front-end-ip"></a>Creación de una IP de front-end de entrada
Cree la configuración de la IP de front-end de entrada para el equilibrador de carga mediante [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0). El equilibrador de carga debe incluir una configuración de dirección IP de front-end de entrada llamada *myfrontendinbound*. Asocie esta configuración a la dirección IP pública *mypublicipinbound*.

```azurepowershell-interactive
$frontendIPin = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendinbound" -PublicIpAddress $pubIPin
```
### <a name="create-an-outbound-front-end-ip"></a>Creación de una IP de front-end de salida
Cree la configuración de la IP de front-end de salida para el equilibrador de carga mediante [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0). Este equilibrador de carga debe incluir una configuración de dirección IP de front-end de salida llamada *myfrontendoutbound*. Asocie esta configuración a la dirección IP pública *mypublicipoutbound*.

```azurepowershell-interactive
$frontendIPout = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendoutbound" -PublicIpAddress $pubIPout
```
### <a name="create-an-inbound-back-end-pool"></a>Creación de un grupo de servidores back-end de entrada
Cree el grupo de direcciones IP de back-end de entrada para el equilibrador de carga con [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0). Asígnele al grupo el nombre *bepoolinbound*.

```azurepowershell-interactive
$bepoolin = New-AzLoadBalancerBackendAddressPoolConfig -Name bepoolinbound
``` 

### <a name="create-an-outbound-back-end-pool"></a>Creación de un grupo de direcciones IP de back-end de salida
Use el comando siguiente para crear otro grupo de direcciones de back-end para definir la conectividad de salida para un grupo de máquinas virtuales con [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0). Asígnele a este grupo el nombre *bepooloutbound*. 

Al crear un grupo de salida independiente, se proporciona la máxima flexibilidad. Sin embargo, puede omitir este paso y usar solo el *bepoolinbound* de entrada si lo prefiere.  

```azurepowershell-interactive
$bepoolout = New-AzLoadBalancerBackendAddressPoolConfig -Name bepooloutbound
``` 

### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

Los sondeos de estado comprueban todas las máquinas virtuales para asegurarse de que pueden enviar tráfico. La máquina virtual que produce comprobaciones de sondeo incorrectas se elimina del equilibrador de carga hasta que vuelve a estar en línea y una comprobación de sondeo determina que es correcta. 

Para supervisar el estado de las máquinas virtuales, cree un sondeo de estado con [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig?view=azps-2.6.0). 

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name http -Protocol "http" -Port 80 -IntervalInSeconds 15 -ProbeCount 2 -RequestPath /
```
### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Una regla de equilibrador de carga define la configuración de la dirección IP de front-end para el tráfico entrante y el grupo de servidores back-end para recibir el tráfico. También define el puerto de origen y de destino requeridos. 

Cree una regla de equilibrador de carga llamada *myinboundlbrule* mediante [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig?view=azps-2.6.0). Esta regla escuchará el puerto 80 en el grupo de servidores front-end *myfrontendinbound*. También usará el puerto 80 para enviar tráfico con equilibrio de carga al grupo de direcciones de back-end *bepoolinbound*. 

```azurepowershell-interactive
$inboundRule = New-AzLoadBalancerRuleConfig -Name inboundlbrule -FrontendIPConfiguration $frontendIPin -BackendAddressPool $bepoolin -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -DisableOutboundSNAT
```

>[!NOTE]
>Esta regla de equilibrio de carga deshabilita la traducción de direcciones de red segura (SNAT) de salida automática por el parámetro **-DisableOutboundSNAT**. La NAT de salida solo la proporciona la regla de salida.

### <a name="create-an-outbound-rule"></a>Cree una regla de salida

Una regla de salida define la dirección IP pública de front-end que está representada por el front-end *myfrontendoutbound*. Este front-end se usará para todo el tráfico NAT de salida, así como para el grupo de servidores back-end al que se aplica la regla.  

Use el comando siguiente para crear una regla de salida *myoutboundrule* para la traducción de red de salida de todas las máquinas virtuales (configuraciones de IP de la NIC) en el grupo de servidores back-end *bepool*.  El comando cambia el tiempo de espera de inactividad de salida de 4 a 15 minutos. Asigna 10 000 puertos SNAT en lugar de 1024. Para más información, consulte [New-AzLoadBalancerOutboundRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalanceroutboundruleconfig?view=azps-2.7.0).

```azurepowershell-interactive
 $outboundRule = New-AzLoadBalancerOutBoundRuleConfig -Name outboundrule -FrontendIPConfiguration $frontendIPout -BackendAddressPool $bepoolout -Protocol All -IdleTimeoutInMinutes 15 -AllocatedOutboundPort 10000
```
Si no quiere usar un grupo de salida independiente, puede cambiar el argumento del grupo de direcciones en el comando anterior y especificar *$bepoolin*.  Se recomienda usar grupos independientes para garantizar la flexibilidad y la legibilidad de la configuración resultante.

### <a name="create-a-load-balancer"></a>Creación de un equilibrador de carga

Use el comando siguiente para crear un equilibrador de carga para la dirección IP de entrada mediante [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer?view=azps-2.6.0). Asígnele al equilibrador de carga el nombre *lb*. Debe incluir una configuración de dirección IP de front-end de entrada. Su grupo de direcciones IP de back-end *bepoolinbound* debe estar asociado a la dirección IP pública *mypublicipinbound* que creó en el paso anterior.

```azurepowershell-interactive
New-AzLoadBalancer -Name lb -Sku Standard -ResourceGroupName myresourcegroupoutbound -Location eastus -FrontendIpConfiguration $frontendIPin,$frontendIPout -BackendAddressPool $bepoolin,$bepoolout -Probe $probe -LoadBalancingRule $inboundrule -OutboundRule $outboundrule 
```

En este momento, puede seguir agregando las máquinas virtuales a los grupos de direcciones IP de back-end *bepoolinbound* y *bepooloutbound* mediante la actualización de la configuración de IP de los recursos de la NIC correspondientes. Actualice la configuración del recurso mediante [Add-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede usar el comando [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.7.0) para quitar el grupo de recursos, el equilibrador de carga y los recursos relacionados.

```azurepowershell-interactive 
  Remove-AzResourceGroup -Name myresourcegroupoutbound
```

## <a name="next-steps"></a>Pasos siguientes
En este artículo creó un equilibrador de carga estándar, configuró las reglas de tráfico de entrada y salida del equilibrador de carga y configuró un sondeo de estado para las máquinas virtuales del grupo de servidores back-end. 

Para más información, continúe con los [tutoriales de Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).
