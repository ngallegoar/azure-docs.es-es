---
title: 'Inquilino de Azure AD para las conexiones VPN de usuario: Autenticación de Azure AD'
description: Puede usar la VPN de usuario de WAN virtual de Azure (punto a sitio) para conectarse a la red virtual mediante la autenticación de Azure AD
titleSuffix: Azure Virtual WAN
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: alzam
ms.openlocfilehash: 74347ce969b6a5ffd57f5ca8396517e78590f3f2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80059459"
---
# <a name="create-an-azure-active-directory-tenant-for-user-vpn-openvpn-protocol-connections"></a>Creación de un inquilino de Azure Active Directory para conexiones de VPN de usuario del protocolo OpenVPN

Al conectarse a la red virtual, puede usar la autenticación basada en certificados o la autenticación RADIUS. Sin embargo, cuando use el protocolo de VPN abierto, también puede usar la autenticación de Azure Active Directory. Este artículo le ayuda a configurar un inquilino de Azure AD para la autenticación de VPN abierta de VPN de usuario de WAN virtual (punto a sitio).

> [!NOTE]
> La autenticación de Azure AD solo se admite para las conexiones de protocolo de OpenVPN&reg;.
>

## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Cree un inquilino Azure AD

Cree un inquilino de Azure AD con los pasos del artículo [Crear un nuevo inquilino](../active-directory/fundamentals/active-directory-access-create-new-tenant.md):

* Nombre organizativo
* Nombre de dominio inicial

Ejemplo:

   ![Nuevo inquilino Azure AD](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Crear usuario de inquilino Azure AD

Después, cree dos cuentas de usuario. Cree una cuenta de Administrador global y una cuenta de usuario principal. La cuenta de usuario principal se usa como cuenta de inserción maestra (cuenta de servicio). Al crear una cuenta de usuario de inquilino Azure AD, ajuste el rol de directorio para el tipo de usuario que desea crear.

Siga los pasos descritos en [este artículo](../active-directory/fundamentals/add-users-azure-active-directory.md) para crear al menos dos usuarios para su inquilino de Azure AD. Asegúrese de cambiar el **rol de directorio** para crear los tipos de cuenta:

* Administrador global
* Usuario

## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>3. Habilitación de la autenticación de Azure AD

1. Busque el ID. de directorio del directorio que desea utilizar para la autenticación. Aparece en la sección propiedades de la página Active Directory.

    ![Identificador de directorio](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. Copie el ID. del directorio.

3. Inicie sesión en el Azure Portal como un usuario al que se le ha asignado el rol de **administrador global**.

4. Después, ceda el consentimiento del administrador. Copie y pegue la dirección URL que pertenece a la ubicación de implementación en la barra de direcciones del explorador:

    Público

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Government

    ```
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Germany

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure China 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. Seleccione la cuenta del **Administrador global** si se le solicita.

    ![Identificador de directorio](./media/openvpn-create-azure-ad-tenant/pick.png)

6. Seleccione **Aceptar** cuando se le solicite.

    ![Accept](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. En el Azure AD, en **Aplicaciones empresariales**, verá la **VPN de Azure** en la lista.

    ![VPN de Azure](./media/openvpn-create-azure-ad-tenant/azurevpn.png)

8. Configure la autenticación de Azure AD para la VPN de usuario y asígnela a un Centro virtual, para lo que debe seguir los pasos que se describen en el artículo acerca de la [configuración de la autenticación de Azure AD para la conexión de punto a sitio con Azure](virtual-wan-point-to-site-azure-ad.md)

## <a name="next-steps"></a>Pasos siguientes

Para conectarse a una red virtual, debe crear y configurar un perfil de cliente VPN y asociarlo a un Centro virtual. Consulte [Configuración de la autenticación de Azure AD para la conexión de punto a sitio con Azure](virtual-wan-point-to-site-azure-ad.md).
