---
title: 'Conexión a Windows Virtual Desktop desde iOS: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente iOS.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3bf9ea6c68e4cbbe721705639e6c6416c0e93c9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128254"
---
# <a name="connect-with-the-ios-client"></a>Conexión con el cliente iOS

> Se aplica a: iOS 13.0 una versión posterior. Compatible con iPhone, iPad y iPod touch.

Puede acceder a los recursos de Windows Virtual Desktop desde su dispositivo iOS con nuestro cliente, que se puede descargar. En esta guía se le indicará cómo configurar el cliente iOS.

## <a name="install-the-ios-client"></a>Instalación del cliente de iOS

Para empezar, [descargue](https://aka.ms/rdios) e instale el cliente en su dispositivo iOS.

## <a name="subscribe-to-a-feed"></a>Suscripción a una fuente

Suscríbase a la fuente que proporciona su administrador para obtener la lista de recursos administrados a los que puede acceder en el dispositivo iOS.

Para suscribirse a una fuente:

1. En el centro de conexiones, pulse **+** y, después, pulse **Add Workspace** (Agregar área de trabajo).
2. Escriba la dirección URL de la fuente en el campo **Feed URL** (Dirección URL de la fuente). La dirección URL de la fuente puede ser una dirección URL o una dirección de correo electrónico.
   - Si usa una dirección URL, use la que le proporcionó el administrador. Normalmente, la dirección URL es <https://rdweb.wvd.microsoft.com>.
   - Para usar el correo electrónico, escriba su dirección de correo electrónico. Esto indica al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador configuró el servidor de esa forma.
3. Pulse **Next** (Siguiente).
4. Escriba sus credenciales cuando se le solicite.
   - En **User Name** (Nombre de usuario), asigne al nombre de usuario permiso para acceder a los recursos.
   - En **Password** (Contraseña), escriba la contraseña asociada al nombre de usuario.
   - También se le puede solicitar que proporcione otra información si el administrador configuró la autenticación de esa forma.
5. Pulse **Save** (Guardar).

Después, el centro de conexiones debería mostrar los recursos remotos.

Una vez que se haya suscrito a una fuente, el contenido de la misma se actualizará automáticamente de forma periódica. Pueden agregarse, cambiarse o quitarse recursos de acuerdo con los cambios hechos por el administrador.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el cliente de iOS, consulte el documento [Introducción al cliente de iOS](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/).
