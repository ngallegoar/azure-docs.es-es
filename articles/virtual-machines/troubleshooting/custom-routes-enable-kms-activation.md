---
title: Uso de rutas personalizadas de Azure para habilitar la activación de KMS con tunelización forzada | Microsoft Docs
description: Se muestra cómo usar rutas personalizadas de Azure para habilitar la activación de KMS al usar tunelización forzada en Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: 1c2050969e95b521554bba100b688add3a987a80
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86526748"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Error de activación de Windows en el escenario de tunelización forzada

En este artículo se describe cómo resolver el problema de activación de KMS que podría experimentar al habilitar la tunelización forzada en escenarios de conexión VPN de sitio a sitio o ExpressRoute.

## <a name="symptom"></a>Síntoma

La [tunelización forzada](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) se habilita en las subredes de red virtual de Azure para dirigir todo el tráfico vinculado a Internet de vuelta a la red local. En este escenario, las máquinas virtuales de Azure que ejecutan Windows no podrán activar este.

## <a name="cause"></a>Causa

Las máquinas virtuales Windows de Azure se deben conectar al servidor de KMS de Azure para la activación de Windows. La activación requiere que la solicitud de activación proceda de una dirección IP pública de Azure. En el escenario de tunelización forzada, se produce un error en la activación porque la solicitud de activación procede de la red local en lugar de una dirección IP pública de Azure.

## <a name="solution"></a>Solución

Para resolver este problema, use la ruta personalizada de Azure para dirigir el tráfico de activación al servidor de KMS de Azure.

La dirección IP del servidor de KMS para la nube global de Azure es 23.102.135.246. El nombre DNS es kms.core.windows.net. Si usa otras plataformas de Azure como Azure Alemania, tendrá que usar la dirección IP del servidor de KMS correspondiente. Para más información, vea la tabla siguiente:

|Plataforma| DNS DE KMS|DIRECCIÓN IP DE KMS|
|------|-------|-------|
|Azure Global|kms.core.windows.net|23.102.135.246|
|Azure Alemania|kms.core.cloudapi.de|51.4.143.248|
|Azure US Government|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Para agregar la ruta personalizada, siga estos pasos:

### <a name="for-resource-manager-vms"></a>Para máquinas virtuales de Resource Manager

 

> [!NOTE] 
> La activación utiliza direcciones IP públicas y se verá afectada por una configuración de Load Balancer SKU estándar. Revise cuidadosamente las [conexiones salientes en Azure](../../load-balancer/load-balancer-outbound-connections.md) para obtener información sobre los requisitos.

1. Abra Azure PowerShell y después [inicie sesión en la suscripción de Azure](/powershell/azure/authenticate-azureps).
2. Ejecute los comandos siguientes:

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable

    # Next, attach the route table to the subnet that hosts the VMs

    Set-AzVirtualNetworkSubnetConfig -Name "Subnet01" -VirtualNetwork $vnet -AddressPrefix "10.0.0.0/24" -RouteTable $RouteTable

    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```
3. Vaya a la máquina virtual que tiene problemas de activación. Use [PsPing](/sysinternals/downloads/psping) para probar si puede alcanzar el servidor de KMS:

    ```console
    psping kms.core.windows.net:1688
    ```

4. Intente activar Windows y compruebe si el problema se ha resuelto.

### <a name="for-classic-vms"></a>Para máquinas virtuales clásicas

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

1. Abra Azure PowerShell y después [inicie sesión en la suscripción de Azure](/powershell/azure/authenticate-azureps).
2. Ejecute los comandos siguientes:

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Vaya a la máquina virtual que tiene problemas de activación. Use [PsPing](/sysinternals/downloads/psping) para probar si puede alcanzar el servidor de KMS:

    ```console
    psping kms.core.windows.net:1688
    ```

4. Intente activar Windows y compruebe si el problema se ha resuelto.

## <a name="next-steps"></a>Pasos siguientes

- [Claves de configuración de cliente KMS](/windows-server/get-started/kmsclientkeys)
- [Review and Select Activation Methods](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134256(v=ws.11)) (Revisión y selección de métodos de activación)
