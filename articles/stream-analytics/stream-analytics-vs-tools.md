---
title: Visualización de trabajos de Azure Stream Analytics en Visual Studio
description: Obtenga información sobre cómo ver, iniciar y detener, probar conexiones, comprobar los resultados y exportar los trabajos de Azure Stream Analytics con Visual Studio.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 07/10/2018
ms.openlocfilehash: 95abf403dfd4abd4c0533b6e8b4a2f83b33fa18b
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045559"
---
# <a name="use-visual-studio-to-view-azure-stream-analytics-jobs"></a>Uso de Visual Studio para ver trabajos de Azure Stream Analytics

Las herramientas de Azure Stream Analytics para Visual Studio hacen que sea fácil para los desarrolladores administrar sus trabajos de Stream Analytics directamente desde el IDE. Con las herramientas de Azure Stream Analytics, puede:
- [Crear trabajos](stream-analytics-quick-create-vs.md)
- Iniciar, detener y [supervisar trabajos](stream-analytics-monitor-jobs-use-vs.md)
- Comprobación de resultados de trabajos
- Exportar los trabajos existentes a un proyecto
- Probar las conexiones de entrada y salida
- [Ejecutar consultas de forma local](stream-analytics-vs-tools-local-run.md)

Aprenda a [instalar las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-tools-for-visual-studio-install.md).

## <a name="explore-the-job-view"></a>Exploración de la vista de trabajos

Puede usar la vista de trabajos para interactuar con trabajos de Azure Stream Analytics desde Visual Studio.

### <a name="open-the-job-view"></a>Abrir la vista de trabajo

1. En el **Explorador de servidores**, haga clic en **Trabajos de Stream Analytics** y seleccione **Actualizar**. El trabajo aparecerá en **Trabajos de Stream Analytics**.

    ![Lista del Explorador de servidores de Stream Analytics](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-list-jobs-01.png)

2. Expanda el nodo de trabajos y haga doble clic en el nodo **Vista de trabajos** para abrir la vista de trabajos.
    
   ![Nodo de trabajo expandido](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-job-view-01.png)

### <a name="start-and-stop-jobs"></a>Iniciar y detener trabajos

Los trabajos de Azure Stream Analytics se pueden administrar totalmente desde la vista de trabajos de Visual Studio. Use los controles para iniciar, detener o eliminar un trabajo.
    
   ![Controles de trabajos de Stream Analytics](./media/stream-analytics-vs-tools/azure-stream-analytics-job-view-controls.png)

## <a name="check-job-results"></a>Comprobación de resultados de trabajos

Las herramientas de Stream Analytics para Visual Studio admiten actualmente la vista previa de la salida para Azure Data Lake Storage y Blob Storage. Para ver el resultado, simplemente haga doble clic en el nodo de salida del diagrama de trabajos en **Job View** (Vista de trabajos) y escriba las credenciales adecuadas.

   ![Salida de blob de trabajo de Stream Analytics](./media/stream-analytics-vs-tools/stream-analytics-blob-preview.png)

## <a name="export-jobs-to-a-project"></a>Exportación de trabajos a un proyecto

Hay dos maneras de exportar un trabajo existente a un proyecto.

1. En el **Explorador de servidores**, en el nodo Trabajos de Stream Analytics, haga clic con el botón derecho en el nodo del trabajo. Seleccione **Exportar a un nuevo proyecto de Stream Analytics**.
    
   ![Exportación de trabajos a un proyecto](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-01.png)
    
    El proyecto generado aparecerá en el **Explorador de soluciones**.
    
   ![Explorador de soluciones](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-02.png)

2. En la vista de trabajos, seleccione **Generar proyecto**.
    
   ![Generación de un proyecto desde la vista de trabajos](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="test-connections"></a>Prueba de conexiones

Las conexiones de entrada y salida se pueden probar en **Job View** (Vista de trabajos) mediante la selección de una opción en la lista desplegable **Probar conexión**.

   ![Lista desplegable Probar conexión](./media/stream-analytics-vs-tools/stream-analytics-test-connection-dropdown.png)

Los resultados de **Probar conexión** se muestran en la ventana **Salida**.

   ![Resultados de Probar conexión](./media/stream-analytics-vs-tools/stream-analytics-test-connection-results.png)

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión y administración de trabajos de Azure Stream Analytics con Visual Studio](stream-analytics-monitor-jobs-use-vs.md)
* [Inicio rápido: creación de un trabajo de Stream Analytics con Visual Studio](stream-analytics-quick-create-vs.md)
* [Tutorial: implementación de un trabajo de Azure Stream Analytics con CI/CD mediante Azure Pipelines](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Integración y desarrollo continuos con las herramientas de Stream Analytics](stream-analytics-tools-for-visual-studio-cicd.md)
