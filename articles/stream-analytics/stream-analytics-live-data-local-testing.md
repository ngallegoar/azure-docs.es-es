---
title: Prueba de datos en vivo con Azure Stream Analytics para Visual Studio
description: Aprenda a probar los trabajos de Azure Stream Analytics localmente mediante datos de streaming en vivo.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 0788d8b4f659364fcedc690185c9159a24434125
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93123921"
---
# <a name="test-live-data-locally-using-azure-stream-analytics-tools-for-visual-studio-preview"></a>Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio (versión preliminar)

Las herramientas de Azure Stream Analytics para Visual Studio le permiten probar los trabajos localmente desde IDE mediante secuencias de eventos en vivo de Azure Event Hub, IoT Hub y Blob Storage. Las pruebas locales de datos en vivo no se pueden reemplazar por las [pruebas de escalabilidad y rendimiento](stream-analytics-streaming-unit-consumption.md) que se realizan en la nube, pero puede ahorrar tiempo durante las pruebas funcionales al no tener que enviar a la nube su trabajo de Stream Analytics cada vez que quiera probarlo. Esta característica se encuentra en versión preliminar y no debe usarse para cargas de trabajo de producción.

## <a name="testing-options"></a>Opciones de pruebas

Se admiten las siguientes opciones de pruebas locales:

|**Entrada**  |**Salida**  |**Tipo de trabajo**  |
|---------|---------|---------|
|Datos estáticos locales   |  Datos estáticos locales   |   Nube/perimetral |
|Datos de entrada en vivo   |  Datos estáticos locales   |   Nube |
|Datos de entrada en vivo   |  Datos de salida en vivo   |   Nube |

## <a name="local-testing-with-live-data"></a>Pruebas locales con datos en vivo

1. Después de haber creado un [proyecto en la nube de Azure Stream Analytics en Visual Studio](stream-analytics-quick-create-vs.md), abra **script.asaql**. En las pruebas locales se usan de forma predeterminada entradas y salidas locales.

   ![Salida y entrada local de Azure Stream Analytics para Visual Studio](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-local-input-output.png)

2. Para probar datos en vivo, elija **Use Cloud Input** (Usar entrada de nube) en el cuadro de lista desplegable.

   ![Entrada de la nube en vivo de Azure Stream Analytics para Visual Studio](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input.png)

3. Establezca el valor de **Start Time** (Hora de inicio) para definir cuándo el trabajo comenzará a procesar los datos de entrada. Puede que el trabajo deba leer los datos de entrada con antelación para garantizar la precisión de los resultados. La hora predeterminada se establece en 30 minutos antes de la hora actual.

   ![Hora de inicio de los datos en vivo de Azure Stream Analytics para Visual Studio](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-start-time.png)

4. Haga clic en **Run Locally** (Ejecutar localmente). Aparece una ventana de consola con el progreso de la ejecución y las métricas del trabajo. Si quiere detener el proceso, puede hacerlo manualmente. 

   ![Ventana de proceso con datos en vivo de Azure Stream Analytics para Visual Studio](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-process-window.png)

   Los resultados de salida se actualizan cada tres segundos con las primeras 500 filas de salida en la ventana de resultados de ejecución locales, y los archivos de salida se colocan en la carpeta **ASALocalRun** de la ruta de acceso del proyecto. También puede hacer clic en el botón **Open Results Folder** (Abrir carpeta de resultados) en la ventana de resultados de ejecución locales para abrir los archivos de salida.

   ![Carpeta de resultados abierta con datos en vivo de Azure Stream Analytics para Visual Studio](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-input-open-results-folder.png)

5. Si quiere transferir los resultados a los receptores de salida en la nube, elija **Output to Cloud** (Salida a la nube) desde el segundo cuadro de lista desplegable. Power BI y Azure Data Lake Storage no son receptores de salida admitidos.

   ![Salida a la nube con datos en vivo de Azure Stream Analytics para Visual Studio](./media/stream-analytics-live-data-local-testing/stream-analytics-local-testing-cloud-output.png)
 
## <a name="limitations"></a>Limitaciones

* Power BI y Azure Data Lake Storage no son receptores de salida admitidos debido a las limitaciones del modelo de autenticación.

* Solo las opciones de entrada de nube tienen compatibilidad con [directivas de tiempo](./stream-analytics-time-handling.md), lo que no ocurre con las opciones de entrada locales.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un trabajo de Stream Analytics mediante las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md)
* [Prueba de las consultas de Stream Analytics localmente con Visual Studio](stream-analytics-vs-tools-local-run.md)
* [Uso de Visual Studio para ver trabajos de Azure Stream Analytics](stream-analytics-vs-tools.md)