---
title: 'Tutorial: Protección de la red virtual de centro mediante Azure Firewall Manager'
description: En este tutorial aprenderá a proteger la red virtual con Azure Firewall Manager mediante Azure Portal.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 3d4d1e65c2200aee178abefb46d3e330acbd3108
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563637"
---
# <a name="tutorial-secure-your-hub-virtual-network-using-azure-firewall-manager"></a>Tutorial: Protección de la red virtual de centro mediante Azure Firewall Manager

Cuando conecta la red local a una red virtual de Azure para crear una red híbrida, la capacidad de controlar el acceso a los recursos de la red de Azure es parte importante de un plan de seguridad global.

Mediante Azure Firewall Manager puede crear una red virtual de centro para proteger el tráfico de la red híbrida destinado a direcciones IP privadas, PaaS de Azure e Internet. Puede usar Azure Firewall Manager para controlar el acceso de red en una red híbrida con directivas que definan el tráfico de red permitido y denegado.

Firewall Manager también admite una arquitectura de centro virtual protegido. Para ver una comparación entre los tipos de arquitectura de red virtual de centro y centro virtual protegido, consulte [¿Cuáles son las opciones de arquitectura de Azure Firewall Manager?](vhubs-and-vnets.md)

En este tutorial se crearán tres redes virtuales:

- **VNet-Hub**: el firewall está en esta red virtual.
- **VNet-Spoke**: la red virtual Spoke representa la carga de trabajo ubicada en Azure.
- **VNet-Onprem**: la red virtual local representa una red local. En una implementación real, se puede conectar mediante una conexión VPN o ExpressRoute. Para simplificar, este tutorial usa una conexión de puerta de enlace de VPN y una red virtual ubicada en Azure para representar una red local.

![Red híbrida](media/tutorial-hybrid-portal/hybrid-network-firewall.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una directiva de firewall
> * Crear las redes virtuales
> * Configuración e implementación del firewall
> * Creación y conexión de las puertas de enlace de VPN
> * Emparejar las redes virtuales de tipo hub-and-spoke
> * Creación de las rutas
> * Creación de las máquinas virtuales
> * Probar el firewall


## <a name="prerequisites"></a>Requisitos previos

Una red híbrida usa el modelo de arquitectura radial para enrutar el tráfico entre redes virtuales de Azure y redes locales. La arquitectura radial tiene los siguientes requisitos:

- Establezca **AllowGatewayTransit** al emparejar VNet-Hub con VNet-Spoke. En una arquitectura de red radial, el tránsito de una puerta de enlace permite que las redes virtuales de radio compartan la puerta de enlace de VPN en el centro, en lugar de implementar puertas de enlace de VPN en todas las redes virtuales de radio. 

   Además, las rutas a las redes virtuales conectadas a la puerta de enlace o a las redes locales se propagarán automáticamente a las tablas de enrutamiento de las redes virtuales emparejadas mediante el tránsito de la puerta de enlace. Para más información, consulte [Configuración del tránsito de la puerta de enlace de VPN para el emparejamiento de red virtual](../vpn-gateway/vpn-gateway-peering-gateway-transit.md).

- Establezca **UseRemoteGateways** al emparejar VNet-Spoke con VNet-Hub. Si se establece **UseRemoteGateways** y también se establece **AllowGatewayTransit** en emparejamiento remoto, la red virtual de radio usa puertas de enlace de la red virtual remota para el tránsito.
- Para enrutar el tráfico de la subred de radio a través del firewall de centro, necesita una ruta definida por el usuario (UDR) que apunte al firewall con la opción **Virtual network gateway route propagation** (Deshabilitar la propagación de rutas de la puerta de enlace de red virtual) deshabilitada. Esta opción evita la distribución de rutas a las subredes de radio. Esto evita que las rutas aprendidas entren en conflicto con la UDR.
- Configure una ruta definida por el usuario en la subred de la puerta de enlace del centro que apunte a la dirección IP del firewall como próximo salto para las redes de radio. No se requiere ninguna ruta definida por el usuario en la subred de Azure Firewall, ya que obtiene las rutas de BGP.

Consulte la sección [Creación de rutas](#create-the-routes) en este tutorial para ver cómo se crean estas rutas.

>[!NOTE]
>Azure Firewall debe tener conectividad directa a Internet. Si AzureFirewallSubnet aprende una ruta predeterminada a la red local mediante BGP, debe reemplazarla por una UDR 0.0.0.0/0 con el valor **NextHopType** establecido como **Internet** para mantener la conectividad directa a Internet.
>
>Azure Firewall puede configurarse para admitir la tunelización forzada. Para más información, consulte [Tunelización forzada de Azure Firewall](../firewall/forced-tunneling.md).

>[!NOTE]
>El tráfico entre redes virtuales emparejadas directamente se enruta directamente aunque una ruta definida por el usuario apunte a Azure Firewall como puerta de enlace predeterminada. Para enviar tráfico de subred a subred al firewall en este escenario, una UDR debe contener el prefijo de red de la subred de destino de forma explícita en ambas subredes.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-firewall-policy"></a>Creación de una directiva de firewall

1. Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).
2. En la barra de búsqueda de Azure Portal, escriba **Firewall Manager** y presione **Entrar**.
3. En la página de Azure Firewall Manager, seleccione **Ver directivas de Azure Firewall**.

   ![Directiva de firewall](media/tutorial-hybrid-portal/firewall-manager-policy.png)

1. Seleccione **Crear una directiva de Azure Firewall**.
1. Seleccione su suscripción y, como grupo de recursos, elija **Crear nuevo** y cree un grupo de recursos llamado **FW-Hybrid-Test**.
2. Como nombre de la directiva, escriba **Pol-Net01**.
3. Como región, seleccione **Este de EE. UU.** .
4. Seleccione **Siguiente: Reglas**.
5. Seleccione **Agregar una colección de reglas**.
6. En **Nombre**, escriba **RCNet01**.
7. En **Tipo de colección de reglas**, seleccione **Red**.
8. En **Prioridad**, escriba **100**.
9. En **Acción**, seleccione **Permitir**.
10. En **Reglas**, como **Nombre**, escriba **AllowWeb**.
11. En **Direcciones de origen**, escriba **192.168.1.0/24**.
12. En **Protocolo**, seleccione **TCP**.
13. En **Puertos de destino**, escriba **80**.
14. En **Tipo de destino**, seleccione **Dirección IP**.
15. En **Destino**, escriba **10.6.0.0/16**.
16. En la siguiente fila de la regla, escriba la siguiente información:
 
    En Nombre, escriba **AllowRDP**.<br>
    En Dirección IP de origen, escriba **192.168.1.0/24**.<br>
    En Protocolo, seleccione **TCP**.<br>
    En Puertos de destino, escriba **3389**.<br>
    En Tipo de destino, seleccione **Dirección IP**.<br>
    En Destino, escriba **10.6.0.0/16**.

1. Seleccione **Agregar**.
2. Seleccione **Revisar + crear**.
3. Revise los valores y, luego, seleccione **Crear**.

## <a name="create-the-firewall-hub-virtual-network"></a>Crear la red virtual del centro de firewall

> [!NOTE]
> El tamaño de la subred AzureFirewallSubnet es /26. Para más información sobre el tamaño de la subred, consulte [Preguntas más frecuentes sobre Azure Firewall](../firewall/firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En **Redes**, seleccione **Red virtuales**.
4. En **Nombre**, escriba **Vnet-Hub**.
5. En **Espacio de direcciones**, escriba **10.5.0.0/16**.
6. En **Suscripción**, seleccione la suscripción.
7. En **Grupo de recursos**, seleccione **FW-Hybrid-Test**.
8. En **Ubicación**, seleccione **Este de EE.UU.**
9. En **Subred**, como **Nombre** escriba **AzureFirewallSubnet**. El firewall estará en esta subred y el nombre de la subred **debe** ser AzureFirewallSubnet.
10. En **Intervalo de direcciones**, escriba **10.5.0.0/26**. 
11. Acepte los restantes valores predeterminados y seleccione **Crear**.

## <a name="create-the-spoke-virtual-network"></a>Crear la red virtual de tipo hub-and-spoke

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En **Redes**, seleccione **Red virtuales**.
4. En **Nombre**, escriba **VNet-Spoke**.
5. En **Espacio de direcciones**, escriba **10.6.0.0/16**.
6. En **Suscripción**, seleccione la suscripción.
7. En **Grupo de recursos**, seleccione **FW-Hybrid-Test**.
8. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
9. En **Subred**, en **Nombre** escriba **SN-Workload**.
10. En **Intervalo de direcciones**, escriba **10.6.0.0/24**.
11. Acepte los restantes valores predeterminados y seleccione **Crear**.

## <a name="create-the-on-premises-virtual-network"></a>Crear la red virtual local

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En **Redes**, seleccione **Red virtuales**.
4. En **Nombre**, escriba **VNet-OnPrem**.
5. En **Espacio de direcciones**, escriba **192.168.0.0/16**.
6. En **Suscripción**, seleccione la suscripción.
7. En **Grupo de recursos**, seleccione **FW-Hybrid-Test**.
8. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
9. En **Subred**, en **Nombre**, escriba **SN-Corp**.
10. En **Intervalo de direcciones**, escriba **192.168.1.0/24**.
11. Acepte los restantes valores predeterminados y seleccione **Crear**.

Una vez implementada la red virtual, cree una segunda subred para la puerta de enlace.

1. En la página **VNet-Onprem**, seleccione **Subredes**.
2. Seleccione **+Subred**.
3. En **Nombre**, escriba **GatewaySubnet.**
4. En **Intervalo de direcciones (bloque CIDR)** , escriba **192.168.2.0/24**.
5. Seleccione **Aceptar**.

### <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

Se trata de la dirección IP pública que se usa para la puerta de enlace local.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **dirección IP pública** y presione **Entrar**.
3. Seleccione **Dirección IP pública** y luego **Crear**.
4. En el nombre, escriba **VNet-Onprem-GW-pip**.
5. En el grupo de recursos, escriba **FW-Hybrid-Test**.
6. En **Ubicación**, seleccione **Este de EE. UU.** .
7. Acepte los otros valores predeterminados y seleccione **Crear**.

## <a name="configure-and-deploy-the-firewall"></a>Configuración e implementación del firewall

Cuando las directivas de seguridad están asociadas con un centro de conectividad, se hace referencia a ellas como *red virtual de centro de conectividad*.

Convierta la red virtual **VNet-Hub** en una *red virtual de centro* y protéjala con Azure Firewall.

1. En la barra de búsqueda de Azure Portal, escriba **Firewall Manager** y presione **Entrar**.
3. En la página de Azure Firewall Manager, en **Add security to virtual networks** (Agregar seguridad a redes virtuales), seleccione **View hub virtual networks** (Ver redes virtuales de centro).
4. Seleccione **Convert virtual networks** (Convertir redes virtuales).
5. Seleccione **VNet-hub** y, luego, elija **Siguiente: Azure Firewall**.
6. En **Directiva de firewall**, seleccione **Pol-Net01**.
7. Seleccione **Siguiente: Revisar y confirmar**.
8. Revise los detalles y, luego, seleccione **Confirmar**.


   Esto tarda unos minutos en implementarse.
7. Una vez finalizada la implementación, vaya al grupo de recursos **FW-Hybrid-Test** y seleccione el firewall.
9. Anote la dirección **Firewall private IP** (IP privada de firewall) de la página **Información general**. Se usará más adelante al crear la ruta predeterminada.

## <a name="create-and-connect-the-vpn-gateways"></a>Creación y conexión de las puertas de enlace de VPN

Las redes virtuales de centro y local están conectadas mediante puertas de enlace VPN.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Creación de una puerta de enlace VPN para la red virtual de centro

Cree la puerta de enlace VPN para la red virtual de centro. Las configuraciones de red a red requieren un VpnType RouteBased. La creación de una puerta de enlace de VPN suele tardar 45 minutos o más, según la SKU de la puerta de enlace de VPN seleccionada.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **puerta de enlace de red virtual** y presione **Entrar**.
3. Seleccione **Puerta de enlace de red virtual** y, después, **Crear**.
4. En **Nombre**, escriba **GW-hub**.
5. En **Región**, seleccione **(EE. UU.) Este de EE. UU.** .
6. En **Tipo de puerta de enlace** seleccione **VPN**.
7. En **Tipo de VPN** seleccione **Basada en rutas**.
8. En **SKU**, seleccione **Básico**.
9. En **Red virtual**, seleccione **VNet-hub**.
10. En **Dirección IP pública**, seleccione **Crear nuevo** y escriba **VNet-hub-GW-pip** como nombre.
11. Acepte el resto de valores predeterminados y seleccione **Revisar y crear**.
12. Revise la configuración y, después, seleccione **Crear**.

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Creación de una puerta de enlace VPN para la red virtual local

Cree la puerta de enlace VPN para la red virtual local. Las configuraciones de red a red requieren un VpnType RouteBased. La creación de una puerta de enlace de VPN suele tardar 45 minutos o más, según la SKU de la puerta de enlace de VPN seleccionada.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **puerta de enlace de red virtual** y presione **Entrar**.
3. Seleccione **Puerta de enlace de red virtual** y, después, **Crear**.
4. En **Nombre**, escriba **GW-Onprem**.
5. En **Región**, seleccione **(EE. UU.) Este de EE. UU.** .
6. En **Tipo de puerta de enlace** seleccione **VPN**.
7. En **Tipo de VPN** seleccione **Basada en rutas**.
8. En **SKU**, seleccione **Básico**.
9. En **Red virtual**, selecciona **VNet-Onprem**.
10. En **Dirección IP pública**, seleccione **Usar existente* y elija **VNet-Onprem-GW-pip** como nombre.
11. Acepte el resto de valores predeterminados y seleccione **Revisar y crear**.
12. Revise la configuración y, después, seleccione **Crear**.

### <a name="create-the-vpn-connections"></a>Creación de las conexiones

Ahora puede crear las conexiones de VPN entre las puertas de enlace de centro y local.

En este paso va a crear la conexión entre la red virtual de centro y la red virtual local. Verá una clave compartida a la que se hace referencia en los ejemplos. Puede utilizar sus propios valores para la clave compartida. Lo importante es que la clave compartida coincida en ambas conexiones. Se tardará unos momentos en terminar de crear la conexión.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la puerta de enlace **GW-hub**.
2. Seleccione **Conexiones** en la columna izquierda.
3. Seleccione **Agregar**.
4. En el nombre de la conexión, escriba **Hub-to-Onprem**.
5. En **Tipo de conexión**, seleccione **De VNet a VNet**.
6. En **Segunda puerta enlace de red virtual**, seleccione **GW-Onprem**.
7. En **Clave compartida (PSK)** , escriba **AzureA1b2C3**.
8. Seleccione **Aceptar**.

Cree la conexión de red entre la red virtual local y la red virtual de centro. Este paso es similar al anterior, excepto que se crea una conexión desde la red virtual local a la red virtual de centro. Asegúrese de que coincidan las claves compartidas. Después de unos minutos, se habrá establecido la conexión.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la puerta de enlace **GW-Onprem**.
2. Seleccione **Conexiones** en la columna izquierda.
3. Seleccione **Agregar**.
4. En el nombre de la conexión, escriba **Onprem-to-Hub**.
5. En **Tipo de conexión**, seleccione **De VNet a VNet**.
6. En **Segunda puerta enlace de red virtual**, seleccione **GW-hub**.
7. En **Clave compartida (PSK)** , escriba **AzureA1b2C3**.
8. Seleccione **Aceptar**.


#### <a name="verify-the-connection"></a>Comprobación de la conexión

Después de unos cinco minutos o más, el estado de ambas conexiones debe ser **conectado**.

![Conexiones de puerta de enlace](media/secure-hybrid-network/gateway-connections.png)

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Emparejar las redes virtuales de tipo hub-and-spoke

Ahora, empareje las redes virtuales de tipo hub-and-spoke.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la red virtual **VNet-hub**.
2. En la columna izquierda, seleccione **Emparejamientos**.
3. Seleccione **Agregar**.
4. En **Nombre**, escriba **HubtoSpoke.** .
5. En **Red virtual**, seleccione **VNet-spoke**.
6. En el nombre del emparejamiento de VNetSpoke con VNet-Hub, escriba **SpoketoHub**.
7. Seleccione **Permitir tránsito de puerta de enlace**.
8. Seleccione **Aceptar**.

### <a name="configure-additional-settings-for-the-spoketohub-peering"></a>Configuración de opciones adicionales para el emparejamiento de SpoketoHub

Deberá habilitar **Permitir tráfico reenviado** en el emparejamiento de SpoketoHub.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la red virtual **VNet-Spoke**.
2. En la columna izquierda, seleccione **Emparejamientos**.
3. Seleccione el emparejamiento de **SpoketoHub**.
4. En **Permitir el tráfico reenviado de VNet-hub a VNet-Spoke**, seleccione **Habilitado**.
5. Seleccione **Guardar**.

## <a name="create-the-routes"></a>Creación de las rutas

A continuación, cree un par de rutas:

- Una ruta desde la subred de puerta de enlace de concentrador a la subred de radio mediante la dirección IP del firewall.
- Una ruta predeterminada desde la subred de radio mediante la dirección IP del firewall.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **tabla de rutas** y presione **Entrar**.
3. Seleccione **Tabla de rutas**.
4. Seleccione **Crear**.
5. En el nombre, escriba **UDR-Hub-Spoke**.
6. Seleccione **FW-Hybrid-Test** en grupo de recursos.
8. En **Ubicación**, seleccione **(EE. UU.) Este de EE. UU.** .
9. Seleccione **Crear**.
10. Una vez creada la tabla de rutas, selecciónela para abrir la página de la tabla de rutas.
11. Seleccione **Rutas** en la columna izquierda.
12. Seleccione **Agregar**.
13. En el nombre de ruta, escriba **ToSpoke**.
14. En el prefijo de dirección, escriba **10.6.0.0/16**.
15. En el tipo del próximo salto, seleccione **Aplicación virtual**.
16. En la dirección del próximo salto, escriba la dirección IP privada del firewall que anotó anteriormente.
17. Seleccione **Aceptar**.

Ahora asocie la ruta a la subred.

1. En la página **UDR-Hub-Spoke - Routes**, seleccione **Subredes**.
2. Seleccione **Asociar**.
4. En **Red virtual**, seleccione **VNet-hub**.
5. En **Subred**, seleccione **GatewaySubnet**.
6. Seleccione **Aceptar**.

Ahora, cree la ruta predeterminada desde la subred radial.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de texto de búsqueda, escriba **tabla de rutas** y presione **Entrar**.
3. Seleccione **Tabla de rutas**.
5. Seleccione **Crear**.
6. En el nombre, escriba **UDR-DG**.
7. Seleccione **FW-Hybrid-Test** en grupo de recursos.
8. En **Ubicación**, seleccione **(EE. UU.) Este de EE. UU.** .
4. En **Propagación de rutas de puerta de enlace de red virtual**, seleccione **Deshabilitado**.
1. Seleccione **Crear**.
2. Una vez creada la tabla de rutas, selecciónela para abrir la página de la tabla de rutas.
3. Seleccione **Rutas** en la columna izquierda.
4. Seleccione **Agregar**.
5. En el nombre de ruta, escriba **ToHub**.
6. En el prefijo de dirección, escriba **0.0.0.0/0**.
7. En el tipo del próximo salto, seleccione **Aplicación virtual**.
8. En la dirección del próximo salto, escriba la dirección IP privada del firewall que anotó anteriormente.
9. Seleccione **Aceptar**.

Ahora asocie la ruta a la subred.

1. En la página **UDR-DG - Routes**, seleccione **Subredes**.
2. Seleccione **Asociar**.
4. En **Red virtual**, seleccione **VNet-spoke**.
5. En **Subred**, seleccione **SN-Workload**.
6. Seleccione **Aceptar**.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Ahora, cree la carga de trabajo de tipo hub-and-spoke y las máquinas virtuales locales, y colóquelas en las subredes adecuadas.

### <a name="create-the-workload-virtual-machine"></a>Creación de la máquina virtual de cargas de trabajo

Cree una máquina virtual en la red virtual radial, que ejecute IIS, sin ninguna dirección IP pública.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En la página **Popular**, seleccione **Windows Server 2016 Datacenter**.
3. Especifique estos valores para la máquina virtual:
    - **Grupo de recursos**: seleccione **FW-Hybrid-Test**.
    - **Nombre de la máquina virtual**: *VM-Spoke-01*.
    - **Región** -  *(EE. UU.) Este de EE. UU.)* .
    - **Nombre de usuario**: *azureuser*.
    - En **Contraseña**, escriba su contraseña.

4. Seleccione **Siguiente: Discos**.
5. Acepte los valores predeterminados y seleccione **Siguiente: Redes**.
6. Seleccione **VNet-Spoke** para la red virtual y la subred es **SN-Workload**.
7. En **IP pública**, seleccione **Ninguno**.
8. En **Puertos de entrada públicos**, seleccione **Permitir los puertos seleccionados** y, después, seleccione **HTTP (80)** y **RDP (3389)** .
9. Seleccione **Siguiente: Administración**.
10. En **Diagnósticos de arranque**, seleccione **Desactivado**.
11. Seleccione **Revisar y crear**, revise la configuración en la página de resumen y, después, seleccione **Crear**.

### <a name="install-iis"></a>Instalación de IIS

1. En Azure Portal, abra Cloud Shell y asegúrese de que está establecido en **PowerShell**.
2. Ejecute el siguiente comando para instalar IIS en la máquina virtual; cambie la ubicación si es necesario:

   ```azurepowershell-interactive
   Set-AzVMExtension `
           -ResourceGroupName FW-Hybrid-Test `
           -ExtensionName IIS `
           -VMName VM-Spoke-01 `
           -Publisher Microsoft.Compute `
           -ExtensionType CustomScriptExtension `
           -TypeHandlerVersion 1.4 `
           -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell      Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
           -Location EastUS
   ```

### <a name="create-the-on-premises-virtual-machine"></a>Creación de la máquina virtual local

Se trata de una máquina virtual con una dirección IP pública a la que se puede conectar mediante Escritorio remoto. Desde allí, puede conectarse al servidor local a través del firewall.

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. En la página **Popular**, seleccione **Windows Server 2016 Datacenter**.
3. Especifique estos valores para la máquina virtual:
    - **Grupo de recursos**: seleccione el existente y, después, **FW-Hybrid-Test**.
    - **Nombre de máquina virtual** - *VM-Onprem*.
    - **Región** -  *(EE. UU.) Este de EE. UU.)* .
    - **Nombre de usuario**: *azureuser*.
    - En **Contraseña**, escriba su contraseña.

4. Seleccione **Siguiente: Discos**.
5. Acepte los valores predeterminados y seleccione **Siguiente: Redes**.
6. Seleccione **VNet-Onprem** como red virtual y **SN-Corp** como subred.
7. En **Puertos de entrada públicos**, seleccione **Permitir los puertos seleccionados** y, después, seleccione **RDP (3389)** .
8. Seleccione **Siguiente: Administración**.
9. En **Diagnósticos de arranque**, seleccione **Desactivado**.
10. Seleccione **Revisar y crear**, revise la configuración en la página de resumen y, después, seleccione **Crear**.

## <a name="test-the-firewall"></a>Probar el firewall

1. En primer lugar, anote la dirección IP privada de la máquina virtual VM-Spoke-01 que aparece en la página de información general de esta máquina virtual.

2. En Azure Portal, conéctese a la máquina virtual **VM-Onprem**.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
3. Abra un explorador web en **VM-Onprem** y vaya a http://\<VM-spoke-01 private IP\>.

   Debería ver la página web de **VM-spoke-01**: ![Página web VM-Spoke-01](media/secure-hybrid-network/vm-spoke-01-web.png)

4. En la máquina virtual **VM-Onprem**, abra un escritorio remoto a **VM-spoke-01** en la dirección IP privada.

   Se realizará la conexión y podrá iniciar sesión.

Con ello, ha comprobado que las reglas de firewall funcionan:

<!---- You can ping the server on the spoke VNet.--->
- Puede examinar el servidor web en la red virtual de tipo hub-and-spoke.
- Puede conectarse al servidor en la red virtual de tipo hub-and-spoke mediante RDP.

A continuación, cambie la acción de recopilación de la regla de red del firewall a **Deny** (Denegar) para comprobar que las reglas del firewall funcionan según lo previsto.

1. Abra el grupo de recursos **FW-Hybrid-Test** y seleccione la directiva de firewall **Pol-Net01**.
2. En **Configuración**, seleccione **Reglas**.
3. En **Reglas de red**, seleccione la colección de reglas **RCNet01**, elija los puntos suspensivos (...) y seleccione **Editar**.
4. En **Acción de colección de reglas**, seleccione **Denegar**.
5. Seleccione **Guardar**.

Antes de probar las reglas modificadas, cierre los escritorios remotos y exploradores existentes en **VM-Onprem**. Una vez finalizada la actualización de la colección de reglas, vuelva a ejecutar las pruebas. Esta vez, todas producirán un error.

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede conservar los recursos relacionados con el firewall para el siguiente tutorial o, si ya no los necesita, eliminar el grupo de recursos **FW-Hybrid-Test** para eliminarlos todos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Protección de una red WAN virtual mediante Azure Firewall Manager](secure-cloud-network.md)