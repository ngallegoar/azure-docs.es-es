---
title: 'Entorno de Windows Virtual Desktop: Azure'
description: Elementos básicos de un entorno de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: d189d5febe54a83cde4fe6cd6aa51e728aa882e6
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259894"
---
# <a name="windows-virtual-desktop-environment"></a>Entorno de Windows Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a la actualización Spring 2020 con objetos de Azure Resource Manager para Windows Virtual Desktop. Si usa la versión Fall 2019 de Windows Virtual Desktop sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/environment-setup-2019.md).
>
> La actualización Spring 2020 de Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. 
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows Virtual Desktop es un servicio que ofrece a los usuarios un acceso fácil y seguro a sus escritorios virtualizados y a RemoteApps. Este tema le proporcionará más información sobre la estructura general del entorno de Windows Virtual Desktop.

## <a name="host-pools"></a>Grupos de host

Un grupo de hosts es una colección de máquinas virtuales de Azure que se registran en Windows Virtual Desktop como hosts de sesión cuando ejecuta el agente de Windows Virtual Desktop. Todas las máquinas virtuales de host de sesión en un grupo de hosts deben provenir de la misma imagen para poder obtener una experiencia de usuario consistente.

Un grupo de hosts puede ser uno de estos dos tipos:

- Personal, donde cada host de sesión se asigna a usuarios individuales.
- Agrupado, donde los hosts de sesión pueden aceptar conexiones de cualquier usuario autorizado a un grupo de aplicaciones dentro del grupo de hosts.

Puede configurar propiedades adicionales en el grupo de hosts para cambiar el comportamiento de equilibrio de carga, cuántas sesiones puede tener cada host de sesión y lo que el usuario puede hacer con los hosts de sesión en el grupo de hosts mientras inicia sesión en Windows Virtual Desktop. Puede controlar los recursos publicados a los usuarios a través de grupos de aplicaciones.

## <a name="app-groups"></a>Grupos de aplicaciones

Un grupo de aplicaciones es una agrupación lógica de aplicaciones instaladas en hosts de sesión del grupo de hosts. Un grupo de aplicaciones puede ser uno de estos dos tipos:

- RemoteApp, donde los usuarios acceden a las instancias de RemoteApp que usted mismo seleccione y publique individualmente en el grupo de aplicaciones.
- Escritorio, donde los usuarios acceden al escritorio completo.

De forma predeterminada, un grupo de aplicaciones de escritorio (denominado "Grupo de aplicaciones de escritorio") se crea automáticamente cada vez que crea un grupo de hosts. Puede eliminar este grupo de aplicaciones en cualquier momento. Sin embargo, no puede crear otro grupo de aplicaciones de escritorio en el grupo de hosts mientras ya exista otro grupo de aplicaciones de escritorio. Para publicar instancias de RemoteApp, debe crear un grupo de aplicaciones de RemoteApp. Puede crear varios grupos de aplicaciones de RemoteApp para adaptarse a diferentes escenarios de trabajo. Los diferentes grupos de aplicaciones de RemoteApp también pueden contener instancias de RemoteApp superpuestas.

Para publicar recursos para los usuarios, debe asignarlos a grupos de aplicaciones. Cuando asigne usuarios a los grupos de aplicaciones, tenga en cuenta lo siguiente:

- No se puede asignar un usuario a un grupo de aplicaciones de escritorio y a un grupo de aplicaciones de RemoteApp en el mismo grupo de hosts. Sin embargo, los usuarios solo pueden iniciar un tipo de grupo de aplicaciones por sesión. Los usuarios no pueden iniciar ambos tipos de grupos de aplicaciones al mismo tiempo en una sola sesión.
- Se puede asignar un usuario a varios grupos de aplicaciones dentro del mismo grupo de hosts, y su fuente será una acumulación de ambos grupos de aplicaciones.

## <a name="workspaces"></a>Áreas de trabajo

Un área de trabajo es una agrupación lógica de grupos de aplicaciones de Windows Virtual Desktop. Cada grupo de aplicaciones de Windows Virtual Desktop debe estar asociado a un área de trabajo para que los usuarios vean las aplicaciones remotas y los escritorio publicados en ellos.  

## <a name="end-users"></a>Usuarios finales

Después de asignar usuarios a sus grupos de aplicaciones, pueden conectarse a una implementación de Windows Virtual Desktop con cualquiera de los clientes de Windows Virtual Desktop.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el acceso delegado y cómo asignar roles a los usuarios en [Acceso delegado en Windows Virtual Desktop](delegated-access-virtual-desktop.md).

Para información sobre cómo configurar el grupo de hosts de Windows Virtual Desktop, consulte [Creación de un grupo de hosts con Azure Portal](create-host-pools-azure-marketplace.md).

Para obtener información sobre cómo conectarse a Windows Virtual Desktop, consulte uno de los siguientes artículos:

- [Conexión con Windows 10 o Windows 7](connect-windows-7-10.md)
- [Conexión con un explorador web](connect-web.md)
- [Conexión con el cliente de Android](connect-android.md)
- [Conexión con el cliente macOS](connect-macos.md)
- [Conexión con el cliente iOS](connect-ios.md)