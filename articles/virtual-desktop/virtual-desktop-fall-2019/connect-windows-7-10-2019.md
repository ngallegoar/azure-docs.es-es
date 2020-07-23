---
title: 'Conexión a Windows Virtual Desktop (otoño de 2019) en Windows 10 o 7: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente de escritorio de Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 07/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 78c400ffc6a84696a82c219294f133b729754259
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261563"
---
# <a name="connect-with-the-windows-desktop-client"></a>Conexión con el cliente de Escritorio de Windows

> Se aplica a: Windows 7, Windows 10 y Windows 10 IoT Enterprise

>[!IMPORTANT]
>Este contenido se aplica a la versión de otoño de 2019 que no admite objetos de Windows Virtual Desktop para Azure Resource Manager. Si está tratando de administrar objetos de Windows Virtual Desktop para Azure Resource Manager incorporados en la actualización de primavera de 2020, consulte [este artículo](../connect-windows-7-10.md).

Para acceder a los recursos de Windows Virtual Desktop en dispositivos con Windows 7, Windows 10 o Windows 10 IoT Enterprise use el cliente de escritorio de Windows.

>[!NOTE]
>De forma predeterminada, el cliente de Windows es la versión de otoño de 2019 de Windows Virtual Desktop. Sin embargo, si el cliente detecta que el usuario también tiene recursos de Azure Resource Manager, agrega automáticamente los recursos o informa al usuario de que están disponibles.

> [!IMPORTANT]
> Windows Virtual Desktop no es compatible con el cliente de Conexión de RemoteApp y Escritorio (RADC) ni con el cliente de Conexión a Escritorio remoto (MSTSC).

> [!IMPORTANT]
> Windows Virtual Desktop no es compatible actualmente con el cliente de Escritorio remoto de la tienda Windows.

## <a name="install-the-windows-desktop-client"></a>Instalación del cliente de escritorio de Windows

Elija el cliente que coincida con su versión de Windows:

- [Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32 bits](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Puede instalar el cliente para el usuario actual, lo que no requiere derechos de administrador, o bien el administrador puede instalar y configurar el cliente, con el fin de que todos los usuarios del dispositivo puedan acceder a él.

Una vez instalado, el cliente se puede iniciar desde el menú Inicio. Para ello, solo hay que buscar **Escritorio remoto**.

## <a name="subscribe-to-a-workspace"></a>Suscripción a un área de trabajo

Hay dos maneras de suscribirse a un área de trabajo. El cliente puede intentar detectar los recursos que usted tiene a disposición en su cuenta profesional o educativa, o bien puede especificar directamente la dirección URL en la que se encuentran los recursos para los casos en los que el cliente no pueda encontrarlos. Una vez que se haya suscrito a un área de trabajo, puede iniciar recursos con uno de los métodos siguientes:

- Ir al Centro de conexiones y haga doble clic en un recurso para iniciarlo.
- También puede ir al menú Inicio y buscar una carpeta con el nombre del área de trabajo o escribir el nombre del recurso en la barra de búsqueda.

### <a name="subscribe-with-a-user-account"></a>Suscripción con una cuenta de usuario

1. En la página principal del cliente, seleccione **Suscribirse**.
2. Inicie sesión con su cuenta de usuario cuando se le solicite.
3. Los recursos aparecerán en el Centro de conexiones y se agruparán por área de trabajo.

### <a name="subscribe-with-a-url"></a>Suscripción con una dirección URL

1. En la página principal del cliente, seleccione **Suscripción con una dirección URL**.
2. Escriba la dirección URL del área de trabajo o su dirección de correo electrónico:
   - Si usa la **URL del espacio de trabajo**, use la que le proporcionó el administrador. Si tiene acceso a recursos desde Windows Virtual Desktop, puede usar una de las siguientes direcciones URL:
     - Windows Virtual Desktop (otoño de 2019): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop (primavera de 2020): `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Si en su lugar utiliza el campo **Correo electrónico**, escriba su dirección de correo electrónico. Esto indica al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador ha configurado la [detección de correo electrónico](/windows-server/remote/remote-desktop-services/rds-email-discovery).
3. Seleccione **Next** (Siguiente).
4. Inicie sesión con su cuenta de usuario cuando se le solicite.
5. Los recursos deben aparecer en el Centro de conexiones, agrupados por área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el cliente de escritorio de Windows, consulte [Introducción al cliente de escritorio de Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
