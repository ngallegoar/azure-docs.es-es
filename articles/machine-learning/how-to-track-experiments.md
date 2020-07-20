---
title: Registro de métricas y experimentos de ML
titleSuffix: Azure Machine Learning
description: Supervise sus experimentos de Azure ML y supervise las métricas de ejecución para mejorar el proceso de creación de modelos. Agregue el registro al script de entrenamiento y vea los resultados registrados de una ejecución.  Use run.log, Run.start_logging o ScriptRunConfig.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: how-to
ms.date: 03/12/2020
ms.custom: seodec18
ms.openlocfilehash: 426c79c19b599127e2235f61e8c917062ede3b79
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84675209"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Supervisión de métricas y ejecuciones de experimentos de Azure ML
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Realice el seguimiento de sus experimentos y supervise las métricas de ejecución para mejorar el proceso de creación del modelo. En este artículo, aprenderá a agregar código de registro a un script de entrenamiento, enviar una ejecución de experimento, supervisar dicha ejecución y comprobar los resultados en Azure Machine Learning.

> [!NOTE]
> Azure Machine Learning también puede registrar información de otros orígenes durante el entrenamiento, como las ejecuciones de Machine Learning automatizado o el contenedor de Docker que ejecuta el trabajo de entrenamiento. Estos registros no están documentados. Si encuentra problemas y se pone en contacto con el Soporte técnico de Microsoft, es posible que puedan usar estos registros durante la resolución de problemas.

> [!TIP]
> La información de este documento va destinada principalmente a aquellos científicos de datos y desarrolladores que deseen supervisar el proceso de entrenamiento del modelo. Si es administrador y está interesado en la supervisión del uso de recursos y eventos desde Azure Machine Learning, como cuotas, ejecuciones de entrenamiento completadas o implementaciones de modelos completadas, consulte [Supervisión de Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="available-metrics-to-track"></a>Métricas disponibles de las que se realiza seguimiento

Las siguientes métricas se pueden agregar a una ejecución durante el entrenamiento de un experimento. Para ver una lista más detallada de aquello de lo que puede realizar el seguimiento en una ejecución, consulte la [documentación de referencia de la clase Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py).

|Tipo| Función de Python | Notas|
|----|:----|:----|
|Valores escalares |Función:<br>`run.log(name, value, description='')`<br><br>Ejemplo:<br>run.log("accuracy", 0.95) |Registra un valor numérico o de cadena en la ejecución con el nombre especificado. Al registrar una métrica en una ejecución, esa métrica se almacena en el registro de ejecución en el experimento.  Puede registrar la misma métrica varias veces dentro de una ejecución y el resultado se considerará un vector de esa métrica.|
|Listas|Función:<br>`run.log_list(name, value, description='')`<br><br>Ejemplo:<br>run.log_list("accuracies", [0.6, 0.7, 0.87]) | Registra una lista de valores en la ejecución con el nombre especificado.|
|Row|Función:<br>`run.log_row(name, description=None, **kwargs)`<br>Ejemplo:<br>run.log_row("Y over X", x=1, y=0.4) | Al usar *log_row* se crea una métrica de varias columnas, tal y como se describe en los argumentos kwargs. Cada parámetro con nombre genera una columna con el valor especificado.  Se puede llamar una vez a *log_row* para registrar una tupla arbitraria, o varias veces en un bucle para generar una tabla completa.|
|Tabla|Función:<br>`run.log_table(name, value, description='')`<br><br>Ejemplo:<br>run.log_table("Y over X", {"x":[1, 2, 3], "y":[0.6, 0.7, 0.89]}) | Registra un objeto de diccionario en la ejecución con el nombre especificado. |
|Imágenes|Función:<br>`run.log_image(name, path=None, plot=None)`<br><br>Ejemplo:<br>`run.log_image("ROC", plot=plt)` | Registra una imagen en el registro de ejecución. Use log_image para registrar un archivo de imagen PNG o un trazado matplotlib en la ejecución.  Estas imágenes serán visibles y comparables en el registro de ejecución.|
|Etiquetar una ejecución|Función:<br>`run.tag(key, value=None)`<br><br>Ejemplo:<br>run.tag("selected", "yes") | Etiqueta la ejecución con una clave de cadena y un valor de cadena opcional.|
|Cargar archivo o directorio|Función:<br>`run.upload_file(name, path_or_stream)`<br> <br> Ejemplo:<br>run.upload_file("best_model.pkl", "./model.pkl") | Carga un archivo en el registro de ejecución. Las ejecuciones capturan automáticamente el archivo en el directorio de salida especificado, cuyo valor predeterminado es "./outputs" para la mayoría de tipos de ejecución.  Use upload_file solo cuando sea necesario cargar archivos adicionales o no se especifique un directorio de salida. Se recomienda agregar `outputs` al nombre, para que se cargue en el directorio de salida. Puede enumerar todos los archivos que están asociados con este registro de ejecución por el parámetro `run.get_file_names()` invocado.|

> [!NOTE]
> Las métricas de valores escalares, listas, filas y tablas pueden tener el tipo: float, integer o string.

## <a name="choose-a-logging-option"></a>Elección de una opción de registro

Si quiere realizar un seguimiento del experimento o supervisarlo, debe agregar código para iniciar el registro al enviar la ejecución. Las siguientes son formas de desencadenar el envío de ejecución:
* __Run.start_logging__: agrega funciones de registro al script de entrenamiento e inicia una sesión de registro interactiva en el experimento especificado. **start_logging** crea una ejecución interactiva para su uso en escenarios como los cuadernos. Todas las métricas que se registran durante la sesión se agregan al registro de ejecución en el experimento.
* __ScriptRunConfig__: agrega funciones de registro al script de entrenamiento y carga la carpeta del script completo con la ejecución.  **ScriptRunConfig** es una clase para configurar las ejecuciones del script. Con esta opción, puede agregar código de supervisión para recibir una notificación al finalizar o para obtener un widget visual para supervisar.
* __El registro del diseñador__: añada funciones de registro a una canalización de diseño de arrastrar y soltar mediante el módulo __Ejecución de script de Python__. Agregue el código Python a los experimentos del diseñador de registros. 

## <a name="set-up-the-workspace"></a>Configuración del área de trabajo
Antes de agregar el registro y enviar un experimento, debe configurar el área de trabajo.

1. Cargue el área de trabajo. Para obtener más información sobre la definición de la configuración del área de trabajo, consulte el [archivo de configuración del área de trabajo](how-to-configure-environment.md#workspace).

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=load_ws)]


## <a name="option-1-use-start_logging"></a>Opción 1: Usar start_logging

**start_logging** crea una ejecución interactiva para su uso en escenarios como los cuadernos. Todas las métricas que se registran durante la sesión se agregan al registro de ejecución en el experimento.

En el ejemplo siguiente, se entrena un modelo sklearn Ridge sencillo de forma local en un cuaderno de Jupyter local. Para más información sobre el envío de experimentos a diferentes entornos, consulte [Configuración de destinos de proceso para el entrenamiento de modelos con Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets).

### <a name="load-the-data"></a>Carga de los datos

En este ejemplo se usa el conjunto de datos de Diabetes, un conocido conjunto de datos pequeño que viene con Scikit-learn. Esta celda carga el conjunto de datos y lo divide en conjuntos de pruebas y entrenamiento aleatorio.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=load_ws)]

### <a name="add-tracking"></a>Adición de seguimiento
Agregue seguimiento del experimento mediante el SDK de Azure Machine Learning y cargue un modelo guardado en el registro de ejecución del experimento. El código siguiente agrega etiquetas y registros y carga un archivo de modelo en la ejecución del experimento.

[!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb?name=create_experiment)]

El script finaliza con ```run.complete()```, que marca la ejecución como completada.  Esta función se usa normalmente en escenarios de cuaderno interactivos.

## <a name="option-2-use-scriptrunconfig"></a>Opción 2: Usar ScriptRunConfig

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) es una clase para configurar las ejecuciones del script. Con esta opción, puede agregar código de supervisión para recibir una notificación al finalizar o para obtener un widget visual para supervisar.

En este ejemplo se amplía el modelo sklearn Ridge básico anterior. Se realiza un barrido simple de parámetros sobre los valores alfa del modelo para capturar las métricas y los modelos entrenados en las ejecuciones del experimento. El ejemplo se ejecuta localmente en un entorno administrado por el usuario. 

1. Cree un script de entrenamiento `train.py`.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train.py)]

2. El script `train.py` hace referencia a `mylib.py`, que le permite obtener la lista de valores alfa que se usarán en el modelo de Ridge.

   [!code-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/mylib.py)] 

3. Configure un entorno local administrado por el usuario.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=user_managed_env)]


4. Envíe el script ```train.py``` para ejecutarse en el entorno administrado por el usuario. Esta carpeta del script completo se envía para el entrenamiento, incluido el archivo ```mylib.py```.

   [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=src)] [!notebook-python[] (~/MachineLearningNotebooks/how-to-use-azureml/training/train-on-local/train-on-local.ipynb?name=run)]

## <a name="option-3-log-designer-experiments"></a>Opción 3: Experimentos del diseñador de registros

Use el módulo __Ejecución de script de Python__ para agregar lógica de registro a los experimentos del diseñador. Puede registrar cualquier valor mediante este flujo de trabajo, pero es especialmente útil registrar las métricas del módulo __Evaluar modelo__ para realizar un seguimiento del rendimiento del modelo en diferentes ejecuciones.

1. Conecte un modulo __Ejecución de script de Python__ a la salida del módulo __evaluar modelo__.

    ![Conecte el módulo de ejecución de script de Python a el módulo evaluar el modelo](./media/how-to-track-experiments/designer-logging-pipeline.png)

1. Pegue el código siguiente en el editor de código__Ejecución de script de Python__ para registrar el error absoluto medio para el modelo entrenado:

    ```python
    # dataframe1 contains the values from Evaluate Model
    def azureml_main(dataframe1 = None, dataframe2 = None):
        print(f'Input pandas.DataFrame #1: {dataframe1}')

        from azureml.core import Run

        run = Run.get_context()

        # Log the mean absolute error to the current run to see the metric in the module detail pane.
        run.log(name='Mean_Absolute_Error', value=dataframe1['Mean_Absolute_Error'])

        # Log the mean absolute error to the parent run to see the metric in the run details page.
        # Note: 'run.parent.log()' should not be called multiple times because of performance issues.
        # If repeated calls are necessary, cache 'run.parent' as a local variable and call 'log()' on that variable.
        run.parent.log(name='Mean_Absolute_Error', value=dataframe1['Mean_Absolute_Error'])
    
        return dataframe1,
    ```

## <a name="manage-a-run"></a>Administración de ejecuciones

En el artículo [Inicio, supervisión y cancelación de las ejecuciones de entrenamiento](how-to-manage-runs.md) se resaltan los flujos de trabajo de Azure Machine Learning específicos sobre cómo administrar sus experimentos.

## <a name="view-run-details"></a>Visualización de los detalles de ejecución

### <a name="view-activequeued-runs-from-the-browser"></a>Visualización de ejecuciones activas o en cola desde el explorador

Los destinos de proceso utilizados para entrenar los modelos son recursos compartidos. Como tal, pueden tener varias ejecuciones en cola o activas en un momento dado. Para ver las ejecuciones de un destino de proceso específico desde el explorador, siga estos pasos:

1. En [Azure Machine Learning Studio](https://ml.azure.com/), seleccione el área de trabajo y, a continuación, seleccione __Compute__ (Proceso) en el lado izquierdo de la página.

1. Seleccione __Training Clusters__ (Clústeres de entrenamiento) para mostrar una lista de los destinos de proceso usados para el entrenamiento. Después, seleccione el clúster.

    ![Selección del clúster de entrenamiento](./media/how-to-track-experiments/select-training-compute.png)

1. Seleccione __Runs__ (Ejecuciones). Se muestra la lista de ejecuciones que usan este clúster. Para ver los detalles de una ejecución específica, use el vínculo de la columna __Run__ (Ejecución). Para ver los detalles del experimento, use el vínculo de la columna __Experiment__ (Experimento).

    ![Selección de ejecuciones para el clúster de entrenamiento](./media/how-to-track-experiments/show-runs-for-compute.png)
    
    > [!TIP]
    > Una ejecución puede contener ejecuciones secundarias, por lo que un trabajo de entrenamiento puede dar lugar a varias entradas.

Una vez finalizada la ejecución, ya no se muestra en esta página. Para ver información sobre las ejecuciones completadas, visite la sección de __experimentos__ de Studio, seleccione el experimento y ejecútelo. Para más información, consulte la sección sobre las [métricas de ejecución de consultas](#queryrunmetrics).

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Supervisar la ejecución con widgets de cuadernos de Jupyter
Cuando se usa el método **ScriptRunConfig** para enviar ejecuciones, se puede ver el progreso de la ejecución con un [widget de Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py). Al igual que el envío de ejecución, el widget es asincrónico y proporciona las actualizaciones directas cada 10 a 15 segundos hasta que se completa el trabajo.

1. Vea el widget de Jupyter mientras espera a que finalice la ejecución.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Captura de pantalla del widget de cuaderno de Jupyter](./media/how-to-track-experiments/run-details-widget.png)

   También puede obtener un vínculo a la misma pantalla en el área de trabajo.

   ```python
   print(run.get_portal_url())
   ```

2. **[Para ejecuciones de aprendizaje automático automatizado]**  Para acceder a los gráficos de una ejecución anterior. Reemplace `<<experiment_name>>` por el nombre del experimento adecuado:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Widget de cuaderno de Jupyter Notebooks para aprendizaje automático automatizado](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Para ver más detalles de una canalización, haga clic en la canalización que quiere explorar en la tabla, y los gráficos se representarán en una ventana emergente desde Azure Machine Learning Studio.

### <a name="get-log-results-upon-completion"></a>Obtener los resultados de registros tras la finalización

El entrenamiento y la supervisión de modelos tienen lugar en segundo plano, de modo que pueden ejecutar otras tareas mientras espera. También puede esperar a que el modelo haya completado el entrenamiento antes de ejecutar más código. Cuando se usa **ScriptRunConfig**, se puede usar ```run.wait_for_completion(show_output = True)``` para mostrar cuándo finaliza el entrenamiento del modelo. La marca ```show_output``` le ofrece una salida detallada. 

<a id="queryrunmetrics"></a>

### <a name="query-run-metrics"></a>Métricas de ejecución de consulta

Puede ver las métricas de un modelo entrenado con ```run.get_metrics()```. Ahora puede obtener todas las métricas que se registraron en el ejemplo anterior para determinar el mejor modelo.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studio"></a>Visualización del experimento en su área de trabajo en [Azure Machine Learning Studio](https://ml.azure.com)

Cuando un experimento ha terminado de ejecutarse, puede ir al registro de ejecución del experimento grabado. Puede acceder al historial desde [Azure Machine Learning Studio](https://ml.azure.com).

Vaya a la pestaña Experimentos y seleccione el experimento. Se le dirigirá al panel de ejecución de experimentos, donde puede ver las métricas y los gráficos de seguimiento que se registran para cada ejecución. 

Puede editar la tabla de la lista de ejecución para que muestre el último valor registrado, el mínimo o el máximo para las ejecuciones. Puede seleccionar o anular la selección de varias ejecuciones en la lista y las ejecuciones seleccionadas rellenarán los gráficos con los datos. También puede agregar nuevos gráficos o editar gráficos para comparar las métricas registradas (mínimo, máximo, último o todos los valores) en varias ejecuciones. Para explorar los datos de manera más eficaz, también puede maximizar los gráficos.

:::image type="content" source="media/how-to-track-experiments/experimentation-tab.gif" alt-text="Detalles de la ejecución en Azure Machine Learning Studio":::

También puede explorar en profundidad una ejecución específica para ver sus salidas o registros, o descargar la instantánea del experimento que ha enviado para que pueda compartir la carpeta del experimento con otros usuarios.

### <a name="viewing-charts-in-run-details"></a>Visualización de gráficos en los detalles de ejecución

Hay varias maneras de usar las API de registro para registrar diferentes tipos de métricas durante una ejecución y verlas como gráficos en Azure Machine Learning Studio.

|Valor registrado|Ejemplo de código| Ver en el portal|
|----|----|----|
|Registrar una matriz de valores numéricos| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Gráfico de líneas de una sola variable|
|Registrar un único valor numérico con el mismo nombre de métrica usado repetidamente (como desde dentro de un bucle)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Gráfico de líneas de una sola variable|
|Registrar una fila con dos columnas numéricas repetidamente|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Gráfico de líneas de dos variables|
|Registrar tabla con dos columnas numéricas|`run.log_table(name='Sine Wave', value=sines)`|Gráfico de líneas de dos variables|


## <a name="example-notebooks"></a>Cuadernos de ejemplo
Los cuadernos siguientes muestran los conceptos de este artículo:
* [how-to-use-azureml/training/train-within-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/track-and-monitor-experiments/logging-api](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

Pruebe estos pasos para saber cómo usar el SDK de Azure Machine Learning para Python:

* Vea un ejemplo de cómo registrar el mejor modelo e implementarlo en el tutorial, [Entrenar un modelo de clasificación de imágenes con Azure Machine Learning](tutorial-train-models-with-aml.md).

* Aprenda a [entrenar modelos de PyTorch con Azure Machine Learning](how-to-train-pytorch.md).
