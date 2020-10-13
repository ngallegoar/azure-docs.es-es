---
title: 'Conexión a Windows Virtual Desktop en Windows 10 o 7: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente de escritorio de Windows.
author: Heidilohr
ms.topic: how-to
ms.date: 09/22/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c9da2acac0957d7fe06d0249775fbed73b5f458d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91287345"
---
# <a name="connect-with-the-windows-desktop-client"></a>Conexión con el cliente de Escritorio de Windows

> Se aplica a: Windows 10, Windows 10 IoT Enterprise y Windows 7

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop con objetos de Windows Virtual Desktop de Azure Resource Manager. Si usa Windows Virtual Desktop (clásico) sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/connect-windows-7-10-2019.md).

Para acceder a los recursos de Windows Virtual Desktop en dispositivos con Windows 10, Windows 10 IoT Enterprise y Windows 7, use el cliente de escritorio de Windows. El cliente no es compatible con Windows 8 ni Windows 8.1.

## <a name="install-the-windows-desktop-client"></a>Instalación del cliente de escritorio de Windows

Elija el cliente que coincida con su versión de Windows:

- [Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows de 32 bits](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Puede instalar el cliente para el usuario actual, lo que no requiere derechos de administrador, o bien el administrador puede instalar y configurar el cliente, con el fin de que todos los usuarios del dispositivo puedan acceder a él.

Una vez instalado, el cliente se puede iniciar desde el menú Inicio. Para ello, solo hay que buscar **Escritorio remoto**.

> [!IMPORTANT]
> Windows Virtual Desktop no es compatible con el cliente de Conexión de RemoteApp y Escritorio (RADC) ni con el cliente de Conexión a Escritorio remoto (MSTSC).

## <a name="subscribe-to-a-workspace"></a>Suscripción a un área de trabajo

Hay dos maneras de suscribirse a un área de trabajo. El cliente puede intentar detectar los recursos disponibles en tu cuenta profesional o educativa, o bien puedes especificar directamente la dirección URL en la que se encuentran los recursos para los casos en los que el cliente no pueda encontrarlos. Una vez que te hayas suscrito a un área de trabajo, podrás iniciar los recursos con uno de los métodos siguientes:

- Ve al centro de conexiones y haz doble clic en un recurso para iniciarlo.
- También puedes ir al menú Inicio y buscar una carpeta con el nombre del área de trabajo o escribir el nombre del recurso en la barra de búsqueda.

### <a name="subscribe-with-a-user-account"></a>Suscripción con una cuenta de usuario

1. En la página principal del cliente, seleccione **Subscribe** (Suscribirse).
2. Inicie sesión con su cuenta de usuario cuando se le solicite.
3. Los recursos aparecerán en el Centro de conexiones y se agruparán por área de trabajo.

>[!NOTE]
>De forma predeterminada, el cliente de Windows es Windows Virtual Desktop (clásico). Sin embargo, si el cliente detecta que el usuario también tiene recursos de Azure Resource Manager, agrega automáticamente los recursos o informa al usuario de que están disponibles.

### <a name="subscribe-with-a-url"></a>Suscripción con una dirección URL

1. En la página principal del cliente, seleccione **Subscribe with URL** (Suscribirse con URL).
2. Escriba la dirección URL del área de trabajo o su dirección de correo electrónico:
   - Si usas la **URL del espacio de trabajo**, usa la que te proporcionó el administrador. Si accedes a los recursos de Windows Virtual Desktop, puedes usar una de las siguientes direcciones URL:
     - Windows Virtual Desktop (clásico): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
     - Windows Virtual Desktop (US Gov): `https://rdweb.wvd.azure.us/api/arm/feeddiscovery`
   - Si en su lugar utiliza el campo **Email** (Correo electrónico), escriba su dirección de correo electrónico. Esto indica al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador ha configurado la [detección de correo electrónico](/windows-server/remote/remote-desktop-services/rds-email-discovery).
3. Seleccione **Next** (Siguiente).
4. Inicie sesión con su cuenta de usuario cuando se le solicite.
5. Los recursos aparecerán en el Centro de conexiones, agrupados por área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el cliente de escritorio de Windows, consulte [Introducción al cliente de escritorio de Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
