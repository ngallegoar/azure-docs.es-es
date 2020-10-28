---
title: Visualización y administración de sensores incorporados en Azure Defender para IoT
description: En este artículo se describe cómo ver y eliminar sensores incorporados, así como descargar nuevos archivos de activación para los sensores incorporados en Azure Defender para IoT.
author: rkarlin
ms.author: rkarlin
ms.date: 10/10/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 6aec19d413e1730bc1599f6cbac1c62e9b304ecd
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094189"
---
# <a name="view-and-manage-onboarded-sensors"></a>Visualización y administración de sensores incorporados

En este artículo se describe cómo ver y eliminar sensores incorporados, así como descargar nuevos archivos de activación para los sensores incorporados.

## <a name="update-sensor-management-mode"></a>Actualización del modo de administración del sensor

Se recomienda que actualice el modo en el que se administra el sensor. Al hacerlo, debe quitar el sensor actual de la página de administración de sensores y cargar un nuevo archivo de activación.

- **Trabaje en el modo administrado en la nube en lugar del modo administrado localmente** : actualice el archivo de activación para el sensor administrado localmente con un archivo de activación correspondiente a un sensor administrado en la nube. Después de la reactivación, las detecciones del sensor se mostrarán tanto en el portal del sensor como en el de Azure Defender para IoT. Una vez que el archivo de reactivación se haya cargado correctamente, se enviará la información de las alertas de detección nuevas a Azure.

- **Trabaje en el modo administrado localmente en lugar del modo administrado en la nube** : actualice el archivo de activación de un sensor administrado en la nube con un archivo de activación correspondiente a un sensor administrado localmente. Después de la reactivación, la información de detección del sensor solo se mostrará en el sensor.

- **Asocie el sensor a una nueva instancia de IoT Hub** : es posible que quiera asociar el sensor a una nueva instancia de IoT Hub. Para ello, vuelva a registrar el sensor y cargue un nuevo archivo de activación.

**Para reactivar el sensor:**

1. Vaya a la página **Sensor Management** (Administración de sensores) de Azure Defender para IoT.

2. Seleccione el sensor para el que quiere cargar un nuevo archivo de activación.

3. Elimínelo.

4. Incorpore de nuevo el sensor desde la página **Incorporación** en el nuevo modo o con una nueva instancia de IoT Hub.

5. Descargue el archivo de activación de la página **Download activation file** (Descargar archivo de activación).

6. Inicie sesión en la consola del sensor de Azure Defender para IoT.

7. En la consola del sensor, seleccione **Configuración del sistema** y, a continuación, seleccione **Reactivación** .

   ![Captura de pantalla de la vista Reactivación](media/updates/image14.png)

8. Seleccione **Cargar** y seleccione el archivo guardado.

9. Seleccione **Activar** .
 
## <a name="delete-sensors"></a>Eliminación de sensores

Si elimina un sensor administrado en la nube, no se enviará información a instancia de IoT Hub.

Elimine los sensores administrados localmente cuando deje de trabajar con ellos.

**Para eliminar los sensores:**

1. Vaya a la página **Sensor Management** (Administración de sensores) de Azure Defender para IoT.

2. Seleccione los sensores que quiere eliminar.

3. Seleccione **Eliminar sensor** .

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las opciones de configuración, prosiga con la guía paso a paso para configurar el módulo.
> [!div class="nextstepaction"]
> [Guía paso a paso de configuración del módulo](./how-to-agent-configuration.md)
