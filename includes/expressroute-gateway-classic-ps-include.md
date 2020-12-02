---
title: archivo de inclusión
description: archivo de inclusión
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 12/13/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c0c05eb88df27ba9e71c0f61bad75457d893a7ce
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95554129"
---
> [!NOTE]
> Estos ejemplos no se aplican a configuraciones coexistentes de S2S y ExpressRoute.
> Para obtener más información sobre cómo trabajar con puertas de enlace en una configuración de coexistencia, vea [Configuración de conexiones coexistentes.](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Adición de una puerta de enlace

Al agregar una puerta de enlace a una red virtual mediante el modelo de recursos clásico, se modifica el archivo de configuración de red directamente antes de crear la puerta de enlace. Los valores de los ejemplos siguientes deben estar presentes en el archivo para crear una puerta de enlace. Si la red virtual tenía anteriormente una puerta de enlace asociada, algunos de estos valores ya estarán presentes. Modifique el archivo para que refleje los valores siguientes.

### <a name="download-the-network-configuration-file"></a>Descarga del archivo de configuración de red

1. Siga los pasos descritos en el artículo [Archivo de configuración de red](/previous-versions/azure/virtual-network/virtual-networks-using-network-configuration-file) para descargar el archivo de configuración de red. Abra el archivo con un editor de texto.
2. Agregue un sitio de red local al archivo. Puede usar cualquier prefijo de dirección válido. Puede agregar cualquier dirección IP válida para la puerta de enlace VPN. Los valores de dirección de esta sección no se usan para las operaciones de ExpressRoute, pero son necesarios para la validación del archivo. En el ejemplo, "branch1" es el nombre del sitio. Puede usar otro nombre, pero asegúrese de usar el mismo valor en la sección de puerta de enlace del archivo.

   ```
   <VirtualNetworkConfiguration>
    <Dns />
    <LocalNetworkSites>
      <LocalNetworkSite name="branch1">
        <AddressSpace>
          <AddressPrefix>165.3.1.0/27</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>3.2.1.4</VPNGatewayAddress>
    </LocalNetworkSite>
   ```
3. Vaya hasta VirtualNetworkSites y modifique los campos.

   * Compruebe que la subred de puerta de enlace exista para la red virtual. Si no es así, ahora puede agregar una. El nombre debe ser "GatewaySubnet".
   * Compruebe que exista la sección de puerta de enlace del archivo. Si no aparece, agréguela. Esto es necesario para asociar la red virtual con el sitio de red local (que representa la red a la que se conecta).
   * Compruebe que el tipo de conexión sea igual a Dedicado. Esto es necesario para las conexiones de ExpressRoute.

   ```
   </LocalNetworkSites>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myAzureVNET" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="default">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.1.0/27</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="branch1">
              <Connection type="Dedicated" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
   </VirtualNetworkConfiguration>
   </NetworkConfiguration>
   ```
4. Guarde el archivo y cárguelo en Azure.

### <a name="create-the-gateway"></a>Creación de la puerta de enlace

Utilice el siguiente comando para crear una puerta de enlace. Sustituya los valores por los suyos propios.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Comprobación de la creación de la puerta de enlace

Utilice el siguiente comando para comprobar si se ha creado la puerta de enlace. Este comando también recupera el identificador de la puerta de enlace, que necesita para realizar otras operaciones.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Cambio del tamaño de una puerta de enlace

Hay varias [SKU de puerta de enlace](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Puede utilizar el siguiente comando para cambiar en cualquier momento la SKU de puerta de enlace.

> [!IMPORTANT]
> Este comando no funciona para la puerta de enlace de UltraPerformance. Para cambiar la puerta de enlace a una puerta de enlace de UltraPerformance, quite primero la puerta de enlace de ExpressRoute existente y, después, cree una nueva puerta de enlace de UltraPerformance. Para degradar la puerta de enlace desde una puerta de enlace de UltraPerformance, quite primero la puerta de enlace de UltraPerformance existente y, después, cree una nueva puerta de enlace.
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Eliminación de una puerta de enlace

Utilice el siguiente comando para quitar una puerta de enlace.

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```