---
title: Crear y ejecutar trabajos en la aplicación de Azure IoT Central | Microsoft Docs
description: Los trabajos de Azure IoT Central permiten funcionalidades de administración de dispositivos de forma masiva, como actualizar propiedades o ejecutar un comando.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 06/08/2020
ms.topic: how-to
ms.openlocfilehash: dec9abc38bc0354ef3d22994a7988bfb006f5769
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84609749"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar trabajos en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para administrar los dispositivos conectados a escala mediante trabajos. Los trabajos le permiten realizar actualizaciones masivas de las propiedades de los dispositivos y ejecutar comandos. Este artículo le mostrará cómo empezar a usar trabajos en su propia aplicación.

## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En esta sección se muestra cómo crear y ejecutar un trabajo. Se muestra cómo establecer el umbral ligero para un grupo de dispositivos de puerta de enlace logística.

1. Vaya a **Trabajos** en el panel izquierdo.

2. Seleccione **+ Nuevo** para crear un nuevo trabajo:

    ![Crear nuevo trabajo](./media/howto-run-a-job/create-new-job.png)

3. Escriba un nombre y una descripción para identificar el trabajo que va a crear.

4. Seleccione el grupo de dispositivos de destino al que desee que se aplique el trabajo. Puede ver el número de dispositivos a los que se aplicará la configuración del trabajo en la sección **Resumen**.

5. A continuación, elija **Propiedad de la nube**, **Propiedad** o **Comando** como el tipo de trabajo que se va a configurar. Para definir una configuración de trabajo **Propiedad**, seleccione una propiedad y establezca su nuevo valor. Para configurar un **Comando**, elija el comando que desea ejecutar. Un trabajo de propiedad puede establecer varias propiedades:

    ![Configurar trabajo](./media/howto-run-a-job/configure-job.png)

6. Después de crear el trabajo, elija **Ejecutar** o **Guardar**. El trabajo aparecerá ahora en la página principal **Trabajos**. En esta página, podrá ver su trabajo en ejecución actualmente y el historial de los trabajos ejecutados o guardados anteriormente. El trabajo guardado se puede abrir de nuevo en cualquier momento para continuar con la edición o para ejecutarlo:

    ![Ver trabajo](./media/howto-run-a-job/view-job.png)

    > [!NOTE]
    > Puede ver hasta 30 días del historial de trabajos ejecutados anteriormente.

7. Para ver una descripción general del trabajo, seleccione el trabajo que ver en la lista. Esta información general contiene detalles del trabajo, los dispositivos y los valores de estado del dispositivo. En esta descripción general también puede seleccionar **Descargar detalles del trabajo** para descargar un archivo CSV de los detalles del trabajo, incluidos los dispositivos y sus valores de estado. Esta información puede ser útil para solucionar problemas:

    ![Ver el estado del dispositivo](./media/howto-run-a-job/download-details.png)

## <a name="manage-jobs"></a>Trabajos de administración

Para detener uno de los trabajos en ejecución, ábralo y seleccione **Detener**. El estado del trabajo cambia para reflejar que el trabajo se ha detenido. En la sección **Resumen** se muestran los dispositivos que se han completado, con errores o aún pendientes.

Para ejecutar un trabajo que está detenido actualmente, selecciónelo y, a continuación, seleccione **Ejecutar**. El estado del trabajo cambia para reflejar que el trabajo está de nuevo en ejecución. La sección **Resumen** continúa actualizándose con el progreso más reciente.

![Manage job (Administrar trabajo)](./media/howto-run-a-job/manage-job.png)

## <a name="copy-a-job"></a>Copia de un trabajo

Para copiar uno de los trabajos existentes, selecciónelo en la página **Trabajos** y elija **Copiar**. Se abrirá una copia de la configuración del trabajo para editarla y se anexará **Copia** al nombre del trabajo. Puede guardar o ejecutar el nuevo trabajo:

![Copia de trabajo](./media/howto-run-a-job/copy-job.png)

## <a name="view-job-status"></a>Ver estado del trabajo

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

### <a name="view-the-device-status-values"></a>Ver los valores del estado del dispositivo

Para ver el estado del trabajo y todos los dispositivos afectados, abra el trabajo. Verá uno de los mensajes de estado siguientes junto a cada nombre de dispositivo:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | El trabajo ejecutado en este dispositivo.                                     |
| Con error               | No se pudo ejecutar el trabajo en este dispositivo. El mensaje de error muestra más información.  |
| Pending              | Aún no se ha ejecutado el trabajo en este dispositivo.                                   |

Seleccione **Descargar** para descargar un archivo CSV con los detalles del trabajo y la lista de dispositivos y sus valores de estado.

### <a name="filter-the-list-of-devices"></a>Filtrado de la lista de dispositivos

Puede filtrar la lista de dispositivos en la página de detalles del trabajo seleccionando el icono de filtro. Puede filtrar por los campos **Id. de dispositivo** o **Estado**:

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Filtrado de la lista de dispositivos":::

### <a name="customize-columns-in-the-device-list"></a>Personalización de las columnas en la lista de dispositivos

Puede elegir columnas adicionales para mostrarlas en la lista de dispositivos; para ello, seleccione el icono de opciones de columna:

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Opciones de columna":::

Verá un cuadro de diálogo que le permite elegir las columnas que se van a mostrar en la lista de dispositivos. Seleccione las columnas que desea mostrar, seleccione el icono de flecha derecha y después **Aceptar**. Para seleccionar todas las columnas disponibles, elija **Seleccionar todos**:

:::image type="content" source="media/howto-run-a-job/column-picker-popup.png" alt-text="Cuadro de diálogo Selector de columnas":::

Las columnas seleccionadas se muestran en la lista de dispositivos:

:::image type="content" source="media/howto-run-a-job/column-picker-column-selected.png" alt-text="Columnas seleccionadas":::

Las columnas seleccionadas se conservan en una sesión de usuario o en las sesiones de usuario que tienen acceso a la aplicación.

## <a name="rerun-jobs"></a>Repetición de la ejecución de los trabajos

Puede volver a ejecutar un trabajo que tenga dispositivos con errores. Seleccione **Volver a ejecutar**:

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Volver a ejecutar un trabajo":::

Escriba el nombre y la descripción del trabajo y, a continuación, seleccione **Volver a ejecutar el trabajo**. Se envía un nuevo trabajo para volver a intentar la acción en los dispositivos con errores:

:::image type="content" source="media/howto-run-a-job/rerun-failed.png" alt-text="Volver a ejecutar los dispositivos con errores":::

> [!NOTE]
> No se puede ejecutar más de cinco trabajos al mismo tiempo desde una aplicación de IoT Central.

> [!NOTE]
> Cuando se completa un trabajo y se elimina un dispositivo que se encuentra en la lista de dispositivos del trabajo, la entrada del dispositivo aparece como eliminada en el nombre del dispositivo y el vínculo de detalles del dispositivo no está disponible para el dispositivo eliminado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Administración de dispositivos](howto-manage-devices.md)
- [Versión de la plantilla de dispositivo](howto-version-device-template.md)
