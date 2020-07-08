---
title: 'Conexión a Windows Virtual Desktop desde macOS: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente macOS.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f2b4696886c898eec5895c4ccb59347cf2a49f03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85209001"
---
# <a name="connect-with-the-macos-client"></a>Conexión con el cliente macOS

> Se aplica a: macOS 10.12, o cualquier versión posterior

>[!IMPORTANT]
>Este contenido se aplica a la actualización Spring 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa la versión Windows Virtual Desktop Fall 2019 sin objetos de Azure Resource Manager, vea [este artículo](./virtual-desktop-fall-2019/connect-macos-2019.md).
>
> La actualización Spring 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede acceder a los recursos de Windows Virtual Desktop desde sus dispositivos macOS con nuestro cliente, que se puede descargar. En esta guía se le indicará cómo configurar el cliente.

## <a name="install-the-client"></a>Instalar el cliente

Para empezar, [descargue](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) e instale el cliente en su dispositivo macOS.

## <a name="subscribe-to-a-feed"></a>Suscripción a una fuente

Suscríbase a la fuente que le proporcionó el administrador para obtener la lista de recursos administrados que tiene a su disposición en el dispositivo macOS.

Para suscribirse a una fuente:

1. Seleccione **Agregar área de trabajo** en la página principal para conectarse al servicio y recuperar los recursos.
2. Escriba la dirección URL de la fuente. Puede ser una dirección URL o una dirección de correo electrónico:
   - Si usa una dirección URL, use la que le proporcionó el administrador. Normalmente, la dirección URL es <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Para usar el correo electrónico, escriba su dirección de correo electrónico. Esto indica al cliente que busque una dirección URL asociada a su dirección de correo electrónico si el administrador configuró el servidor de esa forma.
3. Seleccione **Agregar**.
4. Inicie sesión con su cuenta de usuario cuando se le solicite.

Una vez que haya iniciado sesión, debería ver una lista de recursos disponibles.

Una vez que se haya suscrito a una fuente, el contenido de la misma se actualizará automáticamente de forma periódica. Pueden agregarse, cambiarse o quitarse recursos de acuerdo con los cambios hechos por el administrador.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el cliente macOS, consulte el documento [Introducción al cliente de macOS](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/).
