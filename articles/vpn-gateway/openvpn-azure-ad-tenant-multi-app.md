---
title: 'VPN Gateway: inquilino de Azure AD para distintos grupos de usuarios: Autenticación de Azure AD'
description: Puede usar la P2S VPN para conectarse a la red virtual con la autenticación de Azure AD
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: alzam
ms.openlocfilehash: 118ea21cbdd2e0527659c7c1beb40d8e42fa1d10
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "77485721"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>Creación de un inquilino de Azure Active Directory para conexiones del protocolo P2S OpenVPN

Al conectarse a la red virtual, puede usar la autenticación basada en certificados o la autenticación RADIUS. Sin embargo, cuando use el protocolo de VPN abierto, también puede usar la autenticación de Azure Active Directory. Si quiere que un conjunto de usuarios diferente pueda conectarse a diferentes puertas de enlace de VPN, puede registrar varias aplicaciones en AD y vincularlas a diferentes puertas de enlace de VPN. Este artículo le ayuda a configurar un inquilino de Azure AD para la autenticación de OpenVPN de P2S y a crear y registrar varias aplicaciones en Azure AD a fin de permitir un acceso distinto a los diferentes usuarios y grupos.

> [!NOTE]
> La autenticación de Azure AD solo se admite para las conexiones de protocolo de OpenVPN®.
>

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-enable-authentication-on-the-gateway"></a><a name="enable-authentication"></a>6. Habilitación de la autenticación en la puerta de enlace

En este paso, habilitará la autenticación de Azure AD en la puerta de enlace VPN.

1. Habilite la autenticación de Azure AD en la puerta de enlace VPN mediante la ejecución de los siguientes comandos. Asegúrese de modificar los comandos para que reflejen su propio entorno:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.microsoftonline.com/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.windows.net/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
    > [!NOTE]
    > No use el id. de la aplicación del cliente VPN de Azure en los comandos anteriores: Concederá acceso a todos los usuarios a la puerta de enlace VPN. Use el identificador de las aplicaciones que registró.

2. Ejecute los siguientes comandos para crear y descargar el perfil. Cambie los valores de-ResourcGroupName y-Name para que coincidan con los suyos.

    ```azurepowershell-interactive
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

3. Después de ejecutar los comandos, verá un resultado similar al siguiente. Copie la dirección URL del resultado en el explorador para descargar el archivo zip del perfil.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/profile.png)

4. Extraiga el archivo zip descargado.

5. Busque la carpeta "AzureVPN" descomprimida.

6. Anote la ubicación del archivo "azurevpnconfig. xml". Azurevpnconfig. xml contiene la configuración de la conexión VPN y se puede importar directamente en la aplicación del cliente VPN Azure. También puede distribuir este archivo a todos los usuarios que necesiten conectarse a través del correo electrónico u otros medios. El usuario necesitará credenciales de Azure AD válidas para conectarse correctamente.

## <a name="next-steps"></a>Pasos siguientes

Para conectarse a la red virtual, debe crear y configurar un perfil de cliente de VPN. Consulte [Configurar un cliente VPN para conexiones P2S VPN](openvpn-azure-ad-client.md).
