---
title: 'Conexión a Windows Virtual Desktop desde Android: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente Android.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/25/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a7ca15a301de3c54195c0978aa31121c3624a98a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85209460"
---
# <a name="connect-with-the-android-client"></a>Conexión con el cliente de Android

> Se aplica a: Android 4.1 y posteriores, Chromebooks con ChromeOS 53 y posteriores.

>[!IMPORTANT]
>Este contenido se aplica a la actualización Spring 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa la versión Windows Virtual Desktop Fall 2019 sin objetos de Azure Resource Manager, vea [este artículo](./virtual-desktop-fall-2019/connect-android-2019.md).
>
> La actualización Spring 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

>[!NOTE]
> La capacidad para acceder a los recursos de Windows Virtual Desktop desde el cliente Android está disponible actualmente en versión preliminar.

Puede acceder a los recursos de Windows Virtual Desktop desde su dispositivo Android con nuestro cliente, que se puede descargar. El cliente Android también se puede usar en dispositivos Chromebook que admitan Google Play Store. En esta guía se le indicará cómo configurar el cliente Android.

## <a name="install-the-android-client"></a>Instalación del cliente Android

Para empezar, [descargue](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) e instale el cliente en su dispositivo Android.

## <a name="subscribe-to-a-feed"></a>Suscripción a una fuente

Suscríbase a la fuente que le proporcionó su administrador para obtener la lista de recursos administrados a los que puede acceder en el dispositivo Android.

Para suscribirse a una fuente:

1. En el centro de conexiones, pulse **+** y, después, pulse **Remote Resource Feed** (Fuente de recursos remotos).
2. Escriba la dirección URL de la fuente en el campo **Feed URL** (Dirección URL de la fuente). La dirección URL de la fuente puede ser una dirección URL o una dirección de correo electrónico.
   - Si usa una dirección URL, use la que le proporcionó el administrador, que lo habitual es que sea <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Para usar el correo electrónico, escriba su dirección de correo electrónico. El cliente buscará una dirección URL asociada a su dirección de correo electrónico si el administrador configuró el servidor de esa forma.
3. Pulse **NEXT** (Siguiente).
4. Escriba sus credenciales cuando se le solicite.
   - En **User Name** (Nombre de usuario), asigne al nombre de usuario permiso para acceder a los recursos.
   - En **Password** (Contraseña), escriba la contraseña asociada al nombre de usuario.
   - También se le puede solicitar que proporcione otra información si el administrador configuró la autenticación de esa forma.

Después de suscribirse, el centro de conexiones debería mostrar los recursos remotos.

Una vez que se haya suscrito a una fuente, el contenido de la misma se actualizará automáticamente de forma periódica. Pueden agregarse, cambiarse o quitarse recursos de acuerdo con los cambios hechos por el administrador.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el cliente Android, consulte [Introducción al cliente de Android](/windows-server/remote/remote-desktop-services/clients/remote-desktop-android/).
