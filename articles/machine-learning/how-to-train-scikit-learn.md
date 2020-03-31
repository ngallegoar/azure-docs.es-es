---
title: Entrenamiento de modelos de Machine Learning con scikit-learn
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo ejecutar los scripts de entrenamiento de scikit-learn a escala empresarial mediante la clase estimator de SKlearn de Azure Machine Learning. Los scripts de ejemplo clasifican imágenes de flores Iris para crear un modelo de Machine Learning basado en el conjunto de datos de Iris de scikit-learn.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: bdd2cc400c3df75742689258caea8cb87ee8ccc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942277"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Creación de modelos de Scikit-learn a escala con Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenda a ejecutar los scripts de entrenamiento de scikit-learn a escala empresarial mediante la clase [estimator de SKlearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) de Azure Machine Learning. 

Los scripts de ejemplo de este artículo se usan para clasifica imágenes de flores Iris para crear un modelo de Machine Learning basado en el [conjunto de datos de Iris](https://archive.ics.uci.edu/ml/datasets/iris) de scikit-learn.

Tanto si va a entrenar un modelo de scikit-learn de Machine Learning desde el principio como si va a incorporar un modelo existente a la nube, puede usar Azure Machine Learning para escalar horizontalmente trabajos de entrenamiento de código abierto mediante recursos de proceso en la nube elástica. Puede compilar, implementar y supervisar modelos de nivel de producción, así como crear versiones de dichos mismos, mediante Azure Machine Learning.

## <a name="prerequisites"></a>Prerrequisitos

Ejecute este código en cualquiera de estos entornos:
 - Instancia de Proceso de Azure Machine Learning: no se necesitan descargas ni instalación

    - Complete el [Tutorial: Configuración del entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md) para crear un servidor de cuadernos dedicado en el que se habrán cargado previamente el SDK y el repositorio de ejemplos.
    - Para encontrar un cuaderno completado y expandido, en la carpeta de entrenamiento de ejemplos en el servidor de cuadernos, vaya a este directorio: carpeta **how-to-use-azureml > ml-frameworks > scikit-learn > training > train-hyperparameter-tune-deploy-with-sklearn**.

 - Su propio servidor de Jupyter Notebook

    - [Instalación del SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
    - [Cree un archivo de configuración del área de trabajo](how-to-configure-environment.md#workspace).
    - Descarga del conjunto de datos y el archivo de script de ejemplo 
        - [iris dataset](https://archive.ics.uci.edu/ml/datasets/iris)
        - [train_iris.py](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn)
    - También puede encontrar una [versión de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb) completada de esta guía en la página de ejemplos de GitHub. El cuaderno incluye una sección ampliada que abarca la optimización de hiperparámetros inteligentes y la recuperación del mejor modelo mediante las métricas principales.

## <a name="set-up-the-experiment"></a>Configuración del experimento

En esta sección, para configurar el experimento de entrenamiento, se cargan los paquetes de Python necesarios, se inicializa un área de trabajo, se crea un experimento y se cargan los datos y scripts de entrenamiento.

### <a name="import-packages"></a>Importación de paquetes

En primer lugar, importe las bibliotecas de Python necesarias.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Inicialización de un área de trabajo

El [área de trabajo de Azure Machine Learning](concept-workspace.md) es el recurso de nivel superior para el servicio. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree. En el SDK de Python, puede acceder a los artefactos del área de trabajo mediante la creación de un objeto [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).

Cree un objeto de área de trabajo a partir del archivo `config.json` creado en la [sección de requisitos previos](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-a-machine-learning-experiment"></a>Creación de un experimento de aprendizaje automático

Cree un experimento y una carpeta para almacenar los scripts de entrenamiento. En este ejemplo, cree un experimento denominado "sklearn-iris".

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="prepare-training-script"></a>Preparación del script de entrenamiento

En este tutorial ya se proporciona el script de entrenamiento **train_iris.py**. En la práctica, debería poder usar cualquier script de entrenamiento personalizado tal cual y ejecutarlo con Azure Machine Learning sin tener que modificar el código.

Para usar las funcionalidades de seguimiento y métricas de Azure ML, agregue una pequeña cantidad de código de Azure ML dentro del script de entrenamiento.  El script de entrenamiento **train_iris.py** muestra cómo registrar algunas métricas para la ejecución de Azure Machine Learning mediante el objeto `Run` en el script.

El script de entrenamiento proporcionado usa datos de ejemplo de la función `iris = datasets.load_iris()`.  Para sus propios datos, puede que necesite realizar pasos como [cargar el conjunto de datos y los scripts](how-to-train-keras.md#data-upload) para que los datos estén disponibles durante el entrenamiento.

Copie el script de entrenamiento **train_iris.py** en el directorio del proyecto.

```
import shutil
shutil.copy('./train_iris.py', project_folder)
```

## <a name="create-or-get-a-compute-target"></a>Creación u obtención de un destino de proceso

Cree un destino de proceso en el que ejecutar el trabajo de scikit-learn. Scikit-learn solo admite el procesamiento de CUP de nodo único.

El código siguiente crea un proceso administrado de Azure Machine Learning (AmlCompute) para el recurso de proceso de entrenamiento remoto. La creación de AmlCompute tarda aproximadamente cinco minutos. Si el AmlCompute con ese nombre ya está en el área de trabajo, este código omitirá el proceso de creación.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Para más información sobre los destinos de proceso, vea el artículo [¿Qué es un destino de proceso?](concept-compute-target.md)

## <a name="create-a-scikit-learn-estimator"></a>Creación de un estimador de scikit-learn

El [estimador de scikit-learn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn?view=azure-ml-py) proporciona una manera sencilla de iniciar un trabajo de entrenamiento de scikit-learn en un destino de proceso. Se implementa a través de la clase [`SKLearn`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), que se puede usar para admitir el entrenamiento de CPU de nodo único.

Si el script de entrenamiento necesita paquetes adicionales pip o conda para ejecutarse, puede hacer que los paquetes se instalen en la imagen de Docker resultante. Para ello, pase sus nombres mediante los argumentos `pip_packages` y `conda_packages`.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py'
                    pip_packages=['joblib']
                   )
```


Para más información sobre cómo personalizar el entorno de Python, vea el tema sobre la [creación y administración de entornos de entrenamiento e implementación](how-to-use-environments.md). 

## <a name="submit-a-run"></a>Envío de una ejecución

El [objeto Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) proporciona la interfaz para el historial de ejecución mientras se ejecuta el trabajo y cuando se ha completado.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Durante la ejecución del objeto, pasa por las fases siguientes:

- **Preparando**: se crea una imagen de Docker según el estimador de TensorFlow. La imagen se carga en el registro de contenedor del área de trabajo y se almacena en memoria caché para ejecuciones posteriores. Los registros también se transmiten al historial de ejecución y se pueden consultar para supervisar el progreso.

- **Escalado**: el clúster intenta escalar verticalmente si el clúster de Batch AI requiere más nodos para realizar la ejecución de los que se encuentran disponibles actualmente.

- **En ejecución**: todos los scripts de la carpeta de scripts se cargan en el destino de proceso, se montan o se copian los almacenes de datos y se ejecuta entry_script. Las salidas de stdout y la carpeta ./logs se transmiten al historial de ejecución y se pueden usar para supervisar la ejecución.

- **Posprocesamiento**: la carpeta ./outputs de la ejecución se copia en el historial de ejecución.

## <a name="save-and-register-the-model"></a>Guardado y registro del modelo

Una vez que haya entrenado el modelo, puede guardarlo y registrarlo en el área de trabajo. El registro del modelo permite almacenar los modelos y crear versiones de ellos en el área de trabajo para simplificar la [administración e implementación de modelos](concept-model-management-and-deployment.md).

Para guardar el modelo, agregue el código siguiente al script de entrenamiento, train_iris.py. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Registre el modelo al área de trabajo con el código siguiente. Al especificar los parámetros `model_framework`, `model_framework_version` y `resource_configuration`, la implementación del modelo sin código está disponible. Esto le permite implementar directamente el modelo como un servicio web desde el modelo registrado, y el objeto [`ResourceConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.resource_configuration.resourceconfiguration?view=azure-ml-py) define el recurso de proceso del servicio web.

```Python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = run.register_model(model_name='sklearn-iris', 
                           model_path='outputs/model.joblib',
                           model_framework=Model.Framework.SCIKITLEARN,
                           model_framework_version='0.19.1',
                           resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5))
```

## <a name="deployment"></a>Implementación

El modelo que acaba de registrar se puede implementar exactamente de la misma manera que cualquier otro modelo registrado en Azure Machine Learning, independientemente del estimador que haya usado para el entrenamiento. El procedimiento de implementación contiene una sección sobre el registro de modelos, pero puede ir directamente a la [creación de un destino de proceso](how-to-deploy-and-where.md#choose-a-compute-target) para la implementación, dado que ya tiene un modelo registrado.

### <a name="preview-no-code-model-deployment"></a>(Versión preliminar) Implementación de modelo sin código

En lugar de la ruta de implementación tradicional, también puede usar la característica de implementación sin código (versión preliminar) para Scikit-learn. Se admite la implementación de modelo sin código para todos los tipos de modelos integrados de Scikit-learn. Mediante el registro del modelo como se ha indicado anteriormente con los parámetros `model_framework`, `model_framework_version` y `resource_configuration`, solo tiene que usar la función estática [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model%28class%29?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) para implementar el modelo.

```python
web_service = Model.deploy(ws, "scikit-learn-service", [model])
```

NOTA:  Estas dependencias se incluyen en el contenedor de inferencia Scikit-learn integrado previamente.

```yaml
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
```

El [procedimiento](how-to-deploy-and-where.md) completo trata la implementación en Azure Machine Learning más detalladamente.


## <a name="next-steps"></a>Pasos siguientes

En este artículo ha entrenado y registrado un modelo de Scikit-learn y ha aprendido sobre las opciones de implementación. Consulte estos otros artículos para más información sobre Azure Machine Learning.

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)
* [Arquitectura de referencia para el entrenamiento del aprendizaje profundo distribuido en Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
