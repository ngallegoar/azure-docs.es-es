---
title: 'Importación de un paquete de Edge a una solución de supervisión remota: Azure | Microsoft Docs'
description: En este artículo se describe cómo importar un paquete de IoT Edge a un acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/10/2018
ms.topic: conceptual
ms.openlocfilehash: 34222f396ed3c43932371aa9f64a459bb2a5dd0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "61443018"
---
# <a name="import-an-iot-edge-package-into-your-remote-monitoring-solution-accelerator"></a>Importación de un paquete de IoT Edge en el acelerador de soluciones de supervisión remota

Un manifiesto de implementación define los módulos que se deben implementar en un dispositivo IoT Edge. En este artículo se da por supuesto que un desarrollador de su organización ya ha creado el manifiesto de implementación. Para obtener información acerca de cómo un desarrollador crea un manifiesto, consulte uno de los siguientes artículos de procedimientos de IoT Edge:

- [Implementación de módulos de Azure IoT Edge desde Azure Portal](../iot-edge/how-to-deploy-modules-portal.md)
- [Implementación de módulos de Azure IoT Edge con la CLI de Azure](../iot-edge/how-to-deploy-modules-cli.md)
- [Implementación de módulos de Azure IoT Edge desde Visual Studio Code](../iot-edge/how-to-deploy-modules-vscode.md)

Un desarrollador crea y prueba un manifiesto de implementación en un entorno de desarrollo. Cuando esté listo, puede importar el manifiesto en el acelerador de soluciones de supervisión remota.

## <a name="export-a-manifest"></a>Exportación de un manifiesto

Use Azure Portal para exportar el manifiesto de implementación desde el entorno de desarrollo:

1. En Azure Portal, vaya a la instancia de IoT Hub, que se usa para desarrollar y probar los dispositivos de IoT Edge. Haga clic en **IoT Edge** y, después, **Implementaciones de IoT Edge**: ![IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/iotedge.png).

1. Haga clic en la implementación que tiene la configuración de implementación que quiere usar. La página **Detalles de la implementación** muestra: ![Detalles de la implementación de IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/deploymentdetails.png)

1. Haga clic en **Download IoT Edge manifest**  (Descargar manifiesto de IoT Edge): ![Download deployment manifest](media/iot-accelerators-remote-monitoring-import-edge-package/download.png) (Descargar manifiesto de implementación).

1. Guarde el archivo JSON como un archivo local denominado **deploymentmanifest.json**.

Ahora tiene un archivo que contiene el manifiesto de implementación. En la sección siguiente, importará este manifiesto como un paquete en la solución de supervisión remota.

## <a name="import-a-package"></a>Importación de un paquete

Siga los pasos que se muestran a continuación para importar un manifiesto de implementación de Edge como un paquete en la solución:

1. Vaya a la página **Paquetes** de la interfaz de usuario web de supervisión remota: ![Página de paquetes](media/iot-accelerators-remote-monitoring-import-edge-package/packagespage.png)

1. Haga clic en **+ Nuevo paquete**, elija **Manifiesto de Edge** como el tipo de paquete y haga clic en **Examinar** para seleccionar el archivo **deploymentmanifest.json** que guardó en la sección anterior: ![Seleccionar manifiesto](media/iot-accelerators-remote-monitoring-import-edge-package/selectmanifest.png)

1. Haga clic en **Cargar** para agregar el paquete a la solución de supervisión remota: ![Paquete cargado](media/iot-accelerators-remote-monitoring-import-edge-package/uploadedpackage.png)

Ahora ha cargado un manifiesto de implementación de IoT Edge como un paquete. En la página **Implementaciones**, puede implementar este paquete en los dispositivos de IoT Edge conectados.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a implementar módulos en un dispositivo IoT Edge desde la solución de supervisión remota, el paso siguiente es obtener más información acerca de [IoT Edge](../iot-edge/about-iot-edge.md).
