---
title: Configuración de una ejecución de entrenamiento
titleSuffix: Azure Machine Learning
description: Configure el modelo de Machine Learning en varios entornos de entrenamiento (destinos de proceso). Es fácil cambiar entre entornos de entrenamiento.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 09/28/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: 492d1370a228fc4fc80880102899c9207a514f57
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96447184"
---
# <a name="configure-and-submit-training-runs"></a>Configuración y envío de ejecuciones de entrenamiento

En este artículo, aprenderá a configurar y enviar ejecuciones de Azure Machine Learning para entrenar los modelos. Los fragmentos de código explican las partes clave de la configuración y el envío de un script de entrenamiento.  A continuación, use uno de los [cuadernos de ejemplo](#notebooks) para buscar ejemplos completos del trabajo de un extremo a otro.

Al realizar el entrenamiento, es habitual comenzar en el equipo local y, a continuación, escalar horizontalmente a un clúster basado en la nube. Con Azure Machine Learning, puede ejecutar el script en varios destinos de proceso sin tener que cambiar el script de entrenamiento.

Todo lo que debe hacer es definir el entorno de cada destino de proceso con una **configuración de ejecución de script**.  Después, cuando desee ejecutar el experimento de entrenamiento en un destino de proceso diferente, especifique la configuración de ejecución para ese proceso.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* El [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py) (>= 1.13.0)
* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md), `ws`
* Un destino de proceso, `my_compute_target`.  [Creación de un destino de proceso](how-to-create-attach-compute-studio.md) 

## <a name="whats-a-script-run-configuration"></a><a name="whats-a-run-configuration"></a>¿En qué consiste una configuración de ejecución de script?
Una configuración [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) se usa para configurar la información necesaria para enviar una ejecución de entrenamiento como parte de un experimento.

Envíe el experimento de entrenamiento mediante un objeto ScriptRunConfig.  Este objeto incluye:

* **source_directory**: el directorio de origen que contiene el script de entrenamiento.
* **script**: el script de entrenamiento que se va a ejecutar
* **compute_target**: el destino de proceso en el que se va a ejecutar
* **environment**: el entorno que se va a usar al ejecutar el script
* y algunas opciones configurables adicionales (consulte la [documentación de referencia](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) para obtener más información)

## <a name="train-your-model"></a><a id="submit"></a>Entrenamiento de un modelo

El patrón de código para enviar una ejecución de entrenamiento es el mismo para todos los tipos de destinos de proceso:

1. Creación de un experimento para su ejecución
1. Cree el entorno en el que se ejecutará el script
1. Creación de una configuración ScriptRunConfig, que especifica el destino de proceso y el entorno
1. Envío de la ejecución
1. Espere a que la ejecución se complete

También puede:

* Envíe una ejecución de HyperDrive para el [ajuste de hiperparámetros](how-to-tune-hyperparameters.md).
* Enviar un experimento mediante la [extensión de VS Code](tutorial-train-deploy-image-classification-model-vscode.md#train-the-model).

## <a name="create-an-experiment"></a>Creación de un experimento

Cree un experimento en el área de trabajo.

```python
from azureml.core import Experiment

experiment_name = 'my_experiment'
experiment = Experiment(workspace=ws, name=experiment_name)
```

## <a name="select-a-compute-target"></a>Selección de un destino de proceso

Seleccione el destino de proceso en el que se ejecutará el script de entrenamiento. Si no se especifica ningún destino de proceso en la configuración ScriptRunConfig, o bien si `compute_target='local'`, Azure ML ejecutará el script localmente. 

El código de ejemplo de este artículo da por sentado que ya ha creado un destino de proceso `my_compute_target` en la sección "Requisitos previos".

## <a name="create-an-environment"></a>Creación de un entorno
Los [entornos](concept-environments.md) de Azure Machine Learning son una encapsulación del entorno en el que se produce el entrenamiento del aprendizaje automático. Especifican los paquetes, la imagen de Docker, las variables de entorno y la configuración de software de Python en torno a los scripts de entrenamiento y puntuación. También especifican los entornos de ejecución (Python, Spark o Docker).

Puede definir su propio entorno o usar un entorno mantenido de Azure ML. Los [entornos mantenidos](./how-to-use-environments.md#use-a-curated-environment) son entornos predefinidos que están disponibles en el área de trabajo de forma predeterminada. Estos entornos están respaldados por imágenes de Docker en caché, lo que reduce el costo de preparación de la ejecución. Consulte [Entornos mantenidos de Azure Machine Learning](./resource-curated-environments.md) para obtener la lista completa de entornos mantenidos disponibles.

En el caso de un destino de proceso remoto, puede usar uno de los siguientes entornos seleccionados populares para comenzar:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
myenv = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Para obtener más información y detalles sobre los entornos, consulte [Creación y uso de entornos de software en Azure Machine Learning](how-to-use-environments.md).
  
### <a name="local-compute-target"></a><a name="local"></a>Destino de proceso local

Si el destino de proceso es su **máquina local**, usted es responsable de garantizar que todos los paquetes necesarios estén disponibles en el entorno de Python en el que se ejecutará el script.  Use `python.user_managed_dependencies` para usar el entorno de Python actual (o el de Python en la ruta de acceso que especifique).

```python
from azureml.core import Environment

myenv = Environment("user-managed-env")
myenv.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
# myenv.python.interpreter_path = '/home/johndoe/miniconda3/envs/myenv/bin/python'
```

## <a name="create-the-script-run-configuration"></a>Creación de la configuración de ejecución de script

Ahora que tiene un destino de proceso (`my_compute_target`) y un entorno (`myenv`), cree una configuración de ejecución de script que ejecute el script de entrenamiento (`train.py`) ubicado en el directorio `project_folder`:

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=project_folder,
                      script='train.py',
                      compute_target=my_compute_target,
                      environment=myenv)

# Set compute target
# Skip this if you are running on your local computer
script_run_config.run_config.target = my_compute_target
```

Si no especifica un entorno, se creará un entorno predeterminado.

Si tiene argumentos de la línea de comandos que desea pasar al script de entrenamiento, puede especificarlos a través del parámetro **`arguments`** del constructor ScriptRunConfig, p. ej., `arguments=['--arg1', arg1_val, '--arg2', arg2_val]`.

Si desea invalidar el tiempo máximo predeterminado permitido para la ejecución, puede hacerlo a través del parámetro **`max_run_duration_seconds`** . El sistema intentará cancelar automáticamente la ejecución si tarda más que este valor.

### <a name="specify-a-distributed-job-configuration"></a>Especificación de una configuración del trabajo distribuida
Si desea ejecutar un trabajo de aprendizaje distribuido, proporcione la configuración específica del trabajo distribuido al parámetro **`distributed_job_config`** . Entre los tipos de configuración admitidos se incluyen [MpiConfiguration](/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?preserve-view=true&view=azure-ml-py), [TensorflowConfiguration](/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration?preserve-view=true&view=azure-ml-py) y [PyTorchConfiguration](/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration?preserve-view=true&view=azure-ml-py). 

Para obtener más información y ejemplos sobre la ejecución de trabajos Horovod, TensorFlow y PyTorch distribuidos, consulte:

* [Entrenar modelos de TensorFlow](./how-to-train-tensorflow.md#distributed-training)
* [Entrenar modelos de PyTorch](./how-to-train-pytorch.md#distributed-training)

## <a name="submit-the-experiment"></a>Envío del experimento

```python
run = experiment.submit(config=src)
run.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> Cuando envía la ejecución de entrenamiento, se crea una instantánea del directorio que contiene los scripts de entrenamiento y se envía al destino de proceso. También se almacena como parte del experimento del área de trabajo. Si cambia los archivos y vuelve a enviar la ejecución, solo se cargarán los archivos cambiados.
>
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]
> 
> Para más información sobre las instantáneas, consulte [Instantáneas](concept-azure-machine-learning-architecture.md#snapshots).

> [!IMPORTANT]
> **Carpetas especiales**: dos carpetas, *outputs* y *logs*, reciben un tratamiento especial por parte de Azure Machine Learning. Durante el entrenamiento, si escribe archivos en las carpetas llamadas *outputs* y *logs* relativas al directorio raíz (`./outputs` y `./logs`, respectivamente), estos archivos se cargarán automáticamente en su historial de ejecución, por lo que tendrá acceso a estos una vez completada su ejecución.
>
> Para crear artefactos durante el entrenamiento (por ejemplo, archivos de modelo, puntos de control, archivos de datos o imágenes trazadas), escríbalos en la carpeta `./outputs`.
>
> De forma similar, puede escribir cualquier registro desde su ejecución de entrenamiento en la carpeta `./logs`. Para usar la [integración TensorBoard](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/tensorboard/export-run-history-to-tensorboard/export-run-history-to-tensorboard.ipynb) de Azure Machine Learning, asegúrese de escribir sus registros de TensorBoard en esta carpeta. Mientras su ejecución está en curso, podrá iniciar TensorBoard y transmitir estos registros.  Posteriormente, también podrá restaurar los registros desde cualquiera de sus ejecuciones anteriores.
>
> Por ejemplo, para descargar un archivo escrito en la carpeta *outputs* en su equipo local después de su ejecución de entrenamiento remoto: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

## <a name="git-tracking-and-integration"></a><a id="gitintegration"></a>Integración y seguimiento de Git

Cuando se inicia una ejecución de entrenamiento en la que el directorio de origen es un repositorio de GIT local, se almacena información sobre el repositorio en el historial de ejecución. Para más información, consulte [Integración de Git con Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="notebook-examples"></a><a name="notebooks"></a>Ejemplos de cuadernos

Consulte estos cuadernos para ver ejemplos de configuración de ejecuciones en diversos escenarios de aprendizaje:
* [Cursos sobre diversos destinos de proceso](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [Entrenamiento con marcos de ML](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md) utiliza un destino de proceso administrado para entrenar un modelo.
* Consulte cómo se deben entrenar los modelos con marcos de ML específicos, como [Scikit-learn](how-to-train-scikit-learn.md), [TensorFlow](how-to-train-tensorflow.md) y [PyTorch](how-to-train-pytorch.md).
* Obtenga información sobre cómo [ajustar los hiperparámetros eficazmente](how-to-tune-hyperparameters.md) para crear modelos mejores.
* Cuando tenga un modelo entrenado, aprenda [cómo y dónde implementar los modelos](how-to-deploy-and-where.md).
* Consulte la referencia del SDK de la [clase ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py).
* [Uso de Azure Machine Learning con Azure Virtual Networks](./how-to-network-security-overview.md)
