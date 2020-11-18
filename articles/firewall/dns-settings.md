---
title: Configuración DNS de Azure Firewall
description: Puede configurar Azure Firewall con la configuración de servidor DNS y proxy DNS.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 11/06/2020
ms.author: victorh
ms.openlocfilehash: ad0ac040b510783656617ddbf2063cd94c80aae7
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94380953"
---
# <a name="azure-firewall-dns-settings"></a>Configuración DNS de Azure Firewall

Puede configurar un servidor DNS personalizado y habilitar el proxy DNS para Azure Firewall. Puede configurar estas opciones al implementar el firewall o una versión posterior desde la página **Configuración DNS**.

## <a name="dns-servers"></a>Servidores DNS

Un servidor DNS mantiene y resuelve los nombres de dominio en direcciones IP. De forma predeterminada, Azure Firewall usa Azure DNS para la resolución de nombres. La configuración de **servidor DNS** le permite configurar sus propios servidores DNS para la resolución de nombres de Azure Firewall. Puede configurar un único o varios servidores.

> [!NOTE]
> En el caso de instancias de Azure Firewall administradas mediante Azure Firewall Manager, la configuración de DNS se configura en la directiva de Azure Firewall asociada.

### <a name="configure-custom-dns-servers---azure-portal"></a>Configuración de servidores DNS personalizados: Azure Portal

1. En **Configuración** de Azure Firewall, seleccione **Configuración DNS**.
2. En **Servidores DNS**, puede escribir o agregar servidores DNS existentes que se hayan especificado previamente en la red virtual.
3. Seleccione **Guardar**.
4. El firewall dirige ahora el tráfico DNS a los servidores DNS especificados para la resolución de nombres.

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="Servidores DNS":::

### <a name="configure-custom-dns-servers---azure-cli"></a>Configuración de servidores DNS personalizados: CLI de Azure

En el ejemplo siguiente se actualiza Azure Firewall con servidores DNS personalizados mediante la CLI de Azure.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --dns-servers 10.1.0.4 10.1.0.5
```

> [!IMPORTANT]
> El comando `az network firewall` requiere que la extensión `azure-firewall` de la CLI de Azure esté instalada. Se puede instalar mediante el comando `az extension add --name azure-firewall`. 

### <a name="configure-custom-dns-servers---azure-powershell"></a>Configuración de servidores DNS personalizados: Azure PowerShell

En el ejemplo siguiente se actualiza Azure Firewall con servidores DNS personalizados mediante Azure PowerShell.

```azurepowershell
$dnsServers = @("10.1.0.4", "10.1.0.5")
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSServer = $dnsServers

$azFw | Set-AzFirewall
```

## <a name="dns-proxy"></a>Proxy DNS

Puede configurar Azure Firewall para que actúe como proxy DNS. Un proxy DNS actúa como intermediario para las solicitudes DNS de las máquinas virtuales cliente a un servidor DNS. Si configura un servidor DNS personalizado, debe habilitar el proxy DNS para evitar la falta de coincidencia de la resolución DNS y habilitar el filtrado de FQDN en las reglas de red.

Si no habilita el proxy DNS, las solicitudes DNS del cliente pueden viajar a un servidor DNS en un momento diferente o devolver una respuesta diferente en comparación con la del firewall. El proxy DNS coloca Azure Firewall en la ruta de acceso de las solicitudes de cliente para evitar incoherencias.

Hay dos tipos de función de almacenamiento en caché que se producen cuando Azure Firewall es un proxy DNS:

- Caché positiva: la resolución de DNS se realiza correctamente. El firewall usa el TTL (período de vida) del paquete o el objeto. 

- Caché negativa: la resolución de DNS no obtiene respuesta o resolución. El firewall almacena en caché esta información durante una hora.

El proxy DNS almacena todas las direcciones IP resueltas de FQDN en reglas de red. Como procedimiento recomendado, use FQDN que se resuelvan en una dirección IP.  

### <a name="dns-proxy-configuration"></a>Configuración de proxy DNS

La configuración del proxy DNS requiere tres pasos:
1. Habilite el proxy DNS en la configuración DNS de Azure Firewall.
2. Opcionalmente, configure el servidor DNS personalizado o use el valor predeterminado proporcionado.
3. Por último, debe configurar la dirección IP privada de Azure Firewall como una dirección DNS personalizada en la configuración del servidor DNS de la red virtual. Esto garantiza que el tráfico DNS se dirija a Azure Firewall.

#### <a name="configure-dns-proxy---azure-portal"></a>Configuración del proxy DNS: Azure Portal

Para configurar el proxy DNS, debe configurar los servidores DNS de la red virtual para que usen la dirección IP privada del firewall. A continuación, habilite el proxy DNS en la **configuración DNS** de Azure Firewall.

##### <a name="configure-virtual-network-dns-servers"></a>Configuración de los servidores DNS de la red virtual 

1. Seleccione la red virtual a la que se enrutará el tráfico DNS mediante Azure Firewall.
2. En **Configuración**, seleccione **Servidores DNS**.
3. Seleccione **Personalizado**, en **Servidores DNS**.
4. Escriba la dirección IP privada del firewall.
5. Seleccione **Guardar**.
6. Reinicie las máquinas virtuales conectadas a la red virtual para que se les asigne la nueva configuración de servidor DNS. Hasta que se reinicien, las máquinas virtuales seguirán usando la configuración de DNS que consideran actual.

##### <a name="enable-dns-proxy"></a>Habilitación del proxy DNS

1. Seleccione la instancia de Azure Firewall.
2. En **Configuración**, seleccione **Configuración DNS**.
3. De manera predeterminada, el **proxy DNS** está deshabilitado. Cuando está habilitado, el firewall escucha en el puerto 53 y reenvía las solicitudes DNS a los servidores DNS configurados.
4. Revise la configuración de los **servidores DNS** para asegurarse de que la configuración sea adecuada para su entorno.
5. Seleccione **Guardar**.

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="Proxy DNS":::

#### <a name="configure-dns-proxy---azure-cli"></a>Configuración del proxy DNS: CLI de Azure

Puede usar la CLI de Azure para definir la configuración del proxy DNS en Azure Firewall y actualizar las redes virtuales para usar Azure Firewall como servidor DNS.

##### <a name="configure-virtual-network-dns-servers"></a>Configuración de los servidores DNS de la red virtual

En este ejemplo se configura la red virtual para usar Azure Firewall como servidor DNS.
 
```azurecli-interactive
az network vnet update \
    --name VNetName \ 
    --resource-group VNetRG \
    --dns-servers <firewall-private-IP>
```

##### <a name="enable-dns-proxy"></a>Habilitación del proxy DNS

Este ejemplo habilita la característica de proxy DNS en Azure Firewall.

```azurecli-interactive
az network firewall update \
    --name fwName \ 
    --resource-group fwRG \
    --enable-dns-proxy true
```

#### <a name="configure-dns-proxy---azure-powershell"></a>Configuración del proxy DNS: Azure PowerShell

Puede usar Azure PowerShell para definir la configuración del proxy DNS en Azure Firewall y actualizar las redes virtuales para usar Azure Firewall como servidor DNS.

##### <a name="configure-virtual-network-dns-servers"></a>Configuración de los servidores DNS de la red virtual

 En este ejemplo se configura la red virtual para usar Azure Firewall como servidor DNS.

```azurepowershell
$dnsServers = @("<firewall-private-IP>")
$VNet = Get-AzVirtualNetwork -Name "VNetName" -ResourceGroupName "VNetRG"
$VNet.DhcpOptions.DnsServers = $dnsServers

$VNet | Set-AzVirtualNetwork
```

##### <a name="enable-dns-proxy"></a>Habilitación del proxy DNS

Este ejemplo habilita la característica de proxy DNS en Azure Firewall.

```azurepowershell
$azFw = Get-AzFirewall -Name "fwName" -ResourceGroupName "fwRG"
$azFw.DNSEnableProxy = $true

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Pasos siguientes

[Filtrado de FQDN en reglas de red](fqdn-filtering-network-rules.md)
