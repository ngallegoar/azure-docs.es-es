---
title: 'Conexión al cliente web de Windows Virtual Desktop: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente web.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 09/24/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: bb3a73566cd2477e493ca8621336fb3a51f3b3b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212469"
---
# <a name="connect-with-the-web-client"></a>Conexión con el cliente web

>[!IMPORTANT]
>Este contenido se aplica a la actualización de primavera de 2020 con objetos de Windows Virtual Desktop para Azure Resource Manager. Si usa la versión de otoño de 2019 de Windows Virtual Desktop sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/connect-web-2019.md).
>
> La actualización de primavera de 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El cliente web le permite acceder a los recursos de Windows Virtual Desktop desde un explorador web sin el largo proceso de instalación.

>[!NOTE]
>El cliente web no tiene compatibilidad con SO móvil actualmente.

## <a name="supported-operating-systems-and-browsers"></a>Exploradores y sistemas operativos compatibles

Aunque debería funcionar cualquier explorador compatible con HTML5, oficialmente se admiten los siguientes sistemas operativos y exploradores.

| Browser           | SO admitido                     | Notas               |
|-------------------|----------------------------------|---------------------|
| Microsoft Edge    | Windows                          |                     |
| Internet Explorer | Windows                          |                     |
| Apple Safari      | macOS                            |                     |
| Mozilla Firefox   | Windows, macOS, Linux            | Versión 55 o posterior |
| Google Chrome     | Windows, macOS, Linux, Chrome OS |                     |

## <a name="access-remote-resources-feed"></a>Acceso remoto a la fuente de recursos

En un explorador, vaya a la versión integrada de Windows Virtual Desktop del cliente web de Azure Resource Manager, en <https://rdweb.wvd.microsoft.com/arm/webclient>, e inicie sesión con su cuenta de usuario.

>[!NOTE]
>En cambio, si usa la versión de otoño de 2019 de Windows Virtual Desktop sin integración con Azure Resource Manager, conecte los recursos en <https://rdweb.wvd.microsoft.com/webclient>.

>[!NOTE]
>Si ya ha iniciado sesión con una cuenta de Azure Active Directory diferente a la que quiere usar para Windows Virtual Desktop, debe cerrar la sesión o usar una ventana privada del explorador.

Después de iniciar sesión, debería ver una lista de recursos. Puede iniciar los recursos seleccionándolos de la misma forma en que lo haría con una aplicación normal en la pestaña **Todos los recursos**.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo usar el cliente web, consulte [Introducción al cliente web](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client).
