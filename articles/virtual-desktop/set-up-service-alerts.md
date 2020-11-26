---
title: 'Configuración de las alertas de servicio para Windows Virtual Desktop: Azure'
description: Configuración de Azure Service Health para recibir notificaciones del servicio de Windows Virtual Desktop.
author: Heidilohr
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 66617afee11b02eae0ba5e36d9ff91cbdf21911f
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95023112"
---
# <a name="tutorial-set-up-service-alerts"></a>Tutorial: Configuración de alertas de servicio

>[!IMPORTANT]
>Este contenido se aplica a Windows Virtual Desktop con objetos de Windows Virtual Desktop de Azure Resource Manager. Si usa Windows Virtual Desktop (clásico) sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/set-up-service-alerts-2019.md).

Puede usar Azure Service Health para supervisar los problemas del servicio y los avisos de estado de Windows Virtual Desktop. Azure Service Health puede notificarlo con distintos tipos de alertas (por ejemplo, por correo electrónico o SMS), ayudarlo a entender el impacto de un problema y mantenerlo actualizado con respecto a la resolución del problema. Azure Service Health también puede ayudarle a mitigar el tiempo de inactividad y a preparar el mantenimiento planeado y los cambios que podrían afectar a la disponibilidad de los recursos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear y configurar las alertas de servicio.

Para más información sobre Azure Service Health, consulte la [documentación sobre el estado de Azure](../service-health/index.yml).

## <a name="create-service-alerts"></a>Creación de alertas de servicio

En esta sección se muestra cómo configurar Azure Service Health y cómo establecer las notificaciones a las que puede acceder en Azure Portal. Puede configurar distintos tipos de alertas y programarlas para recibir notificaciones de manera oportuna.

### <a name="recommended-service-alerts"></a>Alertas de servicio recomendadas

Se recomienda crear alertas de servicio para los tipos de eventos de estado siguientes:

- **Problema de servicio:** reciba notificaciones sobre los problemas importantes que afectan la conectividad de los usuarios con el servicio o la capacidad de administrar el inquilino de Windows Virtual Desktop.
- **Aviso de estado:** reciba las notificaciones que requiere su atención. Estos son algunos ejemplos de este tipo de notificación:
    - Máquinas virtuales (VM) no configuradas de manera segura con el puerto 3389 abierto.
    - Funcionalidad en desuso.

### <a name="configure-service-alerts"></a>Configuración de las alertas de servicio

Para configurar las alertas de servicio:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Service Health**.
3. Siga las instrucciones que aparecen en [Creación de alertas del registro de actividad en notificaciones del servicio](../service-health/alerts-activity-log-service-notifications-portal.md?toc=%2fazure%2fservice-health%2ftoc.json) para configurar las alertas y las notificaciones.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a configurar y usar Azure Service Health para supervisar los problemas del servicio y los avisos de estado de Windows Virtual Desktop. Para información sobre el inicio de sesión en Windows Virtual Desktop, continúe con los procedimientos de conexión a Windows Virtual Desktop.

> [!div class="nextstepaction"]
> [Conexión al cliente de Escritorio remoto en Windows 7 y Windows 10](./connect-windows-7-10.md)