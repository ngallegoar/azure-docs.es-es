---
title: 'Importación de un paquete de solución de supervisión remota: Azure | Microsoft Docs'
description: En este artículo se describe cómo importar un paquete de administración de dispositivos automática en un acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 266e31ae9865c8fb427e06e89cd755e7ff38b27f
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92073876"
---
# <a name="import-an-automatic-device-management-package-into-your-remote-monitoring-solution-accelerator"></a>Importación de un paquete de administración de dispositivos automática en el acelerador de soluciones de supervisión remota

Una configuración de administración de dispositivos automática define los cambios de configuración para implementarse en un grupo de dispositivos. En este artículo se da por supuesto que su organización ya ha creado una configuración de administración de dispositivos automática. Para obtener información sobre cómo un desarrollador crea un manifiesto, consulte uno de los siguientes artículos de procedimientos de IoT Hub:

- [Configuración y supervisión de dispositivos IoT a escala mediante Azure Portal](../iot-hub/iot-hub-automatic-device-management.md)
- [Configurar y supervisar dispositivos de IoT a escala mediante la CLI de Azure](../iot-hub/iot-hub-automatic-device-management-cli.md)

Un desarrollador crea y prueba una configuración de administración de dispositivos automática en un entorno de desarrollo. Cuando esté listo, puede importar la configuración en el acelerador de soluciones de supervisión remota.

## <a name="export-a-configuration"></a>Exportación de configuración

Use Azure Portal para exportar la configuración de administración de dispositivos automática de su entorno de desarrollo:

1. En Azure Portal, vaya a la instancia de IoT Hub que se usa para desarrollar y probar los dispositivos IoT. Haga clic en **Configuración de dispositivos IoT**:

    [![Configuración de dispositivos IoT](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-expanded.png#lightbox)

1. Haga clic en la configuración que desee utilizar. Se muestra la página **Detalles de configuración del dispositivo**:

    [![Detalles de configuración del dispositivo IoT](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-expanded.png#lightbox)
1. Haga clic en **Download configuration file** (Descargar archivo de configuración):

    [![Download configuration file (Descargar archivo de configuración)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-expanded.png#lightbox)

1. Guarde el archivo JSON como un archivo local denominado **configuration.json**.

Ahora tendrá un archivo que contiene la configuración de administración de dispositivos automática. En la sección siguiente, importará esta configuración como un paquete en la solución de supervisión remota.

## <a name="import-a-package"></a>Importación de un paquete

Siga estos pasos para importar una configuración de administración de dispositivos automática como un paquete en la solución:

1. Vaya a la página **Paquetes** de la interfaz de usuario web de supervisión remota: ![Página de paquetes](media/iot-accelerators-remote-monitoring-import-adm-package/packagepage.png)

1. Haga clic en **+ Nuevo paquete**, elija **Configuración** como el tipo de paquete y haga clic en **Examinar** para seleccionar el archivo **configuration.json** que guardó en la sección anterior:

    ![Selección de configuración](media/iot-accelerators-remote-monitoring-import-adm-package/uploadpackage.png)

1. Haga clic en **Cargar** para agregar el paquete a la solución de supervisión remota:

    ![Paquete cargado](media/iot-accelerators-remote-monitoring-import-adm-package/uploadedpackage.png)

Ya ha cargado una configuración de administración de dispositivos automática como un paquete. En la página **Implementaciones**, puede implementar este paquete en los dispositivos conectados.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya ha aprendido a crear un paquete de configuración y a importarlo en la solución de supervisión remota, el paso siguiente consiste en saber cómo [administrar dispositivos conectados a la supervisión remota de forma masiva](iot-accelerators-remote-monitoring-bulk-configuration-update.md).