---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/30/2020
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 76c8eb7acf20d8cf68d3573defd947efbc6c3c43
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "85801743"
---
Una de las funcionalidades clave de Azure IoT Edge es que puede implementar código en dispositivos de IoT Edge desde la nube. Los *módulos de IoT Edge* son paquetes ejecutables que se implementan como contenedores. En esta sección, va a implementar un módulo pregenerado desde la [sección de módulos de IoT Edge de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) directamente desde la instancia de Azure IoT Hub.

El módulo que se implementa en esta sección simula un sensor y envía los datos generados. Este módulo es un fragmento de código útil para empezar a trabajar con IoT Edge porque se pueden usar los datos simulados para desarrollo y pruebas. Si desea ver exactamente lo que hace este módulo, puede ver el [código fuente del sensor de temperatura simulado](https://github.com/Azure/iotedge/blob/027a509549a248647ed41ca7fe1dc508771c8123/edge-modules/SimulatedTemperatureSensor/src/Program.cs).

Para implementar el primer módulo desde Azure Marketplace, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.

1. En el menú del panel izquierdo, en **Administración automática de dispositivos**, seleccione **IoT Edge**.

1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.

1. En la barra superior, seleccione **Establecer módulos**.

   ![Seleccionar Establecer módulos en la página de detalles del dispositivo](./media/iot-edge-deploy-module/select-set-modules.png)

1. En la sección **Módulos de IoT Edge**, de la página, haga clic en **Agregar** y seleccione **Módulo de Marketplace** en el menú desplegable.

   ![Agregar módulo de Marketplace](./media/iot-edge-deploy-module/add-marketplace-module.png)

1. En el **Marketplace del módulo de IoT Edge**, busque "sensor de temperatura simulado" y seleccione ese módulo.

1. Tenga en cuenta que el módulo SimulatedTemperatureSensor se agrega a la sección Módulos de IoT Edge, con el estado deseado **en ejecución**.

   Seleccione **Siguiente: Rutas** para continuar con el paso siguiente del asistente.

   ![Ir al siguiente paso una vez que aparezca el módulo del sensor de temperatura](./media/iot-edge-deploy-module/view-temperature-sensor-next-routes.png)

1. En la pestaña **Rutas** del asistente, puede definir cómo se pasan los mensajes entre los módulos e IoT Hub. Las rutas se construyen mediante pares de nombre-valor. Debería ver dos rutas en esta página. La ruta predeterminada denominada **route** envía todos los mensajes a IoT Hub (que se denomina `$upstream`). Una segunda ruta llamada **SimulatedTemperatureSensorToIoTHub** se creó automáticamente al agregar el módulo desde Marketplace. Esta ruta envía todos los mensajes específicamente desde el módulo de temperatura simulado a IoT Hub. Puede eliminar la ruta predeterminada porque es redundante en este caso.

   Seleccione **Siguiente: Revisar y crear** para continuar al próximo paso del asistente.

   ![Eliminar la ruta predeterminada e ir al paso siguiente](./media/iot-edge-deploy-module/delete-route-next-review-create.png)

1. En la pestaña **Revisar y crear** del asistente, puede obtener una vista previa del archivo JSON que define todos los módulos que se implementarán en el dispositivo IoT Edge. Observe que se incluye el módulo **SimulatedTemperatureSensor**, así como los dos módulos en tiempo de ejecución, **edgeAgent** y **edgeHub**. Seleccione **Crear** cuando haya terminado la revisión.

   Cuando se envía una implementación nueva a un dispositivo IoT Edge, no se inserta nada en el dispositivo. En lugar de eso, el dispositivo consulta a IoT Hub de manera periódica para comprobar cualquier instrucción nueva. Si el dispositivo encuentra un manifiesto de implementación actualizado, usa la información sobre la nueva implementación para extraer las imágenes del módulo de la nube y, después, comienza a ejecutar localmente los módulos. Este proceso puede tardar unos minutos.

1. Después de crear los detalles de la implementación del módulo, el asistente lo lleva nuevamente a la página de detalles del dispositivo. En la página de detalles del dispositivo, vea el estado de implementación en la pestaña **Módulos**. Deben aparecer tres módulos: $edgeAgent, $edgeHub y SimulatedTemperatureSensor. Si uno o varios de los módulos aparecen como especificados en la implementación pero no informados por el dispositivo, significa que el dispositivo IoT Edge todavía los está iniciando. Espere unos instantes y seleccione **Actualizar** en la parte superior de la página.

   ![Visualización de SimulatedTemperatureSensor en la lista de módulos implementados](./media/iot-edge-deploy-module/view-deployed-modules.png)
