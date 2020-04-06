---
title: Entrenamiento con azureml-datasets
titleSuffix: Azure Machine Learning
description: Aprenda a usar conjuntos de datos en el entrenamiento.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/09/2020
ms.openlocfilehash: 401383f2d483836bf725051810d78167869f7b22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79237016"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Entrenamiento con conjuntos de datos en Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá las dos maneras de consumir [conjuntos de datos de Azure Machine Learning](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) en una ejecución de entrenamiento de experimento remota sin preocuparse por las cadenas de conexión o las rutas de acceso a los datos.

- Opción 1: Si tiene datos estructurados, cree un TabularDataset y úselo directamente en el script de entrenamiento.

- Opción 2: Si tiene datos no estructurados, cree un FileDataset y monte o descargue archivos en un proceso remoto para el entrenamiento.

Los conjuntos de datos de Azure Machine Learning proporcionan una integración perfecta con productos de entrenamiento de Azure Machine Learning, como [ScriptRun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py), [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py), [HyperDrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py) y [canalizaciones de Azure Machine Learning](how-to-create-your-first-pipeline.md).

## <a name="prerequisites"></a>Prerrequisitos

Para crear conjuntos de datos y entrenar con ellos, necesita:

* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.

> [!Note]
> Algunas clases Dataset tienen dependencias del paquete [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py). Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="option-1-use-datasets-directly-in-training-scripts"></a>Opción 1: Usar los conjuntos de datos directamente en los scripts de entrenamiento

En este ejemplo, creará un objeto [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) que usará como entrada directa para el objeto `estimator` para entrenamiento. 

### <a name="create-a-tabulardataset"></a>Creación de un objeto TabularDataset

En el código siguiente se crea un objeto TabularDataset no registrado a partir de una dirección URL web. También puede crear conjuntos de datos a partir de archivos o rutas de acceso locales en almacenes de datos. Más información sobre [cómo crear conjuntos de datos](https://aka.ms/azureml/howto/createdatasets).

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

### <a name="access-the-input-dataset-in-your-training-script"></a>Acceso al conjunto de datos de entrada en el script de entrenamiento

Los objetos TabularDataset ofrecen la posibilidad de cargar los datos en una biblioteca Pandas o en DataFrame de Spark para que pueda trabajar con bibliotecas conocidas de entrenamiento y preparación de datos. Para aprovechar esta funcionalidad, puede pasar un objeto TabularDataset como entrada en la configuración de entrenamiento y, luego, recuperarlo en el script.

Para ello, acceda al conjunto de datos de entrada mediante el objeto [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py) del script de entrenamiento y use el método [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--). 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic']
# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-estimator"></a>Configuración del estimador

Un objeto [estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) se usa para enviar la ejecución del experimento. Azure Machine Learning tiene estimadores preconfigurados para marcos de aprendizaje automático comunes, así como un estimador genérico.

Este código crea un objeto estimator genérico, `est`, que especifica:

* Un directorio de script para los scripts. Todos los archivos de este directorio se cargan en los nodos del clúster para su ejecución.
* El script de entrenamiento, *train_titanic.* .
* El conjunto de datos de entrada para el entrenamiento, `titanic`. `as_named_input()` es necesario para que el nombre asignado pueda hacer referencia al conjunto de datos de entrada en el script de entrenamiento. 
* El destino de proceso del experimento.
* La definición de entorno del experimento.

```Python
est = Estimator(source_directory=script_folder,
                entry_script='train_titanic.py',
                # pass dataset object as an input with name 'titanic'
                inputs=[titanic_ds.as_named_input('titanic')],
                compute_target=compute_target,
                environment_definition= conda_env)

# Submit the estimator as part of your experiment run
experiment_run = experiment.submit(est)
experiment_run.wait_for_completion(show_output=True)
```


## <a name="option-2--mount-files-to-a-remote-compute-target"></a>Opción 2:  Montar los archivos en un destino de proceso remoto

Si quiere que los archivos de datos estén disponibles en el destino de proceso para el entrenamiento, use [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) para montar o descargar los archivos a los que hace referencia.

### <a name="mount-vs-download"></a>Montaje frente a Descargar

Tanto el montaje como la descarga de archivos de cualquier formato se admiten en los conjuntos de datos creados desde Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database y Azure Database for PostgreSQL. 

Al montar un conjunto de datos, se asocian a un directorio (punto de montaje) los archivos a los que hace referencia en el conjunto de datos y este se vuelve disponible en el destino de proceso. El montaje se admite en procesos basados en Linux, como Proceso de Azure Machine Learning, máquinas virtuales y HDInsight. Al descargar un conjunto de datos, todos los archivos a los que se hace referencia en él se descargarán en el destino de proceso. La descarga se admite con todos los tipos de proceso. 

Si el script procesa todos los archivos a los que se hace referencia en el conjunto de datos y el disco de proceso puede caber en el conjunto de datos completo, se recomienda la descarga para evitar la sobrecarga de la transmisión de datos desde los servicios de almacenamiento. Si el tamaño de los datos supera el tamaño del disco de proceso, no es posible realizar la descarga. En este escenario, se recomienda el montaje, ya que en el momento del procesamiento solo se cargan los archivos de datos usados por el script.

El siguiente código monta el elemento `dataset` en el directorio temporal de `mounted_path`.

```python
import tempfile
mounted_path = tempfile.mkdtemp()

# mount dataset onto the mounted_path of a Linux-based compute
mount_context = dataset.mount(mounted_path)

mount_context.start()

import os
print(os.listdir(mounted_path))
print (mounted_path)
```

### <a name="create-a-filedataset"></a>Creación de un objeto FileDataset

En el ejemplo siguiente se crea un objeto FileDataset no registrado a partir de direcciones URL web. Más información sobre [cómo crear conjuntos de datos](https://aka.ms/azureml/howto/createdatasets) desde otros orígenes.

```Python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
mnist_ds = Dataset.File.from_files(path = web_paths)
```

### <a name="configure-the-estimator"></a>Configuración del estimador

Además de pasar el conjunto de datos mediante el parámetro `inputs` en el estimador, también puede pasarlo mediante `script_params` y obtener la ruta de acceso a los datos (punto de montaje) en el script de entrenamiento por medio de argumentos. De este modo, puede mantener el script de entrenamiento independiente de azureml-sdk. En otras palabras, podrá usar el mismo script de entrenamiento para la depuración local y el entrenamiento remoto en todas las plataformas en la nube.

Un objeto estimator [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) se usa para enviar la ejecución de experimentos scikit-learn. Obtenga más información sobre el entrenamiento con el [calculador de SKlearn](how-to-train-scikit-learn.md).

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    # mount the dataset on the remote compute and pass the mounted path as an argument to the training script
    '--data-folder': mnist_ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.5
}

est = SKLearn(source_directory=script_folder,
              script_params=script_params,
              compute_target=compute_target,
              environment_definition=env,
              entry_script='train_mnist.py')

# Run the experiment
run = experiment.submit(est)
run.wait_for_completion(show_output=True)
```

### <a name="retrieve-the-data-in-your-training-script"></a>Recuperación de los datos en el script de entrenamiento

Después de enviar la ejecución, los archivos de datos a los que el conjunto de datos `mnist` hace referencia se montarán en el destino de proceso. En el código siguiente se muestra cómo recuperar los datos en el script.

```Python
%%writefile $script_folder/train_mnist.py

import argparse
import os
import numpy as np
import glob

from utils import load_data

# retrieve the 2 arguments configured through script_params in estimator
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = args.data_folder
print('Data folder:', data_folder)

# get the file paths on the compute
X_train_path = glob.glob(os.path.join(data_folder, '**/train-images-idx3-ubyte.gz'), recursive=True)[0]
X_test_path = glob.glob(os.path.join(data_folder, '**/t10k-images-idx3-ubyte.gz'), recursive=True)[0]
y_train_path = glob.glob(os.path.join(data_folder, '**/train-labels-idx1-ubyte.gz'), recursive=True)[0]
y_test = glob.glob(os.path.join(data_folder, '**/t10k-labels-idx1-ubyte.gz'), recursive=True)[0]

# load train and test set into numpy arrays
X_train = load_data(X_train_path, False) / 255.0
X_test = load_data(X_test_path, False) / 255.0
y_train = load_data(y_train_path, True).reshape(-1)
y_test = load_data(y_test, True).reshape(-1)
```

## <a name="notebook-examples"></a>Ejemplos de cuadernos

Los [cuadernos de conjunto de datos](https://aka.ms/dataset-tutorial) se demuestran y se analizan con mayor profundidad sobre los conceptos en este artículo.

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento automático de modelos de Machine Learning](how-to-auto-train-remote.md) con TabularDatasets

* [Entrenamiento de modelos de clasificación de imágenes](https://aka.ms/filedataset-samplenotebook) con FileDatasets

* [Entrenamiento con conjuntos de datos mediante canalizaciones](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/pipeline-with-datasets/pipeline-for-image-classification.ipynb)
