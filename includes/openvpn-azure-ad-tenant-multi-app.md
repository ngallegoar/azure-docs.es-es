---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/18/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e950d194ab48cec1a70c7bd17617332cb858a55d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "77485677"
---
## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Cree un inquilino Azure AD

Cree un inquilino de Azure AD con los pasos del artículo [Crear un nuevo inquilino](../articles/active-directory/fundamentals/active-directory-access-create-new-tenant.md):

* Nombre organizativo
* Nombre de dominio inicial

  Ejemplo:

   ![Nuevo inquilino Azure AD](./media/openvpn-azure-ad-tenant-multi-app/new-tenant.png)

## <a name="2-create-tenant-users"></a><a name="users"></a>2. Creación de usuarios inquilinos

En este paso, creará dos usuarios inquilinos de Azure AD: una cuenta de administrador global y una cuenta de usuario principal. La cuenta de usuario principal se usa como cuenta de inserción maestra (cuenta de servicio). Al crear una cuenta de usuario de inquilino Azure AD, ajuste el rol de directorio para el tipo de usuario que desea crear. Siga los pasos descritos en [este artículo](../articles/active-directory/fundamentals/add-users-azure-active-directory.md) para crear al menos dos usuarios para su inquilino de Azure AD. Asegúrese de cambiar el **rol de directorio** para crear los tipos de cuenta:

* Administrador global
* Usuario

## <a name="3-register-the-vpn-client"></a><a name="register-client"></a>3. Registro del cliente VPN

Registre el cliente VPN en el inquilino de Azure AD.

1. Busque el ID. de directorio del directorio que desea utilizar para la autenticación. Aparece en la sección propiedades de la página Active Directory.

    ![Identificador de directorio](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

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

    ![Identificador de directorio](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. Seleccione **Aceptar** cuando se le solicite.

    ![Accept](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. En Azure AD, en **Aplicaciones empresariales**, verá la **VPN de Azure** en la lista.

     ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/azure-vpn.png)

## <a name="4-register-additional-applications"></a><a name="register-apps"></a>4. Registro de aplicaciones adicionales

En este paso, debe registrar aplicaciones adicionales para varios usuarios y grupos.

1. En Azure Active Directory, haga clic en **Registros de aplicaciones** y en **+ Nuevo registro**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. En la página **Registrar una aplicación**, escriba el **nombre**. Seleccione los **tipos de cuenta compatibles** que quiera y, luego, haga clic en **Registrarse** .

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Una vez que se haya registrado la nueva aplicación, haga clic en **Exponer una API** en la hoja de la aplicación.

4. Haga clic en **+ Agregar un ámbito**.

5. Deje el valor **URI de id. de la aplicación** predeterminado. Haga clic en **Guardar y continuar**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app3.png)

6. Rellene los campos obligatorios y asegúrese de que la opción **Estado** está **habilitada**. Haga clic en **Agregar ámbito**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app4.png)

7. Haga clic en **Exponer una API** y, a continuación, **+ Agregar una aplicación cliente**.  En **Id. de cliente**, escriba los valores siguientes en función de la nube:

    - Escriba **41b23e61-6c1e-4545-B367-cd054e0ed4b4** para Azure **público**.
    - Escriba **51bb15d4-3a4f-4ebf-9dca-40096fe32426** para Azure **Government**.
    - Escriba **538ee9e6-310a-468d-afef-ea97365856a9** para Azure **Alemania**.
    - Escriba **49f817b6-84ae-4cc0-928c-73f27289b3aa** para Azure **China 21Vianet**.

8. Haga clic en **Agregar aplicación**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app5.png)

9. En la página **Introducción**, copie el **identificador de aplicación (cliente)** . Necesitará esta información para configurar las puertas de enlace de VPN.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Repita los pasos de esta sección ([Registro de aplicaciones adicionales](#register-apps)) para crear tantas aplicaciones como sea necesario para su requisito de seguridad. Cada aplicación se asociará a una puerta de enlace de VPN y puede tener un conjunto diferente de usuarios. Solo se puede asociar una aplicación a una puerta de enlace.

## <a name="5-assign-users-to-applications"></a><a name="assign-users"></a>5. Asignar usuarios a aplicaciones

Asigne los usuarios a las aplicaciones.

1. En **Azure AD -> Aplicaciones empresariales**, seleccione la aplicación recién registrada y haga clic en **Propiedades**. Asegúrese de que la opción **¿Asignación de usuarios?** esté establecida en **Sí**. Haga clic en **Save**(Guardar).

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. En la página de la aplicación, haga clic en **Usuarios y grupos** y, después, en **+Agregar usuario**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/user3.png)

3. En **Agregar asignación**, haga clic en **Usuarios y grupos**. Seleccione los usuarios que quiere que puedan tener acceso a esta aplicación de VPN. Haga clic en **Seleccionar**.

    ![VPN de Azure](./media/openvpn-azure-ad-tenant-multi-app/user4.png)