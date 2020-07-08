---
title: Configuración de un punto de conexión del equilibrador de carga interno (ILB)
titleSuffix: Azure Application Gateway
description: Este artículo proporciona información sobre cómo configurar Application Gateway con una dirección IP de front-end privada
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 04/16/2020
ms.author: victorh
ms.openlocfilehash: c7a0022c5cff405a993f30cdf2ab5900485c84a1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808127"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Configuración de una puerta de enlace de aplicaciones con un punto de conexión de equilibrador de carga interno (ILB)

Azure Application Gateway se puede configurar con una dirección VIP accesible desde Internet o con un punto de conexión interno que no está expuesto a Internet. Un punto de conexión interno usa una dirección IP privada para el front-end, que también se conoce como *punto de conexión de equilibrio de carga interno (ILB)* .

La configuración de la puerta de enlace con una dirección IP privada de front-end es útil para aplicaciones de línea de negocio internas que no están expuestas a Internet. También es útil para los distintos servicios y niveles de una aplicación de niveles múltiples que se encuentran dentro de un límite de seguridad no expuesto a Internet, pero que aún así siguen necesitando distribución de carga round robin, permanencia de sesión o terminación de Seguridad de la capa de transporte (TLS), conocida previamente como Capa de sockets seguros (SSL).

Este artículo le guía por los pasos necesarios para configurar una puerta de enlace de aplicaciones con una dirección IP privada de front-end mediante Azure Portal.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en <https://portal.azure.com>.

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

Para que Azure se comunique entre los recursos que se crean, se necesita una red virtual. Puede crear una red virtual o usar una existente. En este ejemplo, creará una red virtual. Puede crear una red virtual a la vez que crea la puerta de enlace de aplicaciones. Se crean instancias de Application Gateway en subredes independientes. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end.

1. Expanda el menú del portal y seleccione **Crear un recurso**.
2. Seleccione **Redes** y **Application Gateway** en la lista de destacados.
3. Escriba *myAppGateway* para el nombre de la puerta de enlace de aplicaciones y *myResourceGroupAG* para el nuevo grupo de recursos.
4. En **Región**, seleccione **(EE. UU.) Centro de EE. UU.** .
5. En **Nivel**, seleccione **Estándar**.
6. En **Configurar la red virtual**, seleccione **Crear nueva** y, después, especifique estos valores para ella:
   - *myVNet*: como nombre de la red virtual.
   - *10.0.0.0/16*: como espacio de direcciones de la red virtual.
   - *myAGSubnet*: como nombre de subred.
   - *10.0.0.0/24*: como espacio de direcciones de la subred.
   - *myBackendSubnet*: como nombre de subred de back-end.
   - *10.0.1.0/24*: como espacio de direcciones de la subred de back-end.

    ![Creación de una red virtual](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)

6. Seleccione **Aceptar** para crear la red virtual y la subred.
7. Seleccione **Siguiente:Front-end**.
8. En **Tipo de dirección IP de front-end**, seleccione **Privada**.

   De forma predeterminada, es una asignación de direcciones IP dinámicas. La primera dirección disponible de la subred configurada se asigna como dirección IP de front-end.
   > [!NOTE]
   > Una vez asignada, el tipo de dirección IP (dinámico o estático) no se puede cambiar más adelante.
9. Seleccione **Siguiente:Back-end**.
10. Seleccione **Agregar un grupo de back-end**.
11. En **Nombre**, escriba *appGatewayBackendPool*.
12. En **Agregar grupo de back-end sin destinos**, seleccione **Sí**. Agregará los destinos más adelante.
13. Seleccione **Agregar**.
14. Seleccione **Siguiente:Configuración**.
15. En **Reglas de enrutamiento**, seleccione **Agregar una regla**.
16. En **Nombre de la regla**, escriba *Rrule-01*.
17. En **Nombre del agente de escucha**, escriba *Listener-01*.
18. En **IP de front-end**, seleccione **Privada**.
19. Acepte el resto de valores predeterminados y seleccione la pestaña **Destinos de back-end**.
20. En **Tipo de destino**, seleccione **Grupo de back-end** y, luego, **appGatewayBackendPool**.
21. En **Configuración de HTTP**, seleccione **Crear nuevo**.
22. En **Nombre de la configuración HTTP**, escriba *http-setting-01*.
23. En **Protocolo de back-end**, seleccione **HTTP**.
24. En **Puerto de back-end**, escriba *80*.
25. Acepte el resto de valores predeterminados y seleccione **Agregar**.
26. En la página **Agregar una regla de enrutamiento**, seleccione **Agregar**.
27. Seleccione **Siguiente: Etiquetas**.
28. Seleccione **Siguiente: Review + create** (Revisar y crear).
29. Revise la configuración de la página de resumen y, luego, seleccione **Crear** para crear los recursos de red y la puerta de enlace de aplicaciones. La puerta de enlace de aplicaciones puede tardar varios minutos en crearse. Espere hasta que finalice la implementación correctamente antes de continuar con la siguiente sección.

## <a name="add-backend-pool"></a>Agregar grupo de back-end

El grupo de back-end se usa para enrutar las solicitudes a los servidores back-end, que atienden la solicitud. El servidor back-end puede estar compuesto por NIC, conjuntos de escalado de máquinas virtuales, direcciones IP públicas e internas, nombres de dominio completos (FQDN) y servidores back-end multiinquilino como Azure App Service. En este ejemplo, se usan máquinas virtuales como back-end de destino. Pueden usarse máquinas virtuales existentes o crear otras nuevas. En este ejemplo, se crean dos máquinas virtuales que Azure usa como servidores back-end para la puerta de enlace de aplicaciones.

Para ello, puede:

1. Cree dos máquinas virtuales, *myVM* y *myVM2*, que se usarán como servidores back-end.
2. Instalar IIS en las máquinas virtuales para comprobar que la puerta de enlace de aplicaciones se ha creado correctamente.
3. Agregar los servidores back-end al grupo de back-end.

### <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

1. Seleccione **Crear un recurso**.
2. Seleccione **Proceso** y, luego, **Máquina virtual**.
4. Especifique estos valores para la máquina virtual:
   - En **Grupo de recursos**, seleccione *myResourceGroupAG*.
   - En **Nombre de máquina virtual**, escriba *myVM*.
   - En **Imagen**, seleccione **Windows Server 2019 Datacenter**.
   - Escriba un **Nombre de usuario** válido.
   - Escriba una **Contraseña** válida.
5. Acepte el resto de valores predeterminados y seleccione **Siguiente: Discos**.
6. Acepte los valores predeterminados y seleccione **Siguiente: Redes**.
7. Asegúrese de que **myVNet** está seleccionada como red virtual y que la subred es **myBackendSubnet**.
8. Acepte el resto de valores predeterminados y seleccione **Siguiente: Administración**.
9. Seleccione **Desactivar** para deshabilitar los diagnósticos de arranque.
10. Acepte el resto de valores predeterminados y seleccione **Siguiente: Avanzado**.
11. Seleccione **Siguiente: Etiquetas**.
12. Seleccione **Siguiente: Review + create** (Revisar y crear).
13. Revise la configuración en la página de resumen y seleccione **Crear**. La máquina virtual puede tardar varios minutos en crearse. Espere hasta que finalice la implementación correctamente antes de continuar con la siguiente sección.

### <a name="install-iis"></a>Instalación de IIS

1. Abra Cloud Shell y asegúrese de que está establecido en **PowerShell**.
    ![private-frontendip-3](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Ejecute el siguiente comando para instalar IIS en la máquina virtual:

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `

     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `

     -Location CentralUS `

   ```



3. Cree una segunda máquina virtual e instale IIS según los pasos que acaba de finalizar. Escriba myVM2 como su nombre y como valor de VMName en Set-AzVMExtension.

### <a name="add-backend-servers-to-backend-pool"></a>Incorporación de servidores back-end a un grupo de back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.
2. Seleccione **Grupos de back-end**. Seleccione **appGatewayBackendPool**.
3. En **Tipo de destino**, seleccione **Máquina virtual** y, en **Destino**, seleccione la vNIC asociada a myVM.
4. Repita el procedimiento para agregar MyVM2.
   ![private-frontendip-4](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
5. Seleccione **Guardar**.

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

1. Compruebe la dirección IP de front-end que se ha asignado haciendo clic en la página **Configuraciones de IP de front-end** del portal.
    ![private-frontendip-5](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Copie la dirección IP privada y, luego, péguela en la barra de direcciones del explorador en una máquina virtual de la misma red virtual o del entorno local que tenga conectividad a esta red virtual. Luego, intente acceder a Application Gateway.

## <a name="next-steps"></a>Pasos siguientes

Si quiere supervisar el estado del back-end, consulte [Mantenimiento del back-end y registros de diagnóstico para Application Gateway](application-gateway-diagnostics.md).
