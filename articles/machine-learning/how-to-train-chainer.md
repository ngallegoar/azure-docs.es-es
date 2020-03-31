---
title: Entrenamiento de modelos de Chainer de aprendizaje profundo
titleSuffix: Azure Machine Learning
description: Aprenda a ejecutar los scripts de entrenamiento de PyTorch a escala empresarial mediante la clase estimator de Chainer de Azure Machine Learning.  El script de ejemplo clasifica las imágenes de dígitos escritos a mano para crear una red neuronal de aprendizaje profundo con la biblioteca Chainer de Python que se ejecuta sobre numpy.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 08/02/2019
ms.openlocfilehash: e2840a6295140e0dc22a032fa844c0488403c5a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75534120"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning"></a>Entrenamiento y registro de modelos de Chainer a escala con Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo aprenderá a ejecutar los scripts de entrenamiento de [Chainer](https://chainer.org/) a escala empresarial mediante la clase [estimator de Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) de Azure Machine Learning. En el script de entrenamiento de ejemplo de este artículo se usa el conocido [conjunto de datos de MNIST](http://yann.lecun.com/exdb/mnist/) para clasificar los dígitos escritos a mano mediante una red neuronal profunda (DNN) creada con la biblioteca Chainer de Python que se ejecuta sobre [numpy](https://www.numpy.org/).

Tanto si va a entrenar un modelo de Chainer de aprendizaje profundo desde el principio como si va a incorporar un modelo existente a la nube, puede usar Azure Machine Learning para escalar horizontalmente trabajos de entrenamiento de código abierto mediante recursos de proceso en la nube elástica. Puede compilar, implementar y supervisar modelos de nivel de producción, así como crear versiones de dichos mismos, mediante Azure Machine Learning. 

Más información sobre las diferencias entre [aprendizaje profundo y aprendizaje automático](concept-deep-learning-vs-machine-learning.md).

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Prerrequisitos

Ejecute este código en cualquiera de estos entornos:

- Instancia de Proceso de Azure Machine Learning: no se necesitan descargas ni instalación

    - Complete el [Tutorial: Configuración del entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md) para crear un servidor de cuadernos dedicado en el que se habrán cargado previamente el SDK y el repositorio de ejemplos.
    - En la carpeta de aprendizaje profundo de ejemplos en el servidor de cuadernos, busque un cuaderno completado y archivos en la carpeta **how-to-use-azureml > ml-frameworks > chainer > deployment > train-hyperparameter-tune-deploy-with-chainer**.  El cuaderno incluye secciones expandidas que abarcan el ajuste de hiperparámetros inteligente, la implementación de modelos y los widgets del cuaderno.

- Su propio servidor de Jupyter Notebook

    - [Instalación del SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
    - [Cree un archivo de configuración del área de trabajo](how-to-configure-environment.md#workspace).
    - Descarga del archivo de script de ejemplo [chainer_mnist.py](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer).
     - También puede encontrar una [versión de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) completada de esta guía en la página de ejemplos de GitHub. El cuaderno incluye secciones expandidas que abarcan el ajuste de hiperparámetros inteligente, la implementación de modelos y los widgets del cuaderno.

## <a name="set-up-the-experiment"></a>Configuración del experimento

En esta sección, para configurar el experimento de entrenamiento, se cargan los paquetes de Python necesarios, se inicializa un área de trabajo, se crea un experimento y se cargan los datos y scripts de entrenamiento.

### <a name="import-packages"></a>Importación de paquetes

En primer lugar, importe la biblioteca azureml.core de Python y muestre el número de versión.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Inicialización de un área de trabajo

El [área de trabajo de Azure Machine Learning](concept-workspace.md) es el recurso de nivel superior para el servicio. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree. En el SDK de Python, puede acceder a los artefactos del área de trabajo mediante la creación de un objeto [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).

Cree un objeto de área de trabajo mediante la lectura del archivo `config.json` creado en la [sección de requisitos previos](#prerequisites):

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Creación de un directorio del proyecto
Cree un directorio que contendrá todo el código necesario del equipo local al que necesitará acceder desde el recurso remoto. Esto incluye el script de entrenamiento y los archivos adicionales de los que dependa el script de entrenamiento.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Preparación del script de entrenamiento

En este tutorial, el script de entrenamiento **chainer_mnist.py** ya se proporciona. En la práctica, debería poder usar cualquier script de entrenamiento personalizado tal cual y ejecutarlo con Azure Machine Learning sin tener que modificar el código.

Para usar las funcionalidades de seguimiento y métricas de Azure ML, agregue una pequeña cantidad de código de Azure ML dentro del script de entrenamiento.  El script de entrenamiento **chainer_mnist.py** muestra cómo registrar algunas métricas para la ejecución de Azure Machine Learning mediante el objeto `Run` en el script.

El script de entrenamiento proporcionado usa datos de ejemplo de la función `datasets.mnist.get_mnist` de Chainer.  Para sus propios datos, puede que necesite realizar pasos como [cargar el conjunto de datos y los scripts](how-to-train-keras.md#data-upload) para que los datos estén disponibles durante el entrenamiento.

Copie el script de entrenamiento **chainer_mnist.py** en el directorio del proyecto.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-a-deep-learning-experiment"></a>Creación de un experimento de aprendizaje profundo

Cree un experimento. En este ejemplo, cree un experimento denominado "chainer-mnist".

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Creación u obtención de un destino de proceso

Para entrenar el modelo, necesita un [destino de proceso](concept-compute-target.md). En este ejemplo, usará el proceso administrado de Azure Machine Learning (AmlCompute) para el recurso de proceso de entrenamiento remoto.

**La creación de AmlCompute tarda aproximadamente 5 minutos**. Si el AmlCompute con ese nombre ya está en el área de trabajo, este código omite el proceso de creación.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

Para más información sobre los destinos de proceso, vea el artículo [¿Qué es un destino de proceso?](concept-compute-target.md)

## <a name="create-a-chainer-estimator"></a>Creación de un estimador de Chainer

El [estimador de Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) proporciona una manera sencilla de iniciar trabajos de entrenamiento de Chainer en el destino de proceso.

El estimador de Chainer se implementa a través de la clase genérica [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py), que se puede usar para admitir cualquier marco de trabajo. Para obtener más información sobre cómo entrenar modelos con el estimador genérico, consulte [Entrenamiento de modelos con Azure Machine Learning mediante un objeto Estimator](how-to-train-ml-models.md).

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Envío de una ejecución

El [objeto Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) proporciona la interfaz para el historial de ejecución mientras se ejecuta el trabajo y cuando se ha completado.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Durante la ejecución de Run, pasa por las fases siguientes:

- **Preparando**: se crea una imagen de Docker según el estimador de Chainer. La imagen se carga en el registro de contenedor del área de trabajo y se almacena en memoria caché para ejecuciones posteriores. Los registros también se transmiten al historial de ejecución y se pueden consultar para supervisar el progreso.

- **Escalado**: el clúster intenta escalar verticalmente si el clúster de Batch AI requiere más nodos para realizar la ejecución de los que se encuentran disponibles actualmente.

- **En ejecución**: todos los scripts de la carpeta de scripts se cargan en el destino de proceso, se montan o se copian los almacenes de datos y se ejecuta entry_script. Las salidas de stdout y la carpeta ./logs se transmiten al historial de ejecución y se pueden usar para supervisar la ejecución.

- **Posprocesamiento**: la carpeta ./outputs de la ejecución se copia en el historial de ejecución.

## <a name="save-and-register-the-model"></a>Guardado y registro del modelo

Una vez que haya entrenado el modelo, puede guardarlo y registrarlo en el área de trabajo. El registro del modelo permite almacenar los modelos y crear versiones de ellos en el área de trabajo para simplificar la [administración e implementación de modelos](concept-model-management-and-deployment.md).


Tras completar el entrenamiento del modelo, regístrelo en el área de trabajo con el código siguiente.  

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```

> [!TIP]
> El modelo que acaba de registrar se implementa exactamente de la misma manera que cualquier otro modelo registrado en Azure Machine Learning, independientemente del estimador que haya usado para el entrenamiento. El procedimiento de implementación contiene una sección sobre el registro de modelos, pero puede ir directamente a la [creación de un destino de proceso](how-to-deploy-and-where.md#choose-a-compute-target) para la implementación, dado que ya tiene un modelo registrado.

También puede descargar una copia local del modelo. Esto puede ser útil para realizar trabajos de validación de modelo adicionales de forma local. En el script de entrenamiento, `chainer_mnist.py`, un objeto de protector guarda el modelo en una carpeta local (local para el destino de proceso). Puede usar el objeto Run para descargar una copia del almacén de datos.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha entrenado y registrado un aprendizaje profundo y una red neuronal mediante Chainer en Azure Machine Learning. Para información sobre cómo implementar un modelo, continúe con nuestro artículo sobre la [implementación de modelos](how-to-deploy-and-where.md).

* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)

* [Vea nuestra arquitectura de referencia para el entrenamiento del aprendizaje profundo distribuido en Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
