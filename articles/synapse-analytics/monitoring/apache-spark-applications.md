---
title: Supervisión de aplicaciones Apache Spark con Synapse Studio
description: Use Synapse Studio para supervisar sus aplicaciones de Apache Spark.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 16b37258d922db59f520d4e30c45773f6d1108cf
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341139"
---
# <a name="use-synapse-studio-to-monitor-your-apache-spark-applications"></a>Uso de Synapse Studio para supervisar sus aplicaciones de Apache Spark

Con Azure Synapse Analytics, puede usar Apache Spark para ejecutar cuadernos, trabajos y otros tipos de aplicaciones en los grupos de Apache Spark en su área de trabajo.

En este artículo se explica cómo supervisar las aplicaciones de Apache Spark, lo que le permite vigilar el estado más reciente, los problemas y el progreso.

En este tutorial se describen las tareas siguientes:

* Supervisión de la aplicación de Apache Spark en ejecución
* Visualización de la aplicación de Apache Spark completada
* Visualización de la aplicación de Apache Spark cancelada
* Depuración de la aplicación de Apache Spark con errores

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar este tutorial, asegúrese de que se cumplen los requisitos siguientes:

- Un área de trabajo de Synapse Studio. Para instrucciones, consulte [Creación de un área de trabajo de Synapse Studio](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace#create-a-workspace).

- Un grupo de Apache Spark.

## <a name="view-apache-spark-applications"></a>Visualización de aplicaciones de Apache Spark 
Puede ver todas las aplicaciones de Apache Spark en **Supervisión** -> **Apache Spark applications** (Aplicaciones de Apache Spark).
   ![Aplicaciones de Apache Spark](./media/how-to-monitor-spark-applications/apache-spark-applications.png)

## <a name="view-completed-apache-spark-application"></a>Visualización de la aplicación de Apache Spark completada

Abra **Supervisar** y, a continuación, seleccione la opción **Apache Spark applications** (Aplicaciones de Apache Spark). Para ver los detalles de las aplicaciones de Apache Spark completadas, seleccione la aplicación de Apache Spark y vea los detalles.

  ![Seleccionar el trabajo completado](./media/how-to-monitor-spark-applications/select-completed-job.png)

1. Compruebe los valores de **Tareas completadas** , **Estado** y **Duración total**.

2. Actualice la consulta del registro.

3. Haga clic en **Servidor de historial de Spark** para abrir el vínculo del servidor de historial de Apache Spark.

4. Compruebe la información de **Resumen**.

5. Compruebe los **Registros**. Puede seleccionar diferentes tipos de registros en la lista desplegable, y puede descargar la información de registro haciendo clic en **Download logs** (Descargar registros) y activar la casilla **Filter errors and warnings** (Filtrar errores y advertencias) para filtrar los errores y las advertencias que necesita.

6. Puede ver la información general del trabajo en el gráfico generado. De forma predeterminada, el gráfico muestra todos los trabajos. Puede filtrar esta vista por **Id. de trabajo**.

7. De forma predeterminada, está seleccionada la opción para mostrar **Progreso**. Puede comprobar el flujo de datos seleccionando **Progress**/**Read**/**Written**/**Duration** (Progreso > Leídos > Escritos > Duración) en la lista desplegable **Display** (Mostrar).

8. Para reproducir el trabajo, haga clic en botón **Playback** (Reproducir). Puede hacer clic en el botón **Stop** (Detener) en cualquier momento para detener la operación.

9. Use la barra de desplazamiento o desplácese con el mouse para acercar y alejar el gráfico de trabajos; puede seleccionar **Zoom to Fit** (Zoom para ajustar) para ajustarlo a la pantalla.

10. El nodo de gráfico de trabajo muestra la siguiente información de cada fase:

    * Id.

    * Nombre o descripción.

    * Número de tarea total.

    * Datos leídos: la suma del tamaño de entrada y el tamaño de lectura aleatorio.

    * Datos escritos: suma del tamaño de salida y el tamaño de escrituras aleatorio.

    * Tiempo de ejecución: el tiempo entre la hora de inicio del primer intento y la hora de finalización del último intento.

    * Recuento de filas: la suma de los registros de entrada, los registros de salida, los registros de lectura aleatoria y los registros de escritura aleatoria.

    * Progreso.

     ![Ver el trabajo completado](./media/how-to-monitor-spark-applications/view-completed-job.png)
    
11. Haga clic en **View details** (Ver detalles) en el gráfico y se mostrarán los detalles de la fase.

    ![Detalles de la fase](./media/how-to-monitor-spark-applications/details-for-stage.png)


## <a name="monitor-running-apache-spark-application"></a>Supervisión de la aplicación de Apache Spark en ejecución

Abra **Supervisar** y, a continuación, seleccione la opción **Apache Spark applications** (Aplicaciones de Apache Spark). Para ver los detalles sobre las aplicaciones de Apache Spark en ejecución, seleccione la aplicación de Apache Spark de envío y consulte los detalles. Si la aplicación de Apache Spark todavía está en ejecución, puede supervisar el progreso.

   ![Seleccionar un trabajo en ejecución](./media/how-to-monitor-spark-applications/select-running-job.png)

1. Compruebe los valores de **Tareas completadas** , **Estado** y **Duración total**.

2. Haga clic en **Cancel** (Cancelar) para cancelar la aplicación de Apache Spark.

3. Haga clic en **Refresh** (Actualizar) para actualizar la consulta del registro.

4. Haga clic en el botón **Spark UI** (IU de Spark) para ir a la página del trabajo de Spark.

5. Vea el gráfico. Puede ver la información general del trabajo en el gráfico generado. Consulte los pasos 6, 7, 8, 9 y 10 de [Visualización de la aplicación de Apache Spark completada](#view-completed-apache-spark-application).

6. Compruebe la información de **Resumen**.

7. Compruebe los diagnósticos de la pestaña **Diagnostic** (Diagnóstico).

8. En esta pestaña, compruebe los **registros**. Puede seleccionar diferentes tipos de registros en la lista desplegable, y puede descargar la información de registro haciendo clic en **Download logs** (Descargar registros) y activar la casilla **Filter errors and warnings** (Filtrar errores y advertencias) para filtrar los errores y las advertencias que necesita.

    ![Ver el trabajo en ejecución](./media/how-to-monitor-spark-applications/view-running-job.png)

## <a name="view-canceled-apache-spark-application"></a>Visualización de la aplicación de Apache Spark cancelada

Abra **Supervisar** y, a continuación, seleccione la opción **Apache Spark applications** (Aplicaciones de Apache Spark). Para ver los detalles de las aplicaciones de Apache Spark canceladas, seleccione la aplicación de Apache Spark y vea los detalles.

 ![Seleccionar el trabajo cancelado](./media/how-to-monitor-spark-applications/select-cancelled-job.png) 

1. Compruebe los valores de **Tareas completadas** , **Estado** y **Duración total**.

2. Actualice la consulta del registro.

3. Haga clic en Haga clic en **Servidor de historial de Spark** para abrir el vínculo del servidor de historial de Apache.

4. Vea el gráfico. Puede ver la información general del trabajo en el gráfico generado. Consulte los pasos 6, 7, 8, 9 y 10 de [Visualización de la aplicación de Apache Spark completada](#view-completed-apache-spark-application).

5. Compruebe la información de **Resumen**.

6. Compruebe los **Registros**. Puede seleccionar diferentes tipos de registros en la lista desplegable y puede descargar la información de registro haciendo clic en **Download logs** (Descargar registros) y activar la casilla **Filter errors and warnings** (Filtrar errores y advertencias) para filtrar los errores y las advertencias que necesita.

7. Haga clic en **View details** (Ver detalles) en el gráfico y se mostrarán los detalles de la fase.

   ![Ver el trabajo cancelado](./media/how-to-monitor-spark-applications/view-cancelled-job.png)

## <a name="debug-failed-apache-spark-application"></a>Depuración de la aplicación de Apache Spark con errores

Abra **Supervisar** y, a continuación, seleccione la opción **Apache Spark applications** (Aplicaciones de Apache Spark). Para ver los detalles de las aplicaciones de Apache Spark con errores, seleccione la aplicación de Apache Spark y vea los detalles.

![Seleccionar el trabajo con errores](./media/how-to-monitor-spark-applications/select-failed-job.png)

1. Compruebe los valores de **Tareas completadas** , **Estado** y **Duración total**.

2. Actualice la consulta del registro.

3. Haga clic en **Servidor de historial de Spark** para abrir el vínculo del servidor de historial de Apache Spark.

4. Vea el gráfico. Puede ver la información general del trabajo en el gráfico generado. Consulte los pasos 6, 7, 8, 9 y 10 de [Visualización de la aplicación de Apache Spark completada](#view-completed-apache-spark-application)

5. Compruebe la información de **Resumen**.

6. Compruebe la información del error.

   ![Información del trabajo con errores](./media/how-to-monitor-spark-applications/failed-job-info.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión de ejecuciones de canalización, consulte el artículo [Supervisión de ejecuciones de canalización en Synapse Studio](how-to-monitor-pipeline-runs.md).  
