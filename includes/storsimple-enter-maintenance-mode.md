---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d35f0ef783a2c48f8211657bc8829635c19495aa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186342"
---
#### <a name="to-enter-maintenance-mode"></a>Para acceder al modo de mantenimiento
1. En el menú de la consola serie, seleccione la opción 1, **Iniciar sesión con acceso completo**.
2. Escriba la contraseña. La contraseña predeterminada es **Password1**.
3. En el símbolo del sistema, escriba
   
     `Enter-HcsMaintenanceMode`
4. Verá un mensaje de advertencia que indica que el modo de mantenimiento interrumpe todas las solicitudes de E/S y detiene la conexión al Portal de Azure clásico. También se le solicitará la confirmación. Escriba **Y** para acceder al modo de mantenimiento.
   
    Ambos controladores se reiniciarán. Una vez completado el reinicio, aparecerá otro mensaje que indica que el dispositivo está en modo de mantenimiento.

