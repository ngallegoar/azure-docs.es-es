---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 537c86512db4706077d0089a4f71ee945623c26b
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095492"
---
### <a name="configure-your-notification-hub-with-apns-information"></a>Configuración de un Centro de notificaciones con información de APNS

En **Servicios de notificaciones**, seleccione **Apple** y siga los pasos adecuados en función del enfoque elegido anteriormente en la sección [Creación de un certificado para Notification Hubs](#creating-a-certificate-for-notification-hubs).  

> [!NOTE]
> Use **Production** (Producción) como **Application Mode** (Modo de aplicación) solo si desea enviar notificaciones push a los usuarios que compraron la aplicación en la tienda.

### <a name="option-1-using-a-p12-push-certificate"></a>OPCIÓN 1: Uso de un certificado push .p12

1. Seleccione **Certificado**.

1. Seleccione el icono del archivo.

1. Seleccione el archivo .p12 que exportó antes y, luego, **Open** (Abrir).

1. Si es necesario, especifique la contraseña correcta.

1. Seleccione el modo **Espacio aislado**.

1. Seleccione **Guardar**.

### <a name="option-2-using-token-based-authentication"></a>OPCIÓN 2: Uso de autenticación basada en tokens

1. Seleccione **Token**.
1. Escriba los siguientes valores adquiridos anteriormente:

    - **Identificador de clave**
    - **Identificador de lote**
    - **Identificador de equipo**
    - **Token**

1. Seleccione **Espacio aislado**.
1. Seleccione **Guardar**.
