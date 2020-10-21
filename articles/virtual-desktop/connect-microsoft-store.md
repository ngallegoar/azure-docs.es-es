---
title: 'Conexión con el cliente de Microsoft Store: Azure'
description: Cómo conectarse a Windows Virtual Desktop mediante el cliente de Microsoft Store.
author: Heidilohr
ms.topic: how-to
ms.date: 10/05/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9bab2a11ff9e7907621087e5027929e3e42eaf29
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91744696"
---
# <a name="connect-with-the-microsoft-store-client"></a>Conexión con el cliente de Microsoft Store

>Se aplica a: Windows 10

Puede acceder a los recursos de Windows Virtual Desktop en dispositivos con Windows 10.

## <a name="install-the-microsoft-store-client"></a>Instalación del cliente de Microsoft Store

Puede instalar el cliente para el usuario actual, que no requiere derechos de administrador. Como alternativa, el administrador puede instalar y configurar el cliente para que todos los usuarios del dispositivo puedan acceder a él.

Una vez instalado, el cliente se puede iniciar desde el menú Inicio. Para ello, solo hay que buscar Escritorio remoto.

Para comenzar, [descargue e instale el cliente desde Microsoft Store](https://www.microsoft.com/store/productId/9WZDNCRFJ3PS).

## <a name="subscribe-to-a-workspace"></a>Suscripción a un área de trabajo

Suscríbase al área de trabajo que proporciona su administrador para obtener la lista de recursos administrados a los que puede acceder en el PC.

Para suscribirse a un área de trabajo:

1. En la pantalla del centro de conexiones, pulse **+Agregar** y, después, **Áreas de trabajo**.
2. Escriba la dirección URL del área de trabajo en el campo URL del área de trabajo proporcionado por el administrador. La dirección URL del área de trabajo puede ser una dirección URL o una dirección de correo electrónico.
   
   - Si usa una URL del área de trabajo, utilice la que le proporcionó el administrador.
   - Si se va a conectar desde Windows Virtual Desktop, use una de las siguientes direcciones URL en función de la versión del servicio que use:
       - Windows Virtual Desktop (clásico): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`.
       - Windows Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`.
  
3. Pulse **Suscribirse**.
4. Escriba sus credenciales cuando se le solicite.
5. Después de suscribirse, las áreas de trabajo deben mostrarse en el centro de conexiones.

Se pueden agregar, modificar o eliminar áreas de trabajo en función de los cambios realizados por el administrador.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar el cliente de Microsoft Store, vea [Introducción al cliente de Microsoft Store](/windows-server/remote/remote-desktop-services/clients/windows/).