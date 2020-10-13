---
title: Entrenamiento con azureml-datasets
titleSuffix: Azure Machine Learning
description: Aprenda a usar conjuntos de datos en el entrenamiento.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/31/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: aa1ba4aa07ee4cdc097bd4ed3e6e4d7563360a5d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91331801"
---
# <a name="train-with-datasets-in-azure-machine-learning"></a>Entrenamiento con conjuntos de datos en Azure Machine Learning


En este artículo obtendrá información acerca de cómo usar [conjuntos de datos de Azure Machine Learning](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py&preserve-view=true) en sus experimentos de entrenamiento.  Puede usar conjuntos de datos en el destino de proceso local o remoto sin preocuparse por las cadenas de conexión o las rutas de acceso a datos.

Los conjuntos de datos de Azure Machine Learning proporcionan una integración perfecta con funcionalidades de entrenamiento de Azure Machine Learning como [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true), [HyperDrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py&preserve-view=true) y [canalizaciones de Azure Machine Learning](how-to-create-your-first-pipeline.md).

## <a name="prerequisites"></a>Prerrequisitos

Para crear conjuntos de datos y entrenar con ellos, necesita:

* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true) (>= 1.13.0), que incluye el paquete azureml-datasets.

> [!Note]
> Algunas clases Dataset tienen dependencias del paquete [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py&preserve-view=true). Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="use-datasets-directly-in-training-scripts"></a>Usar los conjuntos de datos directamente en los scripts de entrenamiento

Si tiene datos estructurados que aún no se han registrado como un conjunto de datos, cree un conjunto de datos TabularDataset y úselo directamente en el script de entrenamiento para su experimento local o remoto.

En este ejemplo, se crea un elemento [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py&preserve-view=true) no registrado y se especifica como un argumento de script en el objeto ScriptRunConfig para el entrenamiento. Si desea volver a usar este conjunto de datos TabularDataset con otros experimentos en su área de trabajo, consulte [Registro de conjuntos de registros en su área de trabajo](how-to-create-register-datasets.md#register-datasets).

### <a name="create-a-tabulardataset"></a>Creación de un objeto TabularDataset

En el código siguiente se crea un objeto TabularDataset no registrado a partir de una dirección URL web.  

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

Los objetos TabularDataset ofrecen la posibilidad de cargar los datos de su conjunto de datos TabularDataset en un DataFrame de Pandas o Spark para que pueda trabajar con bibliotecas conocidas de entrenamiento y preparación de datos sin tener que salir del cuaderno.

### <a name="access-dataset-in-training-script"></a>Acceso al conjunto de datos en el script de entrenamiento

El código siguiente configura el argumento de script `--input-data` que se especificará al configurar la ejecución del entrenamiento (consulte la sección siguiente). Cuando se pasa el conjunto de datos tabulares como valor del argumento, Azure ML lo resolverá en el identificador del conjunto de datos, que a su vez podrá usar para acceder al conjunto de datos en el script de entrenamiento (sin tener que codificar el nombre o el identificador del conjunto de datos en el script). A continuación, se usa el método [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) para cargar ese conjunto de datos en un dataframe de Pandas para la exploración y preparación de los datos antes del entrenamiento.

> [!Note]
> Si el origen de datos original contiene NaN, cadenas vacías o valores en blanco, cuando se utiliza `to_pandas_dataframe()`, esos valores se reemplazan como valor *Null*.

Si necesita cargar los datos preparados en un nuevo conjunto de datos a partir de un dataframe de Pandas en memoria, escriba los datos en un archivo local, como un archivo Parquet, y cree el nuevo conjunto de datos a partir de ese archivo. También puede crear conjuntos de datos a partir de archivos o rutas de acceso locales en almacenes de datos. Más información sobre [cómo crear conjuntos de datos](how-to-create-register-datasets.md).

```Python
%%writefile $script_folder/train_titanic.py

import argparse
from azureml.core import Dataset, Run

parser = argparse.ArgumentParser()
parser.add_argument("--input-data", type=str)
args = parser.parse_args()

run = Run.get_context()
ws = run.experiment.workspace

# get the input dataset by ID
dataset = Dataset.get_by_id(ws, id=args.input_data)

# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-training-run"></a>Configuración de la ejecución de entrenamiento
Se usa un objeto [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py&preserve-view=true) para configurar y enviar la ejecución de entrenamiento.

Este código crea un objeto ScriptRunConfig, `src`, que especifica:

* Un directorio de script para los scripts. Todos los archivos de este directorio se cargan en los nodos del clúster para su ejecución.
* El script de entrenamiento, *train_titanic.* .
* El conjunto de datos de entrada del entrenamiento, `titanic_ds`, como un argumento de script. Azure ML lo resolverá en el identificador correspondiente del conjunto de datos cuando se pase al script.
* El destino de proceso de la ejecución.
* El entorno de la ejecución.

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=script_folder,
                      script='train_titanic.py',
                      # pass dataset as an input with friendly name 'titanic'
                      arguments=['--input-dataset', titanic_ds],
                      compute_target=compute_target,
                      environment=myenv)
                             
# Submit the run configuration for your training run
run = experiment.submit(src)
run.wait_for_completion(show_output=True)                             
```

## <a name="mount-files-to-remote-compute-targets"></a>Montar los archivos en destinos de proceso remotos

Si tiene datos no estructurados, cree un conjunto de datos [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset?view=azure-ml-py&preserve-view=true) y monte o descargue los archivos de datos para que estén disponibles para el destino de proceso remoto para el entrenamiento. Obtenga información acerca de cuándo usar [el montaje frente a la descarga](#mount-vs-download) para los experimentos de entrenamiento remoto. 

En el ejemplo siguiente se crea un conjunto de datos FileDataset y se monta el conjunto de datos en el destino de proceso, para lo cual se pasa como un argumento al script de entrenamiento. 

> [!Note]
> Si usa una imagen base de Docker personalizada, tendrá que instalar Fuse a través de `apt-get install -y fuse` como una dependencia para que el montaje del conjunto de datos funcione. Obtenga información sobre cómo [compilar una imagen de compilación personalizada](how-to-deploy-custom-docker-image.md#build-a-custom-base-image).

### <a name="create-a-filedataset"></a>Creación de un objeto FileDataset

En el ejemplo siguiente se crea un objeto FileDataset no registrado a partir de direcciones URL web. Más información sobre [cómo crear conjuntos de datos](how-to-create-register-datasets.md) desde otros orígenes.

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

### <a name="configure-the-training-run"></a>Configuración de la ejecución de entrenamiento
Se recomienda pasar el conjunto de datos como argumento cuando se realiza el montaje mediante el parámetro `arguments` del constructor de `ScriptRunConfig`. Al hacerlo, tendrá la ruta de acceso de datos (punto de montaje) en el script de entrenamiento mediante argumentos. En otras palabras, podrá usar el mismo script de entrenamiento para la depuración local y el entrenamiento remoto en todas las plataformas en la nube.

En el ejemplo siguiente se crea un objeto ScriptRunConfig que pasa el conjunto de datos FileDataset mediante `arguments`. Después de enviar la ejecución, los archivos de datos a los que el conjunto de datos `mnist_ds` hace referencia se montarán en el destino de proceso.

```python
from azureml.core import ScriptRunConfig

src = ScriptRunConfig(source_directory=script_folder,
                      script='train_mnist.py',
                      # the dataset will be mounted on the remote compute and the mounted path passed as an argument to the script
                      arguments=['--data-folder', mnist_ds.as_mount(), '--regularization', 0.5],
                      compute_target=compute_target,
                      environment=myenv)

# Submit the run configuration for your training run
run = experiment.submit(src)
run.wait_for_completion(show_output=True)
```

### <a name="retrieve-the-data-in-your-training-script"></a>Recuperación de los datos en el script de entrenamiento

En el código siguiente se muestra cómo recuperar los datos en el script.

```Python
%%writefile $script_folder/train_mnist.py

import argparse
import os
import numpy as np
import glob

from utils import load_data

# retrieve the 2 arguments configured through `arguments` in the ScriptRunConfig
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

## <a name="mount-vs-download"></a>Montaje frente a descarga

Tanto el montaje como la descarga de archivos de cualquier formato se admiten en los conjuntos de datos creados desde Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database y Azure Database for PostgreSQL. 

Al **montar** un conjunto de datos, se asocian a un directorio (punto de montaje) los archivos a los que hace referencia en el conjunto de datos, y este se vuelve disponible en el destino de proceso. El montaje se admite en procesos basados en Linux, como Proceso de Azure Machine Learning, máquinas virtuales y HDInsight. 

Al **descargar** un conjunto de datos, todos los archivos a los que se hace referencia en él se descargarán en el destino de proceso. La descarga se admite con todos los tipos de proceso. 

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

## <a name="directly-access-datasets-in-your-script"></a>Acceso directo a los conjuntos de datos en el script

A los conjuntos de bases de datos registrados se puede acceder de local y remota en clústeres de proceso como el proceso de Azure Machine Learning. Para acceder a un conjunto de datos registrado en los experimentos, use el siguiente código para acceder al área de trabajo y al conjunto de datos registrado por nombre. De forma predeterminada, el método [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py&preserve-view=true#&preserve-view=trueget-by-name-workspace--name--version--latest--) de la clase `Dataset` devuelve la versión más reciente del conjunto de datos registrada en el área de trabajo.

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="accessing-source-code-during-training"></a>Acceso al código fuente durante el entrenamiento

Azure Blob Storage presenta mayores velocidades de rendimiento que un recurso compartido de archivos de Azure y se escala a un gran número de trabajos iniciados en paralelo. Por esta razón, se recomienda configurar las ejecuciones de manera que usen Blob Storage para transferir archivos de código fuente.

En el ejemplo de código siguiente se especifica en la configuración de ejecución el almacén de datos de blobs que se usará para las transferencias de código fuente.

```python 
# workspaceblobstore is the default blob storage
src.run_config.source_directory_data_store = "workspaceblobstore" 
```

## <a name="notebook-examples"></a>Ejemplos de cuadernos

Los [cuadernos de conjunto de datos](https://aka.ms/dataset-tutorial) se demuestran y se analizan con mayor profundidad sobre los conceptos en este artículo.

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento automático de modelos de aprendizaje automático](how-to-auto-train-remote.md) con TabularDatasets.

* [Entrenamiento de modelos de clasificación de imágenes](https://aka.ms/filedataset-samplenotebook) con FileDatasets.

* [Entrenamiento con conjuntos de datos mediante canalizaciones](how-to-create-your-first-pipeline.md).
