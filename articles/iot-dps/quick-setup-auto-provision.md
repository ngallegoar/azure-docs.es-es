---
title: Configuración de IoT Hub Device Provisioning Service en Azure Portal
description: 'Inicio rápido: Configuración de IoT Hub Device Provisioning Service (DPS) en Azure Portal'
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 765b7e5bb7341974a6a16d3f1ed0bcabf415f9f3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "76029182"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-the-azure-portal"></a>Inicio rápido: Configuración de Azure IoT Hub Device Provisioning Service con Azure Portal

IoT Hub Device Provisioning Service se puede usar con IoT Hub para habilitar el aprovisionamiento sin interacción Just-In-Time a la instancia deseada de IoT Hub sin necesidad de intervención humana, lo que permite a los clientes aprovisionar millones de dispositivos IoT de forma segura y escalable. IoT Hub Device Provisioning Service admite dispositivos IoT con las autenticaciones TPM, de clave simétrica y del certificado X.509. Para más información, consulte [Introducción a IoT Hub Device Provisioning Service](./about-iot-dps.md)

En este inicio rápido aprenderá a configurar IoT Hub Device Provisioning Service en Azure Portal para aprovisionar los dispositivos con los siguientes pasos:
> [!div class="checklist"]
> * Uso de Azure Portal para crear una instancia de IoT Hub
> * Usar Azure Portal para crear un servicio IoT Hub Device Provisioning y obtener el ámbito del identificador
> * Vincular la instancia de IoT Hub al servicio Device Provisioning


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]


## <a name="create-a-new-iot-hub-device-provisioning-service"></a>Creación de una instancia de IoT Hub Device Provisioning Service

1. Vuelva a seleccionar el botón **+ Crear un recurso**.

2. En *Buscar en Marketplace*, escriba **Device Provisioning Service**. Seleccione **IoT Hub Device Provisioning Service** y pulse el botón **Crear**. 

3. Especifique la siguiente información para la nueva instancia de Device Provisioning Service y pulse **Crear**.

    * **Nombre:** escriba un nombre único para la nueva instancia de Device Provisioning Service. Si el nombre que escribe está disponible, aparece una marca de verificación verde.
    * **Subscription** (Suscripción): elija la suscripción que desea usar para crear esta instancia de Device Provisioning Service.
    * **Grupo de recursos:** este campo le permite crear un nuevo grupo de recursos o elegir uno existente para que contenga la nueva instancia. Elija el mismo grupo de recursos que contenga el centro de Iot que creó anteriormente, por ejemplo, **TestResources**. Al colocar juntos todos los recursos relacionados en un grupo, puede administrarlos juntos también. Por ejemplo, si elimina el grupo de recursos también se eliminarán todos los recursos que contiene. Para más información, consulte [Administración de grupos de recursos de Azure Resource Manager](../azure-resource-manager/management/manage-resource-groups-portal.md).
    * **Ubicación:** seleccione la ubicación más cercana a sus dispositivos.

      ![Especificar la información básica de la instancia de Device Provisioning Service en la hoja del portal](./media/quick-setup-auto-provision/create-iot-dps-portal.png)  

4. Seleccione en el botón de notificación para supervisar la creación de la instancia del recurso. Una vez que el servicio se implemente correctamente, seleccione **Anclar al panel** y, después, **Ir al recurso**.

    ![Supervisión de la notificación de implementación](./media/quick-setup-auto-provision/pin-to-dashboard.png)

## <a name="link-the-iot-hub-and-your-device-provisioning-service"></a>Vínculo al centro de IoT y a Device Provisioning Service

En esta sección, agregará una configuración a la instancia de Device Provisioning Service. Esta configuración establece el centro de IoT para el que los dispositivos se aprovisionarán.

1. Seleccione el botón **Todos los recursos** en el menú de la izquierda de Azure Portal. Seleccione la instancia del servicio Device Provisioning que creó en la sección anterior. 

    Si el menú se configura mediante un **control flotante** en lugar del modo **acoplado** en la configuración del portal, tendrá que hacer clic en las tres líneas de la parte superior izquierda para abrir el menú del portal a la izquierda.  

2. En el menú de Device Provisioning Service, seleccione **Instancias de IoT Hub vinculadas**. Pulse el botón **+ Agregar** que aparece en la parte superior. 

3. En la página **Add link to IoT hub** (Agregar vínculo al centro de IoT), proporcione la siguiente información para vincular la nueva instancia del servicio Device Provisioning al centro de IoT. Luego, presione **Guardar**. 

    * **Subscription** (Suscripción): seleccione la suscripción que contenga el centro de IoT que desee vincular a la nueva instancia de Device Provisioning Service.
    * **IoT Hub:** seleccione el centro de IoT que va a vincular con la nueva instancia de Device Provisioning Service.
    * **Directiva de acceso:** seleccione **iothubowner** como las credenciales para establecer el vínculo con el centro de IoT.  

      ![Vínculo del nombre del centro a la instancia de Device Provisioning Service en la hoja del portal](./media/quick-setup-auto-provision/link-iot-hub-to-dps-portal.png)  

3. Ahora debería ver el centro seleccionado en la hoja **Linked IoT hubs** (Centros de IoT vinculados). Es posible que tenga que pulsar **Actualizar** para que aparezca.


## <a name="clean-up-resources"></a>Limpieza de recursos

Otras guías de inicio rápido de esta colección se basan en los valores de esta. Si tiene previsto seguir trabajando con las siguientes guías de inicio rápido o tutoriales, no elimine los recursos creados en esta guía de inicio rápido. Si no tiene previsto continuar, siga estos pasos para eliminar todos los recursos creados en esta guía de inicio rápido en Azure Portal.

1. En el menú de la izquierda de Azure Portal, seleccione **Todos los recursos** y seleccione Device Provisioning Service. En la parte superior del panel de detalles del dispositivo, seleccione **Eliminar**.  
2. En el menú de la izquierda de Azure Portal, seleccione **Todos los recursos** y después su centro de IoT. En la parte superior del panel de detalles del centro, seleccione **Eliminar**.  

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha implementado un centro de IoT y una instancia de Device Provisioning Service, y ha vinculado los dos recursos. Para aprender a usar esta configuración para aprovisionar un dispositivo simulado, vaya al inicio rápido en el que se explica cómo crear un dispositivo simulado.

> [!div class="nextstepaction"]
> [Inicio rápido para crear un dispositivo simulado](./quick-create-simulated-device-symm-key.md)
