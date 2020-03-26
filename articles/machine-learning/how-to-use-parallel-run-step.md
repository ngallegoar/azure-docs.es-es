---
title: Ejecución de predicciones por lotes en macrodatos
titleSuffix: Azure Machine Learning
description: Aprenda a obtener inferencias por lotes de forma asincrónica en grandes cantidades de datos mediante ParallelRunStep en Azure Machine Learning. ParallelRunStep proporciona funcionalidades de procesamiento en paralelo de forma integrada y optimiza la inferencia de alto rendimiento y de tipo "fire-and-forget" (dispara y olvida) para casos de uso de macrodatos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: vaidyas
author: vaidya-s
ms.date: 01/15/2020
ms.custom: Ignite2019
ms.openlocfilehash: 3d283d1094336b928869aa281b4a640d7a62dd94
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79477194"
---
# <a name="run-batch-inference-on-large-amounts-of-data-by-using-azure-machine-learning"></a>Ejecución de la inferencia por lotes en grandes cantidades de datos mediante Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Aprenda a procesar grandes cantidades de datos de forma asincrónica y en paralelo mediante Azure Machine Learning. La funcionalidad ParallelRunStep que se describe aquí se encuentra en versión preliminar pública. Se trata de un método de alto rendimiento y alta capacidad de proceso para generar inferencias y datos para el procesamiento. Proporciona funcionalidades asincrónicas listas para su uso.

Con ParallelRunStep es sencillo escalar inferencias sin conexión a grandes clústeres de máquinas en terabytes de datos de producción, lo que aumenta la productividad y optimiza el costo.

En este artículo, obtendrá información sobre las siguientes tareas:

> * Crear un recurso de proceso remoto.
> * Escribir un script de inferencia personalizado.
> * Crear una [canalización de Machine Learning](concept-ml-pipelines.md) para registrar un modelo de clasificación de imágenes previamente entrenado basado en el conjunto de datos de [MNIST](https://publicdataset.azurewebsites.net/dataDetail/mnist/). 
> * Usar el modelo para ejecutar la inferencia por lotes en las imágenes de ejemplo disponibles en la cuenta de Azure Blob Storage. 

## <a name="prerequisites"></a>Prerrequisitos

* Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Para un inicio rápido guiado, complete el [tutorial de instalación](tutorial-1st-experiment-sdk-setup.md) si aún no tiene un área de trabajo de Azure Machine Learning o una máquina virtual de cuadernos. 

* Para administrar el entorno y las dependencias propias, consulte la [guía paso a paso](how-to-configure-environment.md) sobre la configuración del entorno propio. Ejecute `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-contrib-pipeline-steps` en su entorno para descargar las dependencias necesarias.

## <a name="set-up-machine-learning-resources"></a>Configurar los recursos de aprendizaje automático

Las siguientes acciones configuran los recursos necesarios para ejecutar una canalización de inferencias por lotes:

- Cree un almacén de datos que apunte a un contenedor de blobs que tenga imágenes para inferencia.
- Configure referencias de datos como entradas y salidas para el paso de la canalización de inferencias por lotes.
- Configure un clúster de proceso para ejecutar el paso de inferencia por lotes.

### <a name="create-a-datastore-with-sample-images"></a>Creación de un almacén de datos con imágenes de ejemplo

Obtenga el conjunto de evaluación de MNIST del contenedor de blobs público `sampledata` en una cuenta llamada `pipelinedata`. Cree un almacén de datos con el nombre `mnist_datastore` que apunte a este contenedor. En la siguiente llamada a `register_azure_blob_container`, si se establece la marca `overwrite` en `True` se sobrescribe cualquier almacén de datos que se haya creado anteriormente con ese nombre. 

Puede cambiar este paso para que apunte al contenedor de blobs si proporciona sus propios valores para `datastore_name`, `container_name` y `account_name`.

```python
from azureml.core import Datastore
from azureml.core import Workspace

# Load workspace authorization details from config.json
ws = Workspace.from_config()

mnist_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="mnist_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata",
                      overwrite=True)
```

A continuación, especifique el almacén de datos predeterminado del área de trabajo como almacén de datos de salida. Lo usará para la salida de la inferencia.

Cuando cree su área de trabajo, [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)  y [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)  se adjuntan al área de trabajo de forma predeterminada. Azure Files es el almacén de datos predeterminado para un área de trabajo, pero también puede usar Blob Storage como un almacén de datos. Para obtener más información, consulte [Opciones de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-inputs-and-outputs"></a>Configuración de las entradas y salidas de datos

Ahora debe configurar las entradas y salidas de datos, entre las que se incluyen:

- directorio que contiene las imágenes de entrada.
- El directorio en el que se almacena el modelo previamente entrenado.
- El directorio que contiene las etiquetas.
- El directorio de salida.

[`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) es una clase para explorar, transformar y administrar datos en Azure Machine Learning. Esta clase tiene dos tipos: [`TabularDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) y [`FileDataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py). En este ejemplo usará `FileDataset` como entradas para el paso de canalización de inferencias por lotes. 

> [!NOTE] 
> Por ahora, la compatibilidad con `FileDataset` en la inferencia por lotes está restringida a Azure Blob Storage. 

También puede hacer referencia a otros conjuntos de datos del script de inferencia personalizado. Por ejemplo, puede utilizarlo para acceder a las etiquetas del script para etiquetar las imágenes mediante `Dataset.register` y `Dataset.get_by_name`.

Para más información sobre los conjuntos de datos de Azure Machine Learning, consulte [Creación de conjuntos de datos y acceso a ellos (versión preliminar)](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets).

Los objetos [`PipelineData`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) se usan para transferir los datos intermedios entre los pasos de la canalización. En este ejemplo, se usa para las salidas de inferencia.

```python
from azureml.core.dataset import Dataset

mnist_ds_name = 'mnist_sample_data'

path_on_datastore = mnist_blob.path('mnist/')
input_mnist_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)
registered_mnist_ds = input_mnist_ds.register(ws, mnist_ds_name, create_new_version=True)
named_mnist_ds = registered_mnist_ds.as_named_input(mnist_ds_name)

output_dir = PipelineData(name="inferences", 
                          datastore=def_data_store, 
                          output_path_on_compute="mnist/results")
```

### <a name="set-up-a-compute-target"></a>Configuración de un destino de proceso

En Azure Machine Learning, el *proceso* (o *destino de proceso*) se refiere a las máquinas o clústeres que realizarán los pasos del cálculo en su canal de aprendizaje automático. Ejecute el código siguiente para crear un destino [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) basado en CPU.

```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpu-cluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use get_status()
    print(compute_target.get_status().serialize())
```

## <a name="prepare-the-model"></a>Preparar el modelo

[Descargue el modelo de clasificación de imágenes previamente entrenado](https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz) y extráigalo en el directorio `models`.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

Después, registre el modelo con el área de trabajo para que esté disponible para el recurso de proceso remoto.

```python
from azureml.core.model import Model

# Register the downloaded model 
model = Model.register(model_path="models/",
                       model_name="mnist",
                       tags={'pretrained': "mnist"},
                       description="Mnist trained tensorflow model",
                       workspace=ws)
```

## <a name="write-your-inference-script"></a>Escritura del script de inferencia

>[!Warning]
>El código siguiente es solo un ejemplo que utiliza el [cuaderno de ejemplo](https://aka.ms/batch-inference-notebooks). Deberá crear su propio script para el escenario.

El script *debe contener* dos funciones:
- `init()`: utilice esta función para cualquier preparación costosa o común para la inferencia posterior. Por ejemplo, para cargar el modelo en un objeto global. Solo se llamará a esta función una vez al principio del proceso.
-  `run(mini_batch)`: la función se ejecutará para cada instancia de `mini_batch`.
    -  `mini_batch`: ParallelRunStep invocará el método run y pasará una trama de datos de Pandas o una lista como argumento al método. Cada entrada de min_batch será una ruta de acceso de archivo si la entrada es FileDataset o una trama de datos de Pandas si es TabularDataset.
    -  `response`: el método run() debe devolver una trama de datos de Pandas o una matriz. Para append_row output_action, estos elementos devueltos se anexan al archivo de salida común. Para summary_only, se omite el contenido de los elementos. Para todas las acciones de salida, cada elemento de salida devuelto indica una ejecución correcta del elemento de entrada en el minilote de entrada. Debe asegurarse de que se incluyen suficientes datos en el resultado de la ejecución para asignar la entrada a este. La salida de la ejecución se escribirá en el archivo de salida y no se garantiza que esté en orden, por lo que debe usar alguna clave en la salida para asignarla a la entrada.

```python
# Snippets from a sample script.
# Refer to the accompanying digit_identification.py
# (https://aka.ms/batch-inference-notebooks)
# for the implementation script.

import os
import numpy as np
import tensorflow as tf
from PIL import Image
from azureml.core import Model


def init():
    global g_tf_sess

    # Pull down the model from the workspace
    model_path = Model.get_model_path("mnist")

    # Construct a graph to execute
    tf.reset_default_graph()
    saver = tf.train.import_meta_graph(os.path.join(model_path, 'mnist-tf.model.meta'))
    g_tf_sess = tf.Session()
    saver.restore(g_tf_sess, os.path.join(model_path, 'mnist-tf.model'))


def run(mini_batch):
    print(f'run method start: {__file__}, run({mini_batch})')
    resultList = []
    in_tensor = g_tf_sess.graph.get_tensor_by_name("network/X:0")
    output = g_tf_sess.graph.get_tensor_by_name("network/output/MatMul:0")

    for image in mini_batch:
        # Prepare each image
        data = Image.open(image)
        np_im = np.array(data).reshape((1, 784))
        # Perform inference
        inference_result = output.eval(feed_dict={in_tensor: np_im}, session=g_tf_sess)
        # Find the best probability, and add it to the result list
        best_result = np.argmax(inference_result)
        resultList.append("{}: {}".format(os.path.basename(image), best_result))

    return resultList
```

### <a name="how-to-access-other-files-in-source-directory-in-entry_script"></a>Acceso a otros archivos del directorio de origen de entry_script

Si tiene otro archivo o carpeta en el mismo directorio que el script de entrada, puede buscar el directorio de trabajo actual para hacer referencia a él.

```python
script_dir = os.path.realpath(os.path.join(__file__, '..',))
file_path = os.path.join(script_dir, "<file_name>")
```

## <a name="build-and-run-the-pipeline-containing-parallelrunstep"></a>Compilación y ejecución de la canalización que contiene ParallelRunStep

Ahora ya tiene todo lo que necesita para compilar la canalización.

### <a name="prepare-the-run-environment"></a>Preparación del entorno de ejecución

Primero, especifique las dependencias para el script. Usará este objeto más adelante cuando cree el paso de canalización.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

batch_conda_deps = CondaDependencies.create(pip_packages=["tensorflow==1.13.1", "pillow"])

batch_env = Environment(name="batch_environment")
batch_env.python.conda_dependencies = batch_conda_deps
batch_env.docker.enabled = True
batch_env.docker.base_image = DEFAULT_GPU_IMAGE
batch_env.spark.precache_packages = False
```

### <a name="specify-the-parameters-for-your-batch-inference-pipeline-step"></a>Especificación de los parámetros para el paso de canalización de inferencias por lotes

`ParallelRunConfig` es la configuración principal de la instancia de `ParallelRunStep` de inferencia por lotes recién introducida en la canalización de Azure Machine Learning. Se usa para encapsular el script y configurar los parámetros necesarios, incluidos los siguientes:
- `entry_script`: script de usuario como ruta de acceso de archivo local que se ejecutará en paralelo en varios nodos. Si `source_directory` está presente, utilice una ruta de acceso relativa. De lo contrario, use cualquier ruta de acceso accesible desde la máquina.
- `mini_batch_size`: tamaño del minilote que se pasa a una sola llamada de `run()`. (Opcional; el valor predeterminado es `10` archivos para FileDataset y `1MB` para TabularDataset).
    - En el caso de `FileDataset`, es el número de archivos con un valor mínimo de `1`. Puede combinar varios archivos en un solo minilote.
    - En el caso de `TabularDataset`, es el tamaño de los datos. Los valores posibles son `1024`, `1024KB`, `10MB` y `1GB`. `1MB` es el valor recomendado. El minilote de `TabularDataset` nunca cruzará los límites de los archivos. Por ejemplo, si tiene archivos. csv de varios tamaños, el menor es de 100 KB y el mayor es de 10 MB. Si establece `mini_batch_size = 1MB`, los archivos con un tamaño menor que 1 MB se tratarán como un solo minilote. Los archivos de tamaño mayor que 1 MB se dividirán en varios minilotes.
- `error_threshold`: número de errores de registro para `TabularDataset` y errores de archivo para `FileDataset` que se deben omitir durante el procesamiento. Si el recuento de errores de la entrada supera este valor, el trabajo se anulará. El umbral de error es para toda la entrada y no para los minilotes individuales que se envían al método `run()`. El intervalo es `[-1, int.max]`. La parte `-1` indica que se omitirán todos los errores durante el procesamiento.
- `output_action`: uno de los valores siguientes indica cómo se organizará la salida:
    - `summary_only`: el script de usuario almacenará la salida. `ParallelRunStep` usará la salida solo para el cálculo del umbral de error.
    - `append_row`: en los archivos de entrada solo se creará un archivo en la carpeta de salida para anexar todas las salidas separadas por líneas. El nombre de archivo será `parallel_run_step.txt`.
- `source_directory`: rutas de acceso a las carpetas que contienen todos los archivos que se van a ejecutar en el destino de proceso (opcional).
- `compute_target`: Solo se admite `AmlCompute`.
- `node_count`: número de nodos de proceso que se usarán para ejecutar el script de usuario.
- `process_count_per_node`: número de procesos por nodo.
- `environment`: definición del entorno de Python. Puede configurarla para que use un entorno de Python existente o para un entorno temporal para el experimento. La definición también es responsable de establecer las dependencias de la aplicación necesarias (opcional).
- `logging_level`: nivel de detalle del registro. Los valores con nivel de detalle en aumento son: `WARNING`, `INFO` y `DEBUG`. (Opcional; el valor predeterminado es `INFO`).
- `run_invocation_timeout`: tiempo de espera de invocación del método `run()` en segundos. (Opcional; el valor predeterminado es `60`).

```python
from azureml.contrib.pipeline.steps import ParallelRunConfig

parallel_run_config = ParallelRunConfig(
    source_directory=scripts_folder,
    entry_script="digit_identification.py",
    mini_batch_size="5",
    error_threshold=10,
    output_action="append_row",
    environment=batch_env,
    compute_target=compute_target,
    node_count=4)
```

### <a name="create-the-pipeline-step"></a>Creación del paso de canalización

Cree el paso de canalización mediante el script, la configuración del entorno y los parámetros. Especifique el destino de proceso que ya adjuntó a su área de trabajo como destino de ejecución del script. Use `ParallelRunStep` para crear el paso de canalización de inferencias por lotes, que toma todos los parámetros siguientes:
- `name`: nombre del paso, con las siguientes restricciones de nomenclatura: unique, 3-32 caracteres y regex ^\[a-z\]([-a-z0-9]*[a-z0-9])?$.
- `models`: cero o más nombres de modelo ya registrados en el registro de modelos de Azure Machine Learning.
- `parallel_run_config`: objeto `ParallelRunConfig`, tal y como se definió anteriormente.
- `inputs`: uno o más conjuntos de datos de Azure Machine Learning de tipo único.
- `output`: objeto `PipelineData` que corresponde al directorio de salida.
- `arguments`: lista de argumentos pasados al script de usuario (opcional).
- `allow_reuse`: sirve para decidir si el paso debe volver a usar los resultados anteriores cuando se ejecuta con la misma configuración o entrada. Si este parámetro es `False`, siempre se generará una nueva ejecución para este paso durante la ejecución de la canalización. (Opcional; el valor predeterminado es `True`).

```python
from azureml.contrib.pipeline.steps import ParallelRunStep

parallelrun_step = ParallelRunStep(
    name="batch-mnist",
    models=[model],
    parallel_run_config=parallel_run_config,
    inputs=[named_mnist_ds],
    output=output_dir,
    arguments=[],
    allow_reuse=True
)
```

>[!Note]
> El paso anterior depende de `azureml-contrib-pipeline-steps`, como se describe en [Requisitos previos](#prerequisites). 

### <a name="submit-the-pipeline"></a>Enviar la canalización

Ahora ejecute la canalización. En primer lugar, cree un objeto [`Pipeline`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) con la referencia al área de trabajo y el paso de canalización que creó. El parámetro `steps` es una matriz de pasos. En este caso, la puntuación por lotes consta de un solo paso. Para compilar canalizaciones con varios pasos, colóquelos en orden en esta matriz.

Luego, use la función `Experiment.submit()` para enviar la canalización para su ejecución.

```python
from azureml.pipeline.core import Pipeline
from azureml.core.experiment import Experiment

pipeline = Pipeline(workspace=ws, steps=[parallelrun_step])
pipeline_run = Experiment(ws, 'digit_identification').submit(pipeline)
```

## <a name="monitor-the-parallel-run-job"></a>Supervisión del trabajo de ejecución en paralelo

Un trabajo de inferencias por lotes puede tardar mucho tiempo. En este ejemplo se supervisa el progreso mediante un widget de Jupyter. También puede administrar el progreso del trabajo mediante:

* Azure Machine Learning Studio. 
* La salida de la consola del objeto [`PipelineRun`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.run.pipelinerun?view=azure-ml-py).

```python
from azureml.widgets import RunDetails
RunDetails(pipeline_run).show()

pipeline_run.wait_for_completion(show_output=True)
```

## <a name="next-steps"></a>Pasos siguientes

Para ver cómo funciona esto de un extremo a otro, pruebe el [cuaderno de inferencias por lotes](https://aka.ms/batch-inference-notebooks). 

Para instrucciones sobre depuración y solución de problemas de ParallelRunStep, consulte la [guía de procedimientos](how-to-debug-parallel-run-step.md).

Para instrucciones sobre depuración y solución de problemas para las canalizaciones, consulte la [guía paso a paso](how-to-debug-pipelines.md).

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

