---
title: Crear y ejecutar trabajos en la aplicación de Azure IoT Central | Microsoft Docs
description: Los trabajos de Azure IoT Central permiten funcionalidades de administración de dispositivos de forma masiva, como actualizar propiedades o ejecutar un comando.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/03/2020
ms.topic: how-to
manager: peterpr
ms.openlocfilehash: c9d5a0daa364b09e45699e898511c28d4b4d92ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157762"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar trabajos en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para administrar los dispositivos conectados a escala mediante trabajos. Los trabajos le permiten realizar actualizaciones masivas de las propiedades de los dispositivos y ejecutar comandos. Este artículo le mostrará cómo empezar a usar trabajos en su propia aplicación.

## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En esta sección se muestra cómo crear y ejecutar un trabajo. Se muestra cómo establecer el umbral ligero para un grupo de dispositivos de puerta de enlace logística.

1. Vaya a **Trabajos** en el panel izquierdo.

2. Seleccione **+ Nuevo** para crear un nuevo trabajo:

    ![Crear nuevo trabajo](./media/howto-run-a-job/createnewjob.png)

3. Escriba un nombre y una descripción para identificar el trabajo que va a crear.

4. Seleccione el grupo de dispositivos de destino al que desee que se aplique el trabajo. Puede ver el número de dispositivos a los que se aplicará la configuración del trabajo en la sección **Resumen**.

5. A continuación, elija **Propiedad de la nube**, **Propiedad** o **Comando** como el tipo de trabajo que se va a configurar. Para definir una configuración de trabajo **Propiedad**, seleccione una propiedad y establezca su nuevo valor. Para configurar un **Comando**, elija el comando que desea ejecutar. Un trabajo de propiedad puede establecer varias propiedades:

    ![Configurar trabajo](./media/howto-run-a-job/configurejob.png)

6. Después de crear el trabajo, elija **Ejecutar** o **Guardar**. El trabajo aparecerá ahora en la página principal **Trabajos**. En esta página, podrá ver su trabajo en ejecución actualmente y el historial de los trabajos ejecutados o guardados anteriormente. El trabajo guardado se puede abrir de nuevo en cualquier momento para continuar con la edición o para ejecutarlo:

    ![Ver trabajo](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Puede ver hasta 30 días del historial de trabajos ejecutados anteriormente.

7. Para ver una descripción general del trabajo, seleccione el trabajo que ver en la lista. Esta información general contiene detalles del trabajo, los dispositivos y los valores de estado del dispositivo. En esta descripción general también puede seleccionar **Descargar detalles del trabajo** para descargar un archivo CSV de los detalles del trabajo, incluidos los dispositivos y sus valores de estado. Esta información puede ser útil para solucionar problemas:

    ![Ver el estado del dispositivo](./media/howto-run-a-job/downloaddetails.png)

### <a name="manage-a-job"></a>Administración de un trabajo

Para detener uno de los trabajos en ejecución, ábralo y seleccione **Detener**. El estado del trabajo cambia para reflejar que el trabajo se ha detenido. En la sección **Resumen** se muestran los dispositivos que se han completado, con errores o aún pendientes.

Para ejecutar un trabajo que está detenido actualmente, selecciónelo y, a continuación, seleccione **Ejecutar**. El estado del trabajo cambia para reflejar que el trabajo está de nuevo en ejecución. La sección **Resumen** continúa actualizándose con el progreso más reciente.

![Manage job (Administrar trabajo)](./media/howto-run-a-job/managejob.png)

## <a name="copy-a-job"></a>Copia de un trabajo

Para copiar uno de los trabajos existentes, selecciónelo en la página **Trabajos** y elija **Copiar**. Se abrirá una copia de la configuración del trabajo para editarla y se anexará **Copia** al nombre del trabajo. Puede guardar o ejecutar el nuevo trabajo:

![Copia de trabajo](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Ver el estado del trabajo

Una vez creado un trabajo, la columna **Estado** se actualizará con el mensaje de estado más reciente del trabajo. En la tabla siguiente se muestran los valores de estado posibles:

| Mensaje de estado       | Significado de estado                                          |
| -------------------- | ------------------------------------------------------- |
| Completed            | Este trabajo se ha ejecutado en todos los dispositivos.              |
| Con error               | Este trabajo tiene errores y no se ejecutó completamente en los dispositivos.  |
| Pending              | Este trabajo no ha comenzado a ejecutarse en los dispositivos aún.         |
| En ejecución              | Este trabajo se está ejecutando actualmente en los dispositivos.             |
| Detenido              | El usuario ha detenido manualmente este trabajo.           |

El mensaje de estado va seguido de una descripción general de los dispositivos en el trabajo. En la tabla siguiente se muestran los valores de estado del dispositivo posibles:

| Mensaje de estado       | Significado de estado                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Correcto            | El número de dispositivos en los que se ha ejecutado correctamente el trabajo.       |
| Con error               | El número de dispositivos en los que no se ha ejecutado correctamente el trabajo.       |

### <a name="view-the-device-status"></a>Ver el estado del dispositivo

Para ver el estado del trabajo y todos los dispositivos afectados, abra el trabajo. Seleccione **Descargar detalles del trabajo** para descargar un archivo CSV con los detalles del trabajo, incluida la lista de dispositivos y sus valores de estado. Verá uno de los mensajes de estado siguientes junto a cada nombre de dispositivo:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | Se ha ejecutado el trabajo en este dispositivo.                                     |
| Con error               | No se ha podido ejecutar el trabajo en este dispositivo. El mensaje de error muestra más información.  |
| Pending              | Aún no se ha ejecutado el trabajo en este dispositivo.                                   |

> [!NOTE]
> Si se ha eliminado un dispositivo, no se puede seleccionar. Se muestra como eliminado con el identificador de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Administración de dispositivos](howto-manage-devices.md)
- [Versión de la plantilla de dispositivo](howto-version-device-template.md)
