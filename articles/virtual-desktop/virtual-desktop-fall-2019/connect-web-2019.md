---
title: 'Conexión al cliente web de Windows Virtual Desktop: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente web.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8b73a6bf2c8b853d9e3e939b6c8ff6fc05ac7021
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83657263"
---
# <a name="connect-with-the-web-client"></a>Conexión con el cliente web

>[!IMPORTANT]
>Este contenido se aplica a la versión Fall 2019 que no admite objetos de Windows Virtual Desktop para Azure Resource Manager. Si está tratando de administrar objetos de Windows Virtual Desktop para Azure Resource Manager introducidos en la actualización de Spring 2020, consulte [este artículo](../connect-web.md).

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

En un explorador, vaya al cliente web de Windows Virtual Desktop en <https://rdweb.wvd.microsoft.com/webclient> e inicie sesión con su cuenta de usuario.

>[!NOTE]
>En cambio, si usa la versión de primavera de 2020 de Windows Virtual Desktop con integración con Azure Resource Manager, conecte los recursos en <https://rdweb.wvd.microsoft.com/arm/webclient>.

>[!NOTE]
>Si ya ha iniciado sesión con una cuenta de Azure Active Directory diferente a la que quiere usar para Windows Virtual Desktop, debe cerrar la sesión o usar una ventana privada del explorador.

Después de iniciar sesión, debería ver una lista de recursos. Puede iniciar los recursos seleccionándolos de la misma forma en que lo haría con una aplicación normal en la pestaña **Todos los recursos**.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo usar el cliente web, consulte [Introducción al cliente web](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client).
