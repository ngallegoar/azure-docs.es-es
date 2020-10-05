---
title: Crear y ejecutar trabajos en la aplicación de Azure IoT Central | Microsoft Docs
description: Los trabajos de Azure IoT Central permiten funcionalidades de administración de dispositivos de forma masiva, como actualizar propiedades o ejecutar un comando.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/10/2020
ms.topic: how-to
ms.openlocfilehash: ae8b830469a9b52ae68310dde2e65dcffdf4e3be
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90060822"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar trabajos en la aplicación de Azure IoT Central

Puede usar Azure IoT Central para administrar los dispositivos conectados a gran escala mediante trabajos. Los trabajos le permiten realizar actualizaciones masivas de las propiedades de los dispositivos y la nube, además de ejecutar comandos. Este artículo le mostrará cómo empezar a usar trabajos en su propia aplicación.

## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En el ejemplo siguiente se muestra cómo crear y ejecutar un trabajo para configurar el umbral ligero de un grupo de dispositivos de puerta de enlace logística. Use el Asistente para trabajos para crear y ejecutar trabajos. Puede guardar un trabajo para ejecutarlo más tarde.

1. En el panel izquierdo, seleccione **Trabajos**.

1. Seleccione **+ Nuevo trabajo**.

1. En la página **Configurar el trabajo**, escriba un nombre y una descripción para identificar el trabajo que va a crear.

1. Seleccione el grupo de dispositivos de destino al que desee que se aplique el trabajo. Puede ver el número de dispositivos a los que se aplicará la configuración del trabajo debajo de la selección de **Grupo de dispositivos**.

1. Elija **Propiedad de la nube**, **Propiedad** o **Comando** como el **Tipo de trabajo**:

    Para definir una configuración de trabajo **Propiedad**, seleccione una propiedad y establezca su nuevo valor. Para establecer la configuración de un trabajo **Comando**, elija el comando que desea ejecutar. Un trabajo de propiedad puede establecer varias propiedades.

    :::image type="content" source="media/howto-run-a-job/configure-job.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::.

    Seleccione **Guardar y salir** para agregar el trabajo a la lista de trabajos guardados en la página **Trabajos**. Más adelante puede volver a un trabajo desde la lista de trabajos guardados.

    Seleccione **Siguiente** para ir a la página **Revisar**. En la página **Revisar** se muestran los detalles de configuración del trabajo. Seleccione **Ejecutar** para enviar el trabajo.

    :::image type="content" source="media/howto-run-a-job/job-wizard-review.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::

1. Un trabajo pasa por las fases *pendiente*, *en ejecución* y *completado*. Los detalles de ejecución del trabajo contienen métricas de resultados, detalles de duración y una cuadrícula de lista de dispositivos.

    Una vez completado el trabajo, puede seleccionar **Registro de resultados** para descargar un archivo CSV de los detalles del trabajo, incluidos los dispositivos y sus valores de estado. Esta información puede ser útil para solucionar problemas.

    :::image type="content" source="media/howto-run-a-job/download-details.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::.

1. El trabajo aparece ahora en la lista **Últimos 30 días** de la página **Trabajos**. En esta página, se muestran los trabajos en ejecución actualmente y el historial de los trabajos ejecutados o guardados anteriormente.

    > [!NOTE]
    > Puede ver hasta 30 días del historial de trabajos ejecutados anteriormente.

## <a name="manage-jobs"></a>Trabajos de administración

Para detener un trabajo en ejecución, ábralo y seleccione **Detener**. El estado del trabajo cambia para reflejar que el trabajo se ha detenido. En la sección **Resumen**, se muestran los dispositivos que se han completado, con errores o aún pendientes.

:::image type="content" source="media/howto-run-a-job/manage-job.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::

Una vez que un trabajo se encuentra detenido, puede seleccionar **Continuar** para reanudar la ejecución del trabajo. El estado del trabajo cambia para reflejar que el trabajo está de nuevo en ejecución. La sección **Resumen** continúa actualizándose con el progreso más reciente.

:::image type="content" source="media/howto-run-a-job/stopped-job.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::

## <a name="copy-a-job"></a>Copia de un trabajo

Para copiar un trabajo existente, seleccione un trabajo ejecutado. Seleccione **Copiar** en la página de resultados del trabajo o en la página de detalles de los trabajos:

:::image type="content" source="media/howto-run-a-job/job-details-copy.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::

Se abrirá una copia de la configuración del trabajo para editarla y se anexará **Copia** al nombre del trabajo.

## <a name="view-job-status"></a>Ver estado del trabajo

Una vez creado un trabajo, la columna **Estado** se actualizará con el mensaje de estado más reciente del trabajo. En la tabla siguiente se muestran los valores posibles de *estado del trabajo*:

| Mensaje de estado       | Significado de estado                                          |
| -------------------- | ------------------------------------------------------- |
| Completed            | Este trabajo se ejecutó en todos los dispositivos.              |
| Con error               | Se produjo un error en este trabajo y no se ejecutó por completo en los dispositivos.  |
| Pending              | Este trabajo aún no ha comenzado a ejecutarse en los dispositivos.         |
| En ejecución              | Este trabajo se está ejecutando actualmente en los dispositivos.             |
| Detenido              | Un usuario ha detenido manualmente este trabajo.           |

El mensaje de estado va seguido de una descripción general de los dispositivos en el trabajo. En la tabla siguiente se muestran los valores posibles de *estado del dispositivo*:

| Mensaje de estado       | Significado de estado                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Correcto            | El número de dispositivos en los que se ejecutó correctamente el trabajo.       |
| Con error               | El número de dispositivos en los que no se ejecutó correctamente el trabajo.       |

Para ver el estado del trabajo y todos los dispositivos afectados, abra el trabajo. Verá uno de los mensajes de estado siguientes junto a cada nombre de dispositivo:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | El trabajo se ejecutó en este dispositivo.                                     |
| Con error               | No se pudo ejecutar el trabajo en este dispositivo. El mensaje de error muestra más información.  |
| Pending              | Aún no se ejecutó el trabajo en este dispositivo.                                   |

Para descargar un archivo CSV que incluye los detalles del trabajo y la lista de dispositivos y sus valores de estado, seleccione **Registro de resultados**.

## <a name="filter-the-device-list"></a>Filtro de la lista de dispositivos

Puede filtrar la lista de dispositivos en la página **Detalles del trabajo** seleccionando el icono de filtro. Puede filtrar por los campos **Id. de dispositivo** o **Estado**:

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::.

## <a name="customize-columns-in-the-device-list"></a>Personalización de las columnas en la lista de dispositivos

Puede agregar columnas a la lista de dispositivos; para ello, seleccione el icono de opciones de columna:

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::.

Use el cuadro de diálogo **Opciones de columna** para elegir las columnas de la lista de dispositivos. Seleccione las columnas que desea mostrar, seleccione la flecha derecha y después **Aceptar**. Para seleccionar todas las columnas disponibles, elija **Seleccionar todo**. Las columnas seleccionadas aparecen en la lista de dispositivos.

Las columnas seleccionadas se conservan en una sesión de usuario o en las sesiones de usuario que tienen acceso a la aplicación.

## <a name="rerun-jobs"></a>Repetición de la ejecución de los trabajos

Puede volver a ejecutar un trabajo que tenga dispositivos con errores. Seleccione **Volver a ejecutar en dispositivos con error**:

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Captura de pantalla que muestra las selecciones para crear un trabajo de propiedad denominado Set Light Threshold":::.

Escriba el nombre y la descripción del trabajo y, a continuación, seleccione **Volver a ejecutar el trabajo**. Se envía un nuevo trabajo para volver a intentar la acción en los dispositivos con errores.

> [!NOTE]
> No se pueden ejecutar más de cinco trabajos al mismo tiempo desde una aplicación de Azure IoT Central.
>
> Cuando se completa un trabajo y se elimina un dispositivo que se encuentra en la lista de dispositivos del trabajo, la entrada del dispositivo aparece como eliminada en el nombre del dispositivo. El vínculo de detalles del dispositivo no está disponible para el dispositivo eliminado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Administración de dispositivos](howto-manage-devices.md)
- [Versión de la plantilla de dispositivo](howto-version-device-template.md)
