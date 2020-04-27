---
title: Uso de Azure Virtual WAN para crear una conexión de sitio a sitio a Azure | Microsoft Docs
description: En este tutorial, aprenda a usar Azure Virtual WAN para crear una conexión VPN de punto a sitio a Azure.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 04/16/2020
ms.author: alzam
ms.openlocfilehash: 11007bc39cb1112799c89afaf0ca670aa6760de6
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81482131"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Tutorial: Creación de una conexión VPN de usuario mediante Azure Virtual WAN

Este tutorial muestra cómo usar Virtual WAN para conectarse a los recursos de Azure a través de una conexión VPN de OpenVPN o IPsec/IKE (IKEv2). Este tipo de conexión requiere que se configure un cliente en el equipo cliente. Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una red de área extensa (WAN)
> * Crear un concentrador
> * Creación de una configuración de P2S
> * Descarga de un perfil de cliente VPN
> * Aplicar la configuración de P2S a un concentrador
> * Conectar una red virtual a un concentrador
> * Descargar y aplicar la configuración de cliente VPN
> * Visualizar la instancia de Virtual WAN

![Diagrama de Virtual WAN](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Antes de empezar

Antes de comenzar con la configuración, compruebe que se cumplen los criterios siguientes:

* Tiene una red virtual a la que quiere conectarse. Compruebe que ninguna de las subredes de sus redes locales se superpone a las redes virtuales a las que quiere conectarse. Para crear una red virtual en Azure Portal, consulte este [Inicio rápido](../virtual-network/quick-create-portal.md).

* Su red virtual no tiene ninguna puerta de enlace de red virtual. Si la red virtual tiene alguna puerta de enlace (ya sea VPN o ExpressRoute), tiene que quitarla. Esta configuración requiere que las redes virtuales estén conectadas a la puerta de enlace del centro de conectividad de Virtual WAN.

* Obtenga un intervalo de direcciones IP para la región del concentrador. El centro de conectividad es una red virtual que Virtual WAN crea y usa. El intervalo de direcciones que especifique para el centro de conectividad no se puede superponer a ninguna de las redes virtuales existentes a las que ya esté conectado. Igualmente no se puede superponer a los intervalos de direcciones con las que esté conectadas en el entorno local. Si no está familiarizado con los intervalos de direcciones IP ubicados en la configuración de la red local, póngase de acuerdo con alguien que pueda proporcionarle estos detalles.

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Creación de una instancia de Virtual WAN

Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

1. Vaya a la página de Virtual WAN. En el portal, haga clic en **+Crear un recurso**. Escriba **Virtual WAN** en el cuadro de búsqueda y seleccione ENTRAR.
2. Seleccione **Virtual WAN** en los resultados. En la página Virtual WAN, haga clic en **Crear** para abrir la página Crear una red WAN.
3. Dentro de la página **Crear una red WAN**, en la pestaña **Aspectos básicos**, rellene los campos siguientes:

   ![Red WAN virtual](./media/virtual-wan-point-to-site-portal/vwan.png)

   * **Suscripción**: seleccione la suscripción que quiere usar.
   * **Grupo de recursos**: cree uno nuevo o utilice uno ya existente.
   * **Ubicación del grupo de recursos**: elija una ubicación para los recursos en la lista desplegable. Una red WAN es un recurso global y no reside en una región determinada. De todas formas, tiene que seleccionar una región con el fin de administrar y ubicar más fácilmente el recurso WAN que cree.
   * **Nombre**: escriba el nombre que desea dar a la WAN.
   * **Tipo:** Estándar. Si crea una WAN básica, solo puede crear un centro de conectividad básico. Los centros de conectividad básicos solo pueden tener conectividad VPN de sitio a sitio.
4. Cuando termine de rellenar los campos, haga clic en **Revisión y creación**.
5. Una vez que se haya superado la validación, seleccione **Crear** para crear la WAN virtual.

## <a name="create-an-empty-virtual-hub"></a><a name="hub"></a>Crear un centro virtual vacío

1. En la WAN virtual, seleccione los centros y haga clic en **+ Nuevo centro**

   ![nuevo sitio](media/virtual-wan-point-to-site-portal/hub1.jpg)
2. En la página Crear centro de conectividad virtual, complete los siguientes campos.

   **Región**: seleccione la región en la que quiere implementar el centro de conectividad virtual.

   **Nombre**: escriba el nombre que quiere asignar al centro de conectividad virtual.

   **Espacio de direcciones privadas del centro de conectividad**: intervalo de direcciones del centro de conectividad en la notación CIDR.

   ![nuevo sitio](media/virtual-wan-point-to-site-portal/hub2.jpg)  
3. Haga clic en **Revisar y crear**
4. En la página **Validación superada**, haga clic en **Crear**

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>Creación de una configuración de P2S

Una configuración de P2S define los parámetros para conectarse a los clientes remotos.

1. Vaya a **Todos los recursos**.
2. Haga clic en la red WAN virtual que ha creado.
3. Haga clic en **+Crear una configuración de VPN de usuario** en la parte superior de la página para abrir la página **Crear nueva configuración de VPN de usuario**.

   ![nuevo sitio](media/virtual-wan-point-to-site-portal/p2s1.jpg)
4. En la página **Crear nueva configuración de VPN de usuario**, rellene los campos siguientes:

   **Nombre de la configuración**: nombre que desea usar para hacer referencia a su configuración.

   **Tipo de túnel**: el protocolo que se usará para el túnel.

   **Nombre del certificado raíz**: nombre descriptivo para el certificado.

   **Datos de certificado público**: datos del certificado X.509 codificado en Base 64.
  
5. Haga clic en **Crear** para crear la configuración.

## <a name="edit-hub-assignment"></a><a name="edit"></a>Edición de la asignación del concentrador

1. Vaya a la hoja **Centros** de la red WAN virtual.
2. Seleccione el centro con el que quiere asociar la configuración de servidor VPN y haga clic en **...**

   ![nuevo sitio](media/virtual-wan-point-to-site-portal/p2s4.jpg)
3. Haga clic en **Editar centro de conectividad virtual**.
4. Active la casilla **Incluir puerta de enlace de punto a sitio** y elija la **unidad de escalado de puerta de enlace** que quiera.

   ![nuevo sitio](media/virtual-wan-point-to-site-portal/p2s2.jpg)

En la tabla siguiente se muestran los detalles sobre las **unidades de escalado** disponibles

| **Unidad de escalado** | **Rendimiento** | **Conexiones de punto a sitio** |
| --- | --- | --- |
| 1| 500 Mbps | 500 |
| 2| 1 Gbps | 500 |
| 3| 1,5 Gbps | 500 |
| 4| 2 Gbps | 1000 |
| 5| 2,5 Gbps | 1000 |
| 6| 3 Gbps | 1000 |
| 7| 3,5 Gbps | 5000 |
| 8| 4 Gbps | 5000 |
| 9| 4,5 Gbps | 5000 |
| 10| 5 Gbps | 5000 |
| 11| 5,5 Gbps | 5000 |
| 12| 6 Gbps | 5000 |
| 13| 6,5 Gbps | 10000 |
| 14| 7 Gbps | 10000 |
| 15| 7,5 Gbps | 10000 |
| 16| 8 Gbps | 10000 |
| 17| 8,5 Gbps | 10000 |
| 18| 9 Gbps | 10000 |
| 19| 9,5 Gbps | 10000 |
| 20| 10 Gbps | 10000 |

5. Especifique el **Grupo de direcciones** del que se asignarán direcciones IP a los clientes VPN.
6. Haga clic en **Confirmar**
7. La operación puede tardar hasta 30 minutos en completarse.

## <a name="download-vpn-profile"></a><a name="download"></a>Descarga del perfil de VPN

Use el perfil de VPN para configurar los clientes.

1. En la página de su red WAN virtual, haga clic en **Configuraciones de VPN de usuario**.
2. En la parte superior de la página, haga clic en **Download user VPN config** (Descargar configuración de VPN de usuario).
3. Una vez el archivo se haya terminado de crear, puede hacer clic en el vínculo para descargarlo.
4. Use el archivo de perfil para configurar los clientes de VPN.

### <a name="configure-user-vpn-clients"></a>Configuración de clientes de VPN de usuario
Use el perfil descargado para configurar los clientes de acceso remoto. El procedimiento para cada sistema operativo es diferente, siga las instrucciones correctas a continuación:

#### <a name="microsoft-windows"></a>Microsoft Windows
##### <a name="openvpn"></a>OpenVPN

1. Descargue e instale el cliente OpenVPN desde el sitio web oficial.
2. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuraciones de VPN de usuario de Azure Portal o mediante New-AzureRmVpnClientConfiguration en PowerShell.
3. Descomprima el perfil. Abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN en el Bloc de notas.
4. Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en Base64. En un certificado con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados del certificado. Para conocer los pasos, consulte [cómo exportar un certificado para obtener la clave pública codificada](certificates-point-to-site.md).
5. Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en Base64. Para conocer los pasos, consulte [cómo extraer la clave privada](howto-openvpn-clients.md#windows).
6. No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
7. Copie el archivo vpnconfig.ovpn en la carpeta C:\Program Files\OpenVPN\config.
8. Haga clic con el botón derecho en l icono de OpenVPN en la bandeja del sistema y, después, haga clic en Conectar.

##### <a name="ikev2"></a>IKEv2

1. Seleccione los archivos de configuración de cliente VPN que correspondan a la arquitectura del equipo Windows. Si la arquitectura de procesador es de 64 bits, elija el paquete del instalador "VpnClientSetupAmd64". En caso de que sea de 32 bits, elija el paquete del instalador "VpnClientSetupX86".
2. Haga doble clic en el paquete para instalarlo. Si ve una ventana emergente de SmartScreen, haga clic en Más información y en Ejecutar de todas formas.
3. En el equipo cliente, vaya a Configuración de red y haga clic en VPN. La conexión VPN muestra el nombre de la red virtual a la que se conecta.
4. Antes de intentar conectarse, compruebe que ha instalado un certificado de cliente en el equipo cliente. Es necesario un certificado de cliente para la autenticación al usar el tipo de autenticación de certificados nativo de Azure. Para más información acerca de cómo generar certificados, consulte [Generación de certificados](certificates-point-to-site.md). Para obtener información acerca de cómo instalar un certificado de cliente, consulte [Instalación de certificados de cliente](../vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Visualización de la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.
2. En la página Información general, cada punto del mapa representa un concentrador.
3. En la sección de concentradores y conexiones, puede ver estado del concentrador, sitio, región, estado de la conexión VPN y bytes de entrada y salida.


## <a name="clean-up-resources"></a><a name="cleanup"></a>Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).
