---
title: Implementación de un módulo GPU en el dispositivo Microsoft Azure Stack Edge desde Azure Marketplace | Microsoft Docs
description: En este artículo se describe cómo habilitar el proceso y preparar el dispositivo de Azure Stack Edge para el proceso a través de la interfaz de usuario local.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/25/2020
ms.author: alkohli
ms.openlocfilehash: ac9ff084476f37348b72bdb42b9b74660132b762
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254650"
---
# <a name="deploy-a-gpu-enabled-iot-module-from-azure-marketplace-on-azure-stack-edge-gpu-device"></a>Implementación de un módulo IoT habilitado para GPU desde Azure Marketplace en un dispositivo Azure Stack Edge con GPU

En este artículo se describe cómo implementar un módulo IoT Edge habilitado para la unidad de procesamiento de gráficos (GPU) desde Azure Marketplace en el dispositivo Azure Stack Edge. 

En este artículo aprenderá a:
  - Preparar Azure Stack Edge para ejecutar un módulo de GPU
  - Descargar e implementar el módulo IoT habilitado para GPU desde Azure Marketplace
  - Supervisar la salida del módulo

## <a name="about-sample-module"></a>Información sobre el módulo de ejemplo

El módulo de ejemplo de GPU de este artículo incluye código de ejemplo de pruebas comparativas de PyTorch y TensorFlow entre CPU y GPU.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que tiene:

- Tiene acceso a un dispositivo de Azure Stack Edge con un nodo habilitado para GPU. Este dispositivo se activa con un recurso en Azure. 
- Ha configurado el proceso en este dispositivo. 
    - Siga los pasos del [Tutorial: Configuración del proceso en un dispositivo de Azure Stack Edge](azure-stack-edge-gpu-deploy-configure-compute.md).
    - Asegúrese de que 
- Los siguientes recursos de desarrollo en un cliente Windows:
    - [Visual Studio Code](https://code.visualstudio.com/)  
    - [Extensión de Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).   


## <a name="get-module-from-azure-marketplace"></a>Obtención del módulo en Azure Marketplace

1. Examine todas las [aplicaciones de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps).

    ![Examen de las aplicaciones de Azure Marketplace](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/browse-apps-marketplace-1.png)

2. Busque **Getting started with GPUs** (Introducción a las GPU).

    ![Búsqueda del módulo de ejemplo de GPU](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/search-gpu-sample-module-1.png)

3. Seleccione **Obtenerla ahora**.

    ![Obtención del módulo de ejemplo](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/get-sample-module-1.png)

4. Seleccione  **Continuar** para confirmar las condiciones de uso y la directiva de privacidad del proveedor. 

    ![Obtención del módulo de ejemplo](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/terms-of-use-1.png)

5. Seleccione la suscripción que usó para implementar el dispositivo Azure Stack Edge.

    ![Seleccionar suscripción](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/select-subscription-1.png)

6. Escriba el nombre del servicio IoT Hub que creó al configurar el dispositivo Azure Stack Edge. Para encontrar el nombre de servicio IoT Hub, vaya al recurso de Azure Stack Edge asociado con el dispositivo en Azure Portal. 

    1. En las opciones de menú del panel izquierdo, vaya a **Proceso perimetral > Introducción**. 

    1. En el icono **Configurar el proceso perimetral**, seleccione **Ver configuración**. 

        ![Vista de la configuración de proceso](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/view-config-1.png)

    1. En la hoja **Configuración del proceso perimetral**:

        1. Tome nota del nombre del servicio IoT Hub que creó al configurar el proceso en el dispositivo Azure Stack Edge.
        2. Anote el nombre del dispositivo IoT Edge que se creó al configurar el proceso. Este nombre se usará en el paso posterior.

        ![Configuración del proceso perimetral](media/azure-stack-edge-gpu-deploy-sample-module/view-compute-config-1.png)

10. Elija **Implementar en un dispositivo**.

11. Escriba el nombre del dispositivo IoT Edge o seleccione  **Buscar dispositivo** para buscar entre los dispositivos registrados en el centro.

    ![Búsqueda del dispositivo](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/find-device-1.png)

12. Seleccione  **Crear** para continuar el proceso estándar de configurar un manifiesto de implementación, lo que incluye agregar otros módulos si lo desea. Los detalles del nuevo módulo, como el URI de la imagen, las opciones de creación y las propiedades deseadas están predefinidas pero se pueden cambiar.

    ![Haga clic en Crear](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/target-devices-iot-edge-module-1.png)


13. Compruebe que el módulo está implementado en su instancia de IoT Hub en Azure Portal. Seleccione el dispositivo, luego  **Establecer módulos** y el módulo debe aparecer en la sección  **Módulos IoT Edge** .

    ![Haga clic en Crear](media/azure-stack-edge-gpu-deploy-sample-module-marketplace/running-module-iotres-1.png)

## <a name="monitor-the-module"></a>Supervisión del módulo  

1. En la paleta de comandos de VS Code, ejecute **Azure IoT Hub: Select IoT Hub**.

2. Elija la suscripción y la instancia de IoT Hub que contienen el dispositivo IoT Edge que desea configurar. En este caso, seleccione la suscripción usada para implementar el dispositivo de Azure Stack Edge y seleccione el dispositivo de IoT Edge creado para el dispositivo de Azure Stack Edge. Esto sucede cuando se configura el proceso mediante Azure Portal en los pasos anteriores.

3. En el explorador de Visual Studio Code, expanda la sección Azure IoT Hub. En **Dispositivos**, debería ver el dispositivo de IoT Edge correspondiente a su dispositivo de Azure Stack Edge. 

    1. Seleccione ese dispositivo, haga clic con el botón derecho en **Iniciar la supervisión del punto de conexión de eventos integrado**.
  
        ![Iniciar supervisión](media/azure-stack-edge-gpu-deploy-sample-module/monitor-builtin-event-endpoint-1.png)  

    2. Vaya a **Dispositivos > Módulos** y debería ver el **módulo de GPU** en ejecución.

    3. El terminal de Visual Studio Code también debería mostrar los eventos de IoT Hub como salida de supervisión para el dispositivo de Azure Stack Edge.

        ![Supervisión de la salida](media/azure-stack-edge-gpu-deploy-sample-module/monitor-events-output-1.png) 

        Puede ver que el tiempo necesario para ejecutar el mismo conjunto de operaciones (5000 iteraciones de transformación de forma) por GPU es mucho menor que el de la CPU.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre cómo [configurar la GPU para usar un módulo](azure-stack-edge-j-series-configure-gpu-modules.md).
