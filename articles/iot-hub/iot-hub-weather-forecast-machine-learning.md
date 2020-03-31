---
title: Previsión meteorológica mediante Azure Machine Learning con datos de IoT Hub
description: Use Azure Machine Learning para predecir la posibilidad de lluvia en función de los datos de temperatura y humedad que IoT Hub recopila de un sensor.
author: robinsh
manager: philmea
keywords: Previsión meteorológica con Machine Learning
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/10/2020
ms.author: robinsh
ms.openlocfilehash: b71b86c14c55c312ef420a4d8517140fdded4072
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122228"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Pronóstico meteorológico con los datos del sensor de IoT Hub en Azure Machine Learning

![Diagrama integral](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

El aprendizaje automático es una técnica de ciencia de datos que ayuda a los equipos a aprender de los datos existentes para prever tendencias, resultados y comportamientos futuros. Azure Machine Learning es un servicio de análisis predictivo en la nube que permite crear e implementar rápidamente modelos predictivos como soluciones de análisis.

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Obtenga información sobre cómo usar Azure Machine Learning para realizar pronósticos meteorológicos, como la posibilidad de lluvia, con los datos de temperatura y humedad de Azure IoT Hub. La posibilidad de lluvia es el resultado de un modelo de pronóstico meteorológico preparado. El modelo se basa en datos históricos para predecir la posibilidad de lluvia en función de la temperatura y la humedad.

## <a name="what-you-do"></a>Qué debe hacer

- Implementar el modelo de pronóstico meteorológico como un servicio web.
- Prepare el IoT Hub para el acceso a datos mediante la adición de un grupo de consumidores.
- Crear un trabajo de Stream Analytics y configurar el trabajo para:
  - Leer los datos de temperatura y humedad de IoT Hub.
  - Llamar al servicio web para saber la posibilidad de lluvia.
  - Guardar el resultado en Azure Blob Storage.
- Usar el Explorador de Microsoft Azure Storage para consultar el pronóstico meteorológico.

## <a name="what-you-need"></a>Lo que necesita

- Completar el tutorial [Conectar el simulador en línea de Raspberry Pi a Azure IoT Hub (Node.js)](iot-hub-raspberry-pi-web-simulator-get-started.md) o uno de los tutoriales del dispositivo, por ejemplo, [Conectar Raspberry Pi a Azure IoT Hub (Node.js)](iot-hub-raspberry-pi-kit-node-get-started.md). Estos tutoriales abarcan los requisitos siguientes:
  - Una suscripción de Azure activa.
  - Un centro de Azure IoT en su suscripción.
  - Una aplicación cliente que envía mensajes a su centro de Azure IoT.
- Una cuenta de [Azure Machine Learning Studio (clásico)](https://studio.azureml.net/).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Implementación del modelo de pronóstico meteorológico como un servicio web

En esta sección obtendrá el modelo de predicción meteorológica de la biblioteca de Azure AI. A continuación, agregará un módulo de script de R al modelo para limpiar los datos de temperatura y humedad. Por último, implementará el modelo como un servicio web predictivo.

### <a name="get-the-weather-prediction-model"></a>Obtención del modelo de predicción meteorológica

En esta sección obtendrá el modelo de predicción meteorológica de Azure AI Gallery y lo abrirá en Azure Machine Learning Studio (clásico).

1. Vaya a la [página del modelo de pronóstico meteorológico](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).

   ![Apertura de la página del modelo de predicción meteorológica en Azure AI Gallery](media/iot-hub-weather-forecast-machine-learning/weather-prediction-model-in-azure-ai-gallery.png)

1. Haga clic en **Open in Studio (classic)** (Abrir en Studio [clásico]) para abrir el modelo en Microsoft Azure Machine Learning Studio (clásico).

   ![Apertura del modelo de predicción meteorológica en Azure Machine Learning Studio (clásico)](media/iot-hub-weather-forecast-machine-learning/open-ml-studio.png)

### <a name="add-an-r-script-module-to-clean-temperature-and-humidity-data"></a>Incorporación de un módulo de script de R para limpiar los datos de temperatura y humedad

Para que el modelo se comporte correctamente, los datos de temperatura y humedad deben poder convertirse en datos numéricos. En esta sección, agregará un módulo de script de R al modelo de predicción meteorológica que quita las filas que tienen valores de datos de temperatura o humedad que no se pueden convertir en valores numéricos.

1. En el lado izquierdo de la ventana de Azure Machine Learning Studio, haga clic en la flecha para expandir el panel de herramientas. Escriba "ejecutar" en el cuadro de búsqueda. Seleccione el módulo **Execute R Script** (Ejecutar script de R).

   ![Selección del módulo Execute R Script (Ejecutar script de R)](media/iot-hub-weather-forecast-machine-learning/select-r-script-module.png)

1. Arrastre el módulo **Execute R Script** (Ejecutar script de R) cerca del módulo **Clean Missing Data** (Limpiar datos que faltan) y el módulo **Execute R Script** (Ejecutar script de R) en el diagrama. Elimine la conexión entre los módulos **Clean Missing Data** (Limpiar datos que faltan) y **Execute R Script** (Ejecutar script de R) y, luego, conecte las entradas y salidas del nuevo módulo tal y como se muestra.

   ![Incorporación del módulo Execute R Script (Ejecutar script de R)](media/iot-hub-weather-forecast-machine-learning/add-r-script-module.png)

1. Seleccione el nuevo módulo **Execute R Script** (Ejecutar script de R) para abrir su ventana de propiedades. Copie y pegue el código siguiente en el cuadro **R Script** (Script de R).

   ```r
   # Map 1-based optional input ports to variables
   data <- maml.mapInputPort(1) # class: data.frame

   data$temperature <- as.numeric(as.character(data$temperature))
   data$humidity <- as.numeric(as.character(data$humidity))

   completedata <- data[complete.cases(data), ]

   maml.mapOutputPort('completedata')

   ```

   Cuando haya terminado, la ventana de propiedades debería tener un aspecto similar al siguiente:

   ![Incorporación de código al módulo Execute R Script (Ejecutar script de R)](media/iot-hub-weather-forecast-machine-learning/add-code-to-module.png)

### <a name="deploy-predictive-web-service"></a>Implementación de un servicio web predictivo

En esta sección, validará el modelo, configurará un servicio web predictivo basado en este y, luego, implementará el servicio web.

1. Haga clic en **Ejecutar** para validar los pasos del modelo. Este paso puede tardar unos minutos.

   ![Ejecución del experimento para validar los pasos](media/iot-hub-weather-forecast-machine-learning/run-experiment.png)

1. Haga clic en **CONFIGURAR SERVICIO WEB** > **Servicio web predictivo**. Se abre el diagrama del experimento predictivo.

   ![Implementación del modelo de predicción meteorológica en Azure Machine Learning Studio (clásico)](media/iot-hub-weather-forecast-machine-learning/predictive-experiment.png)

1. En el diagrama del experimento predictivo, elimine la conexión entre el módulo **Web service input** (Entrada de servicio web) y **Weather Dataset** (Conjunto de datos de tiempo) que se encuentra en la parte superior. Luego, arrastre el módulo **Web service input** (Entrada de servicio web) cerca del módulo **Score Model** (Puntuar modelo) y conéctelo como se muestra:

   ![Conexión de dos módulos en Azure Machine Learning Studio (clásico)](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)

1. Haga clic en **EJECUTAR** para validar los pasos del modelo.

1. Haga clic en **IMPLEMENTAR SERVICIO WEB** para implementar el modelo como un servicio web.

1. En el panel del modelo, descargue **Excel 2010 o el libro anterior** para **SOLICITUD/RESPUESTA**.

   > [!Note]
   > Asegúrese de descargar el **libro de Excel 2010 o anterior** aunque vaya a ejecutar una versión posterior de Excel en el equipo.

   ![Descargar Excel para el punto de conexión SOLICITUD/RESPUESTA](media/iot-hub-weather-forecast-machine-learning/download-workbook.png)

1. Abra el libro de Excel, tome nota de la **DIRECCIÓN URL DEL SERVICIO WEB** y de la **CLAVE DE ACCESO**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Creación, configuración y ejecución de un trabajo de Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Creación de un trabajo de Stream Analytics

1. En [Azure Portal](https://portal.azure.com/), haga clic en **Crear un recurso** > **Internet de las cosas** > **Trabajo de Stream Analytics**.
1. Escriba la siguiente información para el trabajo.

   **Nombre del trabajo**: el nombre del trabajo. El nombre debe ser único globalmente.

   **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   **Ubicación**: use la misma ubicación que el grupo de recursos.

   **Anclar al panel**: active esta opción para facilitar el acceso al IoT Hub desde el panel.

   ![Creación de un trabajo de Stream Analytics en Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Haga clic en **Crear**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Adición de una entrada al trabajo de Stream Analytics

1. Abra el trabajo de Stream Analytics.
1. En **Topología de trabajo**, haga clic en **Entradas**.
1. En el panel **Entradas**, haga clic en **Agregar** y, a continuación, escriba la siguiente información:

   **Alias de entrada**: el alias único para la entrada.

   **Origen**: seleccione **IoT Hub**.

   **Grupo de consumidores**: seleccione el grupo de consumidores que ha creado.

   ![Adición de una entrada al trabajo de Stream Analytics en Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Haga clic en **Crear**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Adición de una salida al trabajo de Stream Analytics

1. En **Topología de trabajo**, haga clic en **Salidas**.
1. En el panel **Salidas**, haga clic en **Agregar** y, a continuación, escriba la siguiente información:

   **Alias de salida**: el alias único para la salida.

   **Receptor**: seleccione **Blob Storage**.

   **Cuenta de almacenamiento**: la cuenta de almacenamiento para Blob Storage. Puede crear una cuenta de almacenamiento o usar una existente.

   **Contenedor**: el contenedor donde se guarda el blob. Puede crear un contenedor o usar uno existente.

   **Formato de serialización de eventos**: seleccione **CSV**.

   ![Adición de una salida al trabajo de Stream Analytics en Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Haga clic en **Crear**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Adición de una función al trabajo de Stream Analytics para llamar al servicio web implementado

1. En **Topología de trabajo**, haga clic en **Funciones** > **Agregar**.
1. Escriba la siguiente información:

   **Alias de función**: escriba `machinelearning`.

   **Tipo de función**: seleccione **Azure ML**.

   **Opción de importación**: seleccione **Importar de una suscripción distinta**.

   **Dirección URL**: escriba la DIRECCIÓN URL DEL SERVICIO WEB que anotó del libro de Excel.

   **Clave**: escriba la CLAVE DE ACCESO que anotó del libro de Excel.

   ![Adición de una función al trabajo de Stream Analytics en Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Haga clic en **Crear**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Configuración de la consulta del trabajo de Stream Analytics

1. En **Topología de trabajo**, haga clic en **Consulta**.
1. Reemplace el código existente por el código siguiente:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Reemplace `[YourInputAlias]` por el alias de entrada del trabajo.

   Reemplace `[YourOutputAlias]` por el alias de salida del trabajo.

1. Haga clic en **Save**(Guardar).

### <a name="run-the-stream-analytics-job"></a>Ejecución del trabajo de Stream Analytics

En el trabajo de Stream Analytics, haga clic en **Iniciar** > **Ahora** > **Iniciar**. Una vez que el trabajo se inicia correctamente, su estado cambia de **Detenido** a **En ejecución**.

![Ejecución del trabajo de Stream Analytics](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Uso del Explorador de Microsoft Azure Storage para consultar el pronóstico meteorológico

Ejecute la aplicación cliente para empezar a recopilar y enviar datos de temperatura y humedad a IoT Hub. Por cada mensaje que IoT Hub recibe, el trabajo de Stream Analytics llama al servicio web de pronóstico meteorológico para producir la posibilidad de lluvia. El resultado se guarda luego en Azure Blob Storage. El Explorador de Azure Storage es una herramienta que puede usar para consultar el resultado.

1. [Descargue e instale el Explorador de Microsoft Azure Storage](https://storageexplorer.com/).
1. Abra el Explorador de Azure Storage.
1. Inicie sesión en la cuenta de Azure.
1. Seleccione su suscripción.
1. Haga clic en la suscripción > **Cuentas de almacenamiento** > su cuenta de almacenamiento > **Contenedores de blob** > su contenedor.
1. Abra un archivo .csv para ver el resultado. La última columna registra la posibilidad de lluvia.

   ![Obtención del resultado del pronóstico meteorológico con Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/weather-forecast-result.png)

## <a name="summary"></a>Resumen

Ha usado Azure Machine Learning correctamente para predecir la posibilidad de lluvia en función de los datos de temperatura y humedad que IoT Hub recibe.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]