---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9caf63fc90be7bae0461ddc24c94594a32199765
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91812711"
---
#### <a name="microsoft-windows"></a>Microsoft Windows

##### <a name="openvpn"></a>OpenVPN

1. Descargue e instale el cliente OpenVPN desde el sitio web oficial.
1. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuraciones de VPN de usuario de Azure Portal o mediante New-AzureRmVpnClientConfiguration en PowerShell.
1. Descomprima el perfil. Abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN en el Bloc de notas.
1. Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en Base64. En los certificados con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados de certificados. Para conocer los pasos, consulte [cómo exportar un certificado para obtener la clave pública codificada](../articles/virtual-wan/certificates-point-to-site.md).
1. Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en Base64. Para conocer los pasos, consulte [cómo extraer la clave privada](../articles/virtual-wan/howto-openvpn-clients.md#windows).
1. No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
1. Copie el archivo vpnconfig.ovpn en la carpeta C:\Program Files\OpenVPN\config.
1. Haga clic con el botón derecho en el icono OpenVPN en la bandeja del sistema y, después, seleccione **Conectar**.

##### <a name="ikev2"></a>IKEv2

1. Seleccione los archivos de configuración de cliente VPN que correspondan a la arquitectura del equipo Windows. Si la arquitectura de procesador es de 64 bits, elija el paquete del instalador "VpnClientSetupAmd64". En caso de que sea de 32 bits, elija el paquete del instalador "VpnClientSetupX86".
1. Haga doble clic en el paquete para instalarlo. Si ve una ventana emergente de SmartScreen, seleccione **Más información** y, después, **Ejecutar de todas formas**.
1. En el equipo cliente, vaya a **Configuración de red** y haga clic en **VPN**. La conexión VPN muestra el nombre de la red virtual a la que se conecta.
1. Antes de intentar conectarse, compruebe que ha instalado un certificado de cliente en el equipo cliente. Es necesario un certificado de cliente para la autenticación al usar el tipo de autenticación de certificados nativo de Azure. Para más información acerca de cómo generar certificados, consulte [Generación de certificados](../articles/virtual-wan/certificates-point-to-site.md). Para obtener información acerca de cómo instalar un certificado de cliente, consulte [Instalación de certificados de cliente](../articles/vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).
