---
title: Supervise visualmente Azure Data Factory
description: Aprenda a supervisar visualmente las factorías de datos de Azure
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: fcbf48081484941fd93f209bee21eb14c2c38310
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91570134"
---
# <a name="visually-monitor-azure-data-factory"></a>Supervise visualmente Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Una vez que haya creado y publicado una canalización en Azure Data Factory, puede asociarla a un desencadenador o iniciar manualmente una ejecución ad hoc. Puede supervisar todas las ejecuciones de la canalización de forma nativa en la experiencia de usuario de Azure Data Factory. Para abrir la experiencia de supervisión, seleccione el icono **Supervisar y administrar** de la hoja de la factoría de datos de [Azure Portal](https://portal.azure.com/). Si ya está en ADF UX, haga clic en el icono **Supervisar** de la barra lateral izquierda.

Todas las ejecuciones de la factoría de datos se muestran en la zona horaria local del explorador. Si cambia de zona horaria, todos los campos de fecha y hora se ajustan al que ha seleccionado.

## <a name="monitor-pipeline-runs"></a>La supervisión de la canalización se ejecuta

La vista de supervisión predeterminada es una lista de ejecuciones de la canalización en el período de tiempo seleccionado. Se muestran las siguientes columnas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de la canalización | Nombre de la canalización |
| Acciones | Iconos que le permiten ver detalles de actividad, cancelar o volver a ejecutar la canalización |
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de la canalización (MM/DD/AAAA, HH: MM: SS a.m./p.m.) |
| Duration | Duración de ejecución (HH) |
| Desencadenado por | El nombre del desencadenador que inició la canalización |
| Status | **Error**, **Correcto**, **En curso**, **Cancelado** o **En cola** |
| Anotaciones | Etiquetas filtrables asociadas a una canalización  |
| Parámetros | Parámetros para la ejecución de la canalización (nombre/pares de valor) |
| Error | Si se ha producido un error en la canalización, el error de ejecución |
| Ejecutar identificador | Identificador de la ejecución de canalización |

![Vista de lista de supervisión de las ejecuciones de canalización](media/monitor-visually/pipeline-runs.png)

Debe seleccionar manualmente el botón **Actualizar** para actualizar la lista de ejecuciones de actividad y de la canalización. La actualización automática no se admite actualmente.

![Botón Actualizar](media/monitor-visually/refresh.png)

## <a name="monitor-activity-runs"></a>Supervisión de las ejecuciones de actividad

Para ver las ejecuciones de actividad de cada ejecución de la canalización, seleccione el icono **Ver ejecuciones de actividad** de la columna **Acciones**. La vista de lista muestra ejecuciones de actividad correspondiente a cada ejecución de canalización.

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de actividad | Nombre de la actividad dentro de la canalización |
| Tipo de actividad | Tipo de la actividad, como **Copy**, **ExecuteDataFlow** o **AzureMLExecutePipeline** |
| Acciones | Iconos que le permiten ver información de entrada JSON, información de salida JSON o experiencias de supervisión específicas de la actividad detalladas | 
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de actividad (MM/DD/AAAA, HH: MM: SS a.m./p.m.) |
| Duration | Duración de ejecución (HH) |
| Status | **Error**, **Correcto**, **En curso** o **Cancelado** |
| Integration Runtime | Instancia de Integration Runtime donde se ejecutó la actividad |
| Propiedades de usuario | Propiedades definidas por el usuario de la actividad |
| Error | Si se ha producido un error en la actividad, el error de ejecución |
| Ejecutar identificador | Identificador de la ejecución de la actividad |

![Vista de lista para supervisar las ejecuciones de actividad](media/monitor-visually/activity-runs.png)

### <a name="promote-user-properties-to-monitor"></a>Promocionar las propiedades de usuario para supervisar

Promueva cualquier propiedad de actividad de canalización como propiedad de usuario para que se convierta en una entidad que supervise. Por ejemplo, puede promover las propiedades **Origen** y **Destino** de la actividad de copia de la canalización como propiedades de usuario. Seleccione **Generar automáticamente** para generar las propiedades de usuario **Origen** y **Destino** de una actividad de copia.

![Creación de propiedades de usuario](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Puede promover un máximo de cinco propiedades de la actividad de canalización como propiedades del usuario.

Después de crear las propiedades del usuario, puede supervisarlas en las vistas de lista de supervisión. Si el origen de la actividad de copia es un nombre de tabla, puede supervisar el nombre de la tabla de origen como una columna de la vista de lista en las ejecuciones de actividad.

![Lista de ejecuciones de actividad sin propiedades de usuario](media/monitor-visually/monitor-user-properties-image2.png)

![Agregar columnas para propiedades del usuario a la lista de ejecuciones de actividad](media/monitor-visually/monitor-user-properties-image3.png)

![Lista de ejecuciones de actividad con columnas de propiedades de usuario](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="configure-the-list-view"></a>Configuración de la vista de lista

### <a name="order-and-filter"></a>Ordenar y filtrar

Alterne si las ejecuciones de la canalización van a estar en orden descendente o ascendente según la hora de inicio de la ejecución. Filtre las ejecuciones de la canalización mediante las siguientes columnas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de la canalización | Filtre por el nombre de la canalización. |
| Hora de inicio de la ejecución |  Determine el intervalo de tiempo de las ejecuciones de la canalización mostradas. Las opciones incluyen filtros rápidos para las **últimas 24 horas**, la **última semana** y los **últimos 30 días**, o bien puede seleccionar una fecha y hora personalizadas. |
| Estado de ejecución | Filtrar ejecuciones por estado: **Correcto**, **Error**, **En cola**, **Cancelado** o **En curso**. |
| Anotaciones | Filtre por etiquetas aplicadas a cada canalización |
| Ejecuciones | Filtre si desea ver canalizaciones nuevamente ejecutadas |

![Opciones para filtrar](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Agregar o quitar columnas
Haga clic con el botón derecho en el encabezado de la vista de lista y elija las columnas que desee que aparezcan en la vista de lista.

![Opciones de las columnas](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Ajustar el ancho de columna
Para aumentar y disminuir los anchos de columna en la vista de lista, mantenga el puntero sobre el encabezado de columna.

## <a name="rerun-activities-inside-a-pipeline"></a>Volver a ejecutar las actividades en una canalización

Puede volver a ejecutar las actividades en una canalización. Seleccione **Ver ejecuciones de actividad** y seleccione la actividad de la canalización a partir de la que desea volver a ejecutar la canalización.

![Ver ejecuciones de actividad](media/monitor-visually/rerun-activities-image1.png)

![Seleccionar una ejecución de actividad](media/monitor-visually/rerun-activities-image2.png)

### <a name="rerun-from-failed-activity"></a>Volver a ejecutar desde la actividad con errores

Si se produce un error en una actividad, se agota el tiempo de espera o se cancela, puede seleccionar **Volver a ejecutar desde la actividad con errores** para volver a ejecutar la canalización desde esa actividad con errores.

![Volver a ejecutar la actividad con errores](media/monitor-visually/rerun-failed-activity.png)

### <a name="view-rerun-history"></a>Ver el historial de repetición de ejecuciones

Puede ver el historial de repetición de ejecuciones de todas las ejecuciones de canalización en la vista de lista.

![Visualización del historial](media/monitor-visually/rerun-history-image1.png)

También puede ver el historial de repetición de ejecuciones de una ejecución de canalización determinada.

![Ver el historial de ejecuciones de una canalización](media/monitor-visually/rerun-history-image2.png)

## <a name="monitor-consumption"></a>Supervisión del consumo

Para ver los recursos utilizados por una ejecución de canalización, haga clic en el icono de consumo junto a la ejecución. 

![Captura de pantalla que muestra dónde puede ver los recursos consumidos por una canalización.](media/monitor-visually/monitor-consumption-1.png)

Al hacer clic en el icono, se abre un informe de consumo de recursos usados por esa canalización. 

![Supervisión del consumo](media/monitor-visually/monitor-consumption-2.png)

Puede conectar estos valores a la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/details/data-factory/) para calcular el costo de la ejecución de la canalización. Para más información sobre los precios de Azure Data Factory, consulte [Descripción de los precios](pricing-concepts.md).

> [!NOTE]
> Estos valores devueltos por la calculadora de precios son una estimación. No refleja el importe exacto en el que se le facturará Azure Data Factory 

## <a name="gantt-views"></a>Vistas de Gantt

Use las vistas de Gantt para visualizar rápidamente las canalizaciones y ejecuciones de actividad.

![Ejemplo de diagrama de Gantt](media/monitor-visually/gantt1.png)

Puede consultar la vista de Gantt en función de la canalización o el grupo, mediante las anotaciones o etiquetas que haya creado en esas canalizaciones.

![Anotaciones de diagrama de Gantt](media/monitor-visually/gantt2.png)

La longitud de la barra le informará de la duración de la canalización. Asimismo, puede seleccionar la barra para ver más detalles.

![Duración del diagrama de Gantt](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Visitas guiadas
Seleccione el icono de **información** de la esquina inferior izquierda. Después, seleccione **Visitas guiadas** para obtener instrucciones detalladas para supervisar las ejecuciones de actividad y de la canalización.

![Visitas guiadas](media/monitor-visually/guided-tours.png)

## <a name="alerts"></a>Alertas

Puede generar alertas en función de métricas admitidas en Data Factory. Seleccione **Supervisar** > **Alertas y métricas**, en la página de supervisión de Data Factory, para empezar.

![Página de supervisión de Data Factory](media/monitor-visually/start-page.png)

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Creación de alertas

1.  Seleccione **Nueva regla de alertas** para crear una nueva alerta.

    ![Botón Nueva regla de alertas](media/monitor-visually/new-alerts.png)

1.  Especifique el nombre de la regla y seleccione la gravedad de la alerta.

    ![Cuadros del nombre de la regla y de la gravedad](media/monitor-visually/name-and-severity.png)

1.  Seleccione los criterios de la alerta.

    ![Cuadro para los criterios de destino](media/monitor-visually/add-criteria-1.png)

    ![Captura de pantalla que muestra dónde se selecciona una métrica para configurar la condición de la alerta.](media/monitor-visually/add-criteria-2.png)

    ![Lista de criterios](media/monitor-visually/add-criteria-3.png)

    Puede crear alertas de varias métricas, incluidas para el tamaño/recuento de entidades de ADF, ejecuciones de actividad/canalización/desencadenador, uso de CPU/memoria/número de nodos/cola de Integration Runtime (IR), así como para ejecuciones de paquetes de SSIS y operaciones de inicio/parada de IR de SSIS.

1.  Configure la lógica de la alerta. Puede crear una alerta de la métrica seleccionada para todas las canalizaciones y actividades correspondientes. También puede seleccionar un tipo de actividad, nombre de actividad, nombre de canalización o tipo de error determinados.

    ![Opciones para configurar la lógica de alerta](media/monitor-visually/alert-logic.png)

1.  Configure las notificaciones push, por correo electrónico, SMS o voz de la alerta. Cree un grupo de acciones para las notificaciones de alerta o elija uno existente.

    ![Opciones para configurar notificaciones](media/monitor-visually/configure-notification-1.png)

    ![Opciones para agregar una notificación](media/monitor-visually/configure-notification-2.png)

1.  Cree la regla de alertas.

    ![Opciones para crear una regla de alertas](media/monitor-visually/create-alert-rule.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión y la administración de canalizaciones, consulte el artículo [Supervisión y administración de canalizaciones mediante programación](https://docs.microsoft.com/azure/data-factory/monitor-programmatically).
