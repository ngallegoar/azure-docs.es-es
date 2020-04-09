---
title: Intervalos de direcciones IP privadas de SNAT de Azure Firewall
description: Puede configurar intervalos de direcciones IP privadas para que el firewall no aplique la traducción de direcciones de red de origen (SNAT) al tráfico que se dirige a esas direcciones IP.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 03/20/2020
ms.author: victorh
ms.openlocfilehash: ed8cef00b7de67458c607373c724a3717f14a7cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064805"
---
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Intervalos de direcciones IP privadas de SNAT de Azure Firewall

Azure Firewall no aplica SNAT con reglas de red cuando la dirección IP de destino es un intervalo de direcciones IP privadas, de acuerdo con la normativa [RFC 1918 de IANA](https://tools.ietf.org/html/rfc1918). Las reglas de aplicación se aplican siempre mediante un [proxy transparente](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy), independientemente de la dirección IP de destino.

Si su organización usa un intervalo de direcciones IP públicas para las redes privadas, Azure Firewall aplicará SNAT al tráfico para una de las direcciones IP privadas de firewall en AzureFirewallSubnet. Sin embargo, puede configurar Azure Firewall de modo que **no** aplique SNAT al intervalo de direcciones IP públicas.

## <a name="configure-snat-private-ip-address-ranges"></a>Configuración de intervalos de direcciones IP privadas de SNAT

Puede usar Azure PowerShell para especificar un intervalo de direcciones IP al que el firewall no aplicará SNAT.

### <a name="new-firewall"></a>Nuevo firewall

En el caso de un nuevo firewall, el comando de Azure PowerShell es:

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> IANAPrivateRanges se expande a los valores predeterminados actuales en Azure Firewall mientras que los demás intervalos se agregan a él.

Para más información, consulte [New-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0).

### <a name="existing-firewall"></a>Firewall existente

Para configurar un firewall existente, use los siguientes comandos de Azure PowerShell:

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>Plantillas

Puede agregar lo siguiente a la sección `additionalProperties`:

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).