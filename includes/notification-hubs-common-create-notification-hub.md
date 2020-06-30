---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 7c3887367b6365ea3577bbff6cc19bf34d178ee6
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095502"
---
### <a name="create-a-notification-hub"></a>Creación de un centro de notificaciones 

En esta sección, creará un centro de notificaciones y configurará la autenticación con **APNS**. Puede usar un certificado push P12 o una autenticación basada en token. Si desea usar un centro de notificaciones ya creado, puede omitir los pasos hasta el paso 5.

1. Inicie sesión en [Azure](https://portal.azure.com).

1. Haga clic en **Crear un recurso**, busque y elija **Centro de notificaciones** y, a continuación, haga clic en **Crear**.

1. Actualice los campos siguientes y haga clic en **Crear**:

    **DETALLES BÁSICOS**  

    **Subscription** (Suscripción): Elija la **suscripción** de destino en la lista desplegable.  
    **Grupos de recursos:** Cree un **grupo de recursos** o escoja uno ya existente.  

    **DETALLES DEL ESPACIO DE NOMBRES**  

    **Espacio de nombres de Notification Hubs:** escriba un nombre único global para el espacio de nombres de **Notification Hubs**.  

    > [!NOTE]
    > Asegúrese de que la opción **Crear nuevo** está seleccionada para este campo.

    **DETALLES DE NOTIFICATION HUBS**  

    **Notification Hubs:** especifique un nombre para el **centro de notificaciones**.  
    **Ubicación:** elija una ubicación adecuada en la lista desplegable.  
    **Plan de tarifa:** mantenga la opción predeterminada **Gratis**,  

    > [!NOTE]
    > a menos que haya alcanzado el número máximo de centros del nivel Gratis.

1. Una vez que se haya aprovisionado el **centro de notificaciones**, vaya a ese recurso.
1. Vaya al nuevo **centro de notificaciones**.
1. Seleccione **Directivas de acceso** en la lista (en **ADMINISTRAR**.
1. Anote los valores de **nombre de directiva** junto con sus valores correspondientes de **cadena de conexión**.
