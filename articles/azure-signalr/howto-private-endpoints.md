---
title: Usar puntos de conexión privados
titleSuffix: Azure SignalR Service
description: Información general de los puntos de conexión privados para el acceso seguro a Azure SignalR Service desde redes virtuales.
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: article
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 80369883b84ca30cae475235d41addcfba7e52e1
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152340"
---
# <a name="use-private-endpoints-for-azure-signalr-service"></a>Uso de puntos de conexión privados para Azure SignalR Service

Puede usar [puntos de conexión privados](../private-link/private-endpoint-overview.md) en Azure SignalR Service para que los clientes de una red virtual puedan acceder de forma segura a los datos a través de una instancia de [Private Link](../private-link/private-link-overview.md). El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual para Azure SignalR Service. El tráfico de red entre los clientes de la red virtual y Azure SignalR Service atraviesa un vínculo privado de la red troncal de Microsoft, lo que elimina la exposición a la red pública de Internet.

El uso de puntos de conexión privados para Azure SignalR Service le permite:

- Proteger Azure SignalR Service mediante el control de acceso a la red para bloquear todas las conexiones en el punto de conexión público de Azure SignalR Service.
- Aumentar la seguridad de la red virtual (VNet), ya que permite bloquear la filtración de datos de la red virtual.
- Conectarse de forma segura a Azure SignalR Service desde las redes locales que se conectan a la red virtual mediante [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) o instancias de [ExpressRoute](../expressroute/expressroute-locations.md) con emparejamiento privado.

## <a name="conceptual-overview"></a>Información general conceptual

![Información general de los puntos de conexión privados para Azure SignalR Service](media/howto-private-endpoints/private-endpoint-overview.png)

Un punto de conexión privado es una interfaz de red especial para un servicio de Azure de la [red virtual](../virtual-network/virtual-networks-overview.md). Cuando se crea un punto de conexión privado para Azure SignalR Service, este proporciona conectividad segura entre los clientes de la red virtual y el servicio. Al punto de conexión privado se le asigna una dirección IP del intervalo de direcciones IP de la red virtual. La conexión entre el punto de conexión privado y Azure SignalR Service usa un vínculo privado seguro.

Las aplicaciones de la red virtual se pueden conectar a Azure SignalR Service a través del punto de conexión privado sin problemas, **ya que se usan las mismas cadenas de conexión y mecanismos de autorización que en cualquier otro caso**. Los puntos de conexión privados se pueden usar con todos los protocolos que admita Azure SignalR Service, incluida la API REST.

Cuando se crea un punto de conexión privado para una instancia de Azure SignalR Service, se envía una solicitud de consentimiento para su aprobación al propietario de Azure SignalR Service. Si el usuario que solicita la creación del punto de conexión privado también es propietario de la instancia de Azure SignalR Service, esta solicitud de consentimiento se aprueba automáticamente.

Los propietarios de Azure SignalR Service pueden administrar las solicitudes de consentimiento y los puntos de conexión privados desde la pestaña *Puntos de conexión privados* de Azure SignalR Service en [Azure Portal](https://portal.azure.com).

> [!TIP]
> Si desea restringir el acceso a Azure SignalR Service de modo que solo se pueda realizar mediante el punto de conexión privado, [configure el control de acceso de red](howto-network-access-control.md#managing-network-access-control) para denegar o controlar todo acceso que se realice mediante el punto de conexión público.

### <a name="connecting-to-private-endpoints"></a>Conexión a puntos de conexión privados

Los clientes de una red virtual que usen el punto de conexión privado deben utilizar la misma cadena de conexión para Azure SignalR Service que aquellos clientes que se conectan mediante el punto de conexión público. Confiamos en la resolución del DNS para enrutar automáticamente las conexiones desde la red virtual a Azure SignalR Service a través de un vínculo privado.

> [!IMPORTANT]
> Use la misma cadena de conexión para conectarse a Azure SignalR Service mediante puntos de conexión privados, tal como haría en otras ocasiones. No se conecte a Azure SignalR Service mediante su dirección URL de subdominio `privatelink`.

De forma predeterminada, se crea una [zona DNS privada](../dns/private-dns-overview.md) conectada a la red virtual con las actualizaciones necesarias para los puntos de conexión privados. Sin embargo, si usa su propio servidor DNS, puede que tenga que realizar cambios adicionales en la configuración de DNS. En la sección [Cambios de DNS](#dns-changes-for-private-endpoints) que aparece a continuación se describen las actualizaciones necesarias para los puntos de conexión privados.

## <a name="dns-changes-for-private-endpoints"></a>Cambios de DNS en puntos de conexión privados

Al crear un punto de conexión privado, el registro del recurso CNAME de DNS de Azure SignalR Service se actualiza a un alias de un subdominio con el prefijo `privatelink`. De forma predeterminada, también se crea una [zona DNS privada](../dns/private-dns-overview.md), que se corresponde con el subdominio `privatelink`, con los registros de recursos D de DNS para los puntos de conexión privados.

Al resolver el nombre de dominio de Azure SignalR Service desde fuera de la red virtual con el punto de conexión privado, se resuelve en el punto de conexión público de Azure SignalR Service. Cuando se resuelve desde la red virtual que hospeda el punto de conexión privado, el nombre de dominio se resuelve en la dirección IP de este.

En el ejemplo anterior, los registros de recursos DNS de Azure SignalR Service "foobar", cuando se resuelven desde fuera de la red virtual que hospeda el punto de conexión privado, serán:

| Nombre                                                  | Tipo  | Value                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``foobar.service.signalr.net``                        | CNAME | ``foobar.privatelink.service.signalr.net``            |
| ``foobar.privatelink.service.signalr.net``            | A     | \<Azure SignalR Service public IP address\>           |

Como ya se ha mencionado, puede denegar o controlar el acceso de los clientes de fuera de la red virtual con el punto de conexión público mediante el control de acceso de red.

Los registros de recursos DNS "foobar", cuando los resuelva un cliente en la red virtual que hospeda el punto de conexión privado, serán:

| Nombre                                                  | Tipo  | Value                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``foobar.service.signalr.net``                        | CNAME | ``foobar.privatelink.service.signalr.net``            |
| ``foobar.privatelink.service.signalr.net``            | A     | 10.1.1.5                                              |

Este enfoque permite el acceso a Azure SignalR Service **mediante la misma cadena de conexión** para los clientes de la red virtual que hospeda los puntos de conexión privados y los clientes que están fuera de esta.

Si va a usar un servidor DNS personalizado en la red, los clientes deben ser capaces de resolver el FQDN del punto de conexión de Azure SignalR Service en la dirección IP del punto de conexión privado. Debe configurar el servidor DNS para delegar el subdominio del vínculo privado en la zona DNS privada de la red virtual, o bien configurar los registros D para `foobar.privatelink.service.signalr.net` con la dirección IP del punto de conexión privado.

> [!TIP]
> Cuando use un servidor DNS personalizado o local, debe configurarlo para resolver el nombre de Azure SignalR Service en el subdominio `privatelink` en la dirección IP del punto de conexión privado. Para ello, puede delegar el subdominio `privatelink` en la zona DNS privada de la red virtual, o bien configurar la zona DNS en el servidor DNS y agregar los registros A de DNS.

El nombre de zona DNS recomendado para los puntos de conexión privados de Azure SignalR Service es `privatelink.service.signalr.net`.

Para más información sobre cómo configurar su propio servidor DNS para que admita puntos de conexión privados, consulte los siguientes artículos:

- [Resolución de nombres de recursos en redes virtuales de Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)
- [Configuración de DNS para puntos de conexión privados](../private-link/private-endpoint-overview.md#dns-configuration)

## <a name="create-a-private-endpoint"></a>Creación de un punto de conexión privado

### <a name="create-a-private-endpoint-along-with-a-new-azure-signalr-service-in-the-azure-portal"></a>Creación de un punto de conexión privado junto con una nueva instancia de Azure SignalR Service en Azure Portal

1. Al crear una instancia de Azure SignalR Service, seleccione la pestaña **Redes**. Elija **Punto de conexión privado** como método de conectividad.

    ![Creación de Azure SignalR Service: pestaña Redes](media/howto-private-endpoints/portal-create-blade-networking-tab.png)

1. Haga clic en **Agregar**. Rellene la suscripción, el grupo de recursos, la ubicación y el nombre del nuevo punto de conexión privado. Elija una red virtual y una subred.

    ![Creación de una instancia de Azure SignalR Service: Adición de un punto de conexión privado](media/howto-private-endpoints/portal-create-blade-add-private-endpoint.png)

1. Haga clic en **Revisar + crear**.

### <a name="create-a-private-endpoint-for-an-existing-azure-signalr-service-in-the-azure-portal"></a>Creación de un punto de conexión privado para una instancia de Azure SignalR Service existente en Azure Portal

1. Vaya a la instancia de Azure SignalR Service.

1. Haga clic en el menú de configuración denominado **Conexiones de punto de conexión privado**.

1. Haga clic en el botón **+ Punto de conexión privado** en la parte superior.

    ![Hoja Conexiones de punto de conexión privado](media/howto-private-endpoints/portal-private-endpoint-connections-blade.png)

1. Rellene la suscripción, el grupo de recursos, el nombre del recurso y la región para el nuevo punto de conexión privado.
    
    ![Creación de un punto de conexión privado: Información básica](media/howto-private-endpoints/portal-create-private-endpoint-basics.png)

1. Elija el recurso de Azure SignalR Service de destino.

    ![Creación de un punto de conexión privado: Recurso](media/howto-private-endpoints/portal-create-private-endpoint-resource.png)

1. Elección de la red virtual de destino

    ![Creación de un punto de conexión privado: Configuración](media/howto-private-endpoints/portal-create-private-endpoint-configuration.png)

1. Haga clic en **Revisar + crear**.

### <a name="create-a-private-endpoint-using-azure-cli"></a>Creación de un punto de conexión privado mediante la CLI de Azure

1. Inicio de sesión en la CLI de Azure
    ```console
    az login
    ```
1. Selección de una suscripción a Azure
    ```console
    az account set --subscription {AZURE SUBSCRIPTION ID}
    ```
1. Creación de un grupo de recursos
    ```console
    az group create -n {RG} -l {AZURE REGION}
    ```
1. Registro de Microsoft.SignalRService como proveedor
    ```console
    az provider register -n Microsoft.SignalRService
    ```
1. Creación de una instancia de Azure SignalR Service
    ```console
    az signalr create --name {NAME} --resource-group {RG} --location {AZURE REGION} --sku Standard_S1
    ```
1. Creación de una red virtual
    ```console
    az network vnet create --resource-group {RG} --name {vNet NAME} --location {AZURE REGION}
    ```
1. Incorporación de una subred
    ```console
    az network vnet subnet create --resource-group {RG} --vnet-name {vNet NAME} --name {subnet NAME} --address-prefixes {addressPrefix}
    ```
1. Deshabilitación de directivas de red virtual
    ```console
    az network vnet subnet update --name {subnet NAME} --resource-group {RG} --vnet-name {vNet NAME} --disable-private-endpoint-network-policies true
    ```
1. Incorporación de una zona de DNS privado
    ```console
    az network private-dns zone create --resource-group {RG} --name privatelink.service.signalr.net
    ```
1. Vinculación de una zona de DNS privado a una red virtual
    ```console
    az network private-dns link vnet create --resource-group {RG} --virtual-network {vNet NAME} --zone-name privatelink.service.signalr.net --name {dnsZoneLinkName} --registration-enabled true
    ```
1. Creación de un punto de conexión privado (aprobación automática)
    ```console
    az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.SignalRService/SignalR/{NAME}" --group-ids signalr --connection-name {Private Link Connection Name} --location {AZURE REGION}
    ```
1. Crear un punto de conexión privado (solicitud manual de aprobación)
    ```console
    az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.SignalRService/SignalR/{NAME}" --group-ids signalr --connection-name {Private Link Connection Name} --location {AZURE REGION} --manual-request
    ```
1. Mostrar el estado de la conexión
    ```console
    az network private-endpoint show --resource-group {RG} --name {Private Endpoint Name}
    ```

## <a name="pricing"></a>Precios

Para más información sobre los precios, consulte [Precios de Azure Private Link](https://azure.microsoft.com/pricing/details/private-link).

## <a name="known-issues"></a>Problemas conocidos

Tenga en cuenta los siguientes problemas conocidos relacionados con los puntos de conexión privados para Azure SignalR Service

### <a name="free-tier"></a>Nivel Gratis

No se puede crear ningún punto de conexión privado para Azure SignalR Service en el nivel gratuito.

### <a name="access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Restricciones de acceso para los clientes de redes virtuales con puntos de conexión privados

Los clientes de redes virtuales con puntos de conexión privados se enfrentan a ciertas restricciones al acceder a otras instancias de Azure SignalR Service que tienen puntos de conexión privados. Por ejemplo, supongamos que una red virtual N1 tiene un punto de conexión privado para una instancia S1 de Azure SignalR Service. Si Azure SignalR Service S2 tiene un punto de conexión privado en una red virtual N2, los clientes de la red virtual N1 también deben tener acceso a Azure SignalR Service S2 con un punto de conexión privado. Si Azure SignalR Service S2 no tiene ningún punto de conexión privado, los clientes de la red virtual N1 pueden acceder a Azure SignalR Service en esa cuenta sin un punto de conexión privado.

Esta restricción es el resultado de los cambios de DNS realizados cuando Azure SignalR Service S2 crea un punto de conexión privado.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Reglas de grupo de seguridad de red para subredes con puntos de conexión privados

Actualmente, no se pueden configurar reglas de [grupo de seguridad de red](../virtual-network/network-security-groups-overview.md) (NSG) ni rutas definidas por el usuario para puntos de conexión privados. Las reglas de NSG que se aplican a la subred que hospeda el punto de conexión privado se aplican al punto de conexión privado. Una solución alternativa limitada para este problema es implementar las reglas de acceso para los puntos de conexión privados en las subredes de origen, aunque este enfoque puede requerir mayor sobrecarga de administración.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración del control de acceso a la red](howto-network-access-control.md)