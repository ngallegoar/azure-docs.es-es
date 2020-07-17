---
title: Creación de conjuntos de datos de Azure Machine Learning para acceder a los datos
titleSuffix: Azure Machine Learning
description: Aprenda a crear conjuntos de datos de Azure Machine Learning para acceder a los datos necesarios en las ejecuciones de experimentos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 06/29/2020
ms.openlocfilehash: baa238f36c41b5f494e8748cd5cd563bd212f483
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610717"
---
# <a name="create-azure-machine-learning-datasets"></a>Creación de conjuntos de datos de Azure Machine Learning

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo aprenderá a crear conjuntos de datos de Azure Machine Learning para acceder a los datos de los experimentos locales o remotos. Para comprender el lugar de los almacenes de datos en el flujo de trabajo global de acceso a datos de Azure Machine Learning, consulte el artículo [Acceso seguro a los datos](concept-data.md#data-workflow).

Con los conjuntos de datos de Azure Machine Learning, puede:

* Mantener una copia de datos única en el almacenamiento, a la que hacen referencia los conjuntos de datos.

* Acceder sin problemas a los datos durante el entrenamiento de un modelo, sin preocuparse por la ruta de acceso a los datos ni por las cadenas de conexión.

* Compartir datos y colaborar con otros usuarios.

## <a name="prerequisites"></a>Requisitos previos
Para crear y trabajar con conjuntos de datos, necesita lo siguiente:

* Suscripción a Azure. Si no tiene una, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.

> [!NOTE]
> Algunas clases de conjunto de tipos tienen dependencias en el paquete [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py), que solo es compatible con Python de 64 bits. Para los usuarios de Linux, estas clases solo se admiten en las siguientes distribuciones:  Red Hat Enterprise Linux, Ubuntu, Fedora y CentOS.

## <a name="compute-size-guidance"></a>Orientación sobre el tamaño de proceso

Cuando cree un conjunto de datos, revise su capacidad de procesamiento y el tamaño de los datos en memoria. El tamaño de los datos en el almacenamiento no es el mismo que en una trama de datos. Por ejemplo, los datos de los archivos CSV se pueden expandir hasta 10 veces en una trama de datos, por lo que un archivo CSV de 1 GB puede convertirse en 10 GB en una trama de datos. 

El factor principal es el tamaño del conjunto de datos en la memoria; es decir, como una trama de datos. Se recomienda que el tamaño de proceso y la capacidad de procesamiento tengan una RAM del doble del tamaño. Por lo tanto, si la trama de datos es de 10 GB, necesitará un destino de proceso con más de 20 GB de RAM para asegurarse de que dicha trama de datos tiene suficiente espacio en la memoria y se puede procesar. Si los datos están comprimidos, pueden expandirse más. 20 GB de datos relativamente dispersos almacenados en el formato Parquet comprimido pueden expandirse hasta usar aproximadamente 800 GB en memoria. Dado que los archivos Parquet almacenan los datos en un formato de columnas, si solo necesita la mitad de las columnas, solo tiene que cargar aproximadamente 400 GB en la memoria.
 
Si usa Pandas, no es necesario que tenga más de una CPU virtual, ya que solo usará esa cifra. Puede paralelizar fácilmente a muchas CPU virtuales en una única instancia o nodo de proceso de Azure Machine Learning a través de Modin y Dask/Ray, así como escalar horizontalmente a un clúster grande (si es necesario) con solo cambiar `import pandas as pd` por `import modin.pandas as pd`. 
 
Si no obtiene un espacio virtual lo suficientemente grande para los datos, tiene dos opciones: usar un marco como Spark o Dask para realizar el procesamiento en los datos "fuera de la memoria"; es decir, la trama de datos se carga a la memoria RAM partición por partición y se procesa y el resultado final se recopila al final. Si este procedimiento es demasiado lento, Spark o Dask le permiten escalar horizontalmente a un clúster que aún se podría usar de forma interactiva. 

## <a name="dataset-types"></a>Tipos de conjuntos de datos

Hay dos tipos de conjuntos de datos en función de la forma en que los usuarios los consumen en el entrenamiento:

* [TabularDatasets](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) representa los datos en formato tabular mediante el análisis del archivo o la lista de archivos proporcionados. Esto le proporciona la capacidad de materializar los datos en un DataFrame de Pandas o Spark. Puede crear un objeto `TabularDataset` a partir de archivos. csv, .tsv, .parquet y .jsonl, así como de los resultados de una consulta SQL. Para obtener una lista completa, vea la [clase TabularDatasetFactory](https://aka.ms/tabulardataset-api-reference).

* La clase [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) hace referencia a uno o varios archivos de sus almacenes de datos o direcciones URL públicas. Con este método, puede descargar o montar los archivos en el proceso como un objeto FileDataset. Los archivos pueden estar en cualquier formato, lo que permite una gama mayor de escenarios de aprendizaje automático, lo que incluye el aprendizaje profundo. 

## <a name="create-datasets"></a>Creación de conjuntos de datos

Mediante la creación de un conjunto de datos, puede crear una referencia a la ubicación del origen de datos, junto con una copia de sus metadatos. Al conservarse los datos en la ubicación existente, no se genera ningún costo de almacenamiento adicional. Puede crear conjuntos de datos de `TabularDataset` y `FileDataset` mediante el SDK de Python o Azure Machine Learning Studio en https://ml.azure.com.

Para que Azure Machine Learning pueda acceder a los datos, deben crearse conjuntos de datos a partir de las rutas de acceso de los [almacenes de datos de Azure](how-to-access-data.md) o las direcciones URL web públicas. 

### <a name="use-the-sdk"></a>Uso del SDK

Para crear conjuntos de datos desde un [almacén de datos de Azure](how-to-access-data.md) mediante el SDK de Python:

1. Compruebe que `contributor` o `owner` acceden al almacén de datos de Azure registrado.

2. Cree el conjunto de datos mediante referencias a rutas de acceso en el almacén de datos.

> [!Note]
> Puede crear un conjunto de datos a partir de varias rutas de acceso en varios almacenes de datos. No hay ningún límite en el número de archivos ni el tamaño de los datos a partir de los cuales puede crear un conjunto de datos. Aunque para cada ruta de acceso de datos, se enviarán algunas solicitudes al servicio de almacenamiento para comprobar si dicha ruta dirige a un archivo o una carpeta. Esta sobrecarga puede provocar una disminución del rendimiento o un error. Un conjunto de datos que hace referencia a una carpeta con 1000 archivos se considera que hace referencia a una ruta de acceso a datos. Se recomienda crear un conjunto de resultados que haga referencia a menos de 100 rutas de acceso en almacenes de datos para un rendimiento óptimo.

#### <a name="create-a-tabulardataset"></a>Creación de un objeto TabularDataset

Use el método [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none--support-multi-line-false-) en la clase `TabularDatasetFactory` para leer archivos en los formatos .csv o .tsv, y crear una clase TabularDataset sin registrar. Si se lee de varios archivos, los resultados se agregarán en una representación tabular. 

En el código siguiente se obtiene el área de trabajo existente y el almacén de datos deseado por nombre. A continuación, pasa las ubicaciones del almacén de datos y los archivos al parámetro `path` para crear un nuevo objeto TabularDataset, `weather_ds`.

```Python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
    
# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)

# create a TabularDataset from 3 file paths in datastore
datastore_paths = [(datastore, 'weather/2018/11.csv'),
                   (datastore, 'weather/2018/12.csv'),
                   (datastore, 'weather/2019/*.csv')]

weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

De forma predeterminada, al crear un objeto TabularDataset, los tipos de datos de las columnas se deducen automáticamente. Si los tipos deducidos no coinciden con los esperados, puede especificar los tipos de columna mediante el código siguiente. El parámetro `infer_column_type` solo es aplicable a los conjuntos de datos creados a partir de archivos delimitados. También puede [obtener más información acerca de los tipos de datos compatibles](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py).

> [!IMPORTANT] 
> Si el almacenamiento está detrás de un firewall o una red virtual, solo se admite la creación de un conjunto de datos a través del SDK. Para crear el conjunto de datos, asegúrese de incluir los parámetros `validate=False` y `infer_column_types=False` en el método `from_delimited_files()`. Esto omite la comprobación de validación inicial y garantiza que se pueda crear el conjunto de datos a partir de estos archivos seguros. 

```Python
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|Nombre|Sex|Age|SibSp|Parch|Vale|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|hombre|22,0|1|0|A/5 21171|7,2500||S
1|2|True|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|mujer|38,0|1|0|PC 17599|71,2833|C85|C
2|3|True|3|Heikkinen, Miss. Laina|mujer|26,0|0|0|STON/O2. 3101282|7,9250||S

Para crear un conjunto de datos a partir de un dataframe de Pandas en memoria, escriba los datos en un archivo local —como un archivo CSV— y cree el conjunto de datos a partir de ese archivo. El código siguiente muestra este flujo de trabajo.

```python
local_path = 'data/prepared.csv'
dataframe.to_csv(local_path)
upload the local file to a datastore on the cloud
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required
from azureml.core import Workspace, Dataset

subscription_id = 'xxxxxxxxxxxxxxxxxxxxx'
resource_group = 'xxxxxx'
workspace_name = 'xxxxxxxxxxxxxxxx'

workspace = Workspace(subscription_id, resource_group, workspace_name)

# get the datastore to upload prepared data
datastore = workspace.get_default_datastore()

# upload the local file from src_dir to the target_path in datastore
datastore.upload(src_dir='data', target_path='data')
# create a dataset referencing the cloud location
dataset = Dataset.Tabular.from_delimited_files(datastore.path('data/prepared.csv'))
```

Use el método [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none--query-timeout-30-) en la clase `TabularDatasetFactory` para leer desde Azure SQL Database:

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

En los objetos TabularDataset, puede especificar una marca de tiempo desde una columna de los datos o desde el lugar en que estén almacenados los datos del patrón de ruta de acceso para habilitar un rasgo de la serie temporal. Esta especificación permite filtrar de forma fácil y eficaz por hora.

Use el método [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) en la clase `TabularDataset` para especificar la columna de marca de tiempo y habilitar el filtrado por tiempo. Para más información, consulte [Demostración de la API relacionada con una serie temporal tabular con los datos meteorológicos de NOAA](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb).

```Python
# create a TabularDataset with time series trait
datastore_paths = [(datastore, 'weather/*/*/*/data.parquet')]

# get a coarse timestamp column from the path pattern
dataset = Dataset.Tabular.from_parquet_files(path=datastore_path, partition_format='weather/{coarse_time:yyy/MM/dd}/data.parquet')

# set coarse timestamp to the virtual column created, and fine grain timestamp from a column in the data
dataset = dataset.with_timestamp_columns(fine_grain_timestamp='datetime', coarse_grain_timestamp='coarse_time')

# filter with time-series-trait-specific methods
data_slice = dataset.time_before(datetime(2019, 1, 1))
data_slice = dataset.time_after(datetime(2019, 1, 1))
data_slice = dataset.time_between(datetime(2019, 1, 1), datetime(2019, 2, 1))
data_slice = dataset.time_recent(timedelta(weeks=1, days=1))
```

#### <a name="create-a-filedataset"></a>Creación de un objeto FileDataset

Use el método [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) en la clase `FileDatasetFactory` para cargar archivos en cualquier formato y crear un objeto FileDataset sin registrar. Si el almacenamiento está detrás de un firewall o una red virtual, establezca el parámetro `validate =False` en el método `from_files()`. Esto omite el paso de validación inicial y garantiza que se pueda crear el conjunto de datos a partir de estos archivos seguros.

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [(datastore, 'animals')]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = ['https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
             'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz']
mnist_ds = Dataset.File.from_files(path=web_paths)
```

#### <a name="on-the-web"></a>En la Web 
Tanto los siguientes pasos como la animación muestran cómo crear un conjunto de datos en Azure Machine Learning Studio, https://ml.azure.com.

![Creación de un conjunto de datos con la interfaz de usuario](./media/how-to-create-register-datasets/create-dataset-ui.gif)

Para crear un conjunto de datos en el estudio:
1. Inicie sesión en https://ml.azure.com.
1. Seleccione **Conjuntos de datos** en la sección **Recursos** del panel de la izquierda. 
1. Seleccione **Crear conjunto de datos** para elegir el origen del conjunto de datos. Este origen puede ser archivos locales, un almacén de datos o direcciones URL públicas.
1. Seleccione **Tabular** o **Archivo** para el tipo de conjunto de datos.
1. Seleccione **Siguiente** para abrir el formulario **Datastore and file selection** (Almacén de datos y selección de archivos). En este formulario, seleccione dónde desea mantener el conjunto de datos después de su creación, así como los archivos de datos que se usarán para el conjunto de datos. 
1. Seleccione **Siguiente** para rellenar los formularios **Settings and preview** (Configuración y versión preliminar) y **Esquema**; se rellenan de forma inteligente en función del tipo de archivo y se puede configurar el conjunto de archivos antes de la creación en estos formularios. 
1. Seleccione **Siguiente** para revisar el formulario **Confirmar detalles**. Compruebe sus selecciones y cree un perfil de datos opcional para el conjunto de datos. Más información acerca de la [generación de perfiles de datos](how-to-use-automated-ml-for-ml-models.md#profile). 
1. Seleccione **Crear** para completar la creación del conjunto de datos.

## <a name="register-datasets"></a>Registro de conjuntos de datos

Para completar el proceso de creación, registre los conjuntos de datos con un área de trabajo. Use el método [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--create-new-version-false-) para registrar los conjuntos de datos con el área de trabajo, con el fin de que puedan compartirse con otros usuarios y reutilizarse en varios experimentos en el área de trabajo:

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Los conjuntos de datos creados mediante Azure Machine Learning Studio se registran automáticamente en el área de trabajo.

## <a name="create-datasets-with-azure-open-datasets"></a>Creación de conjuntos de datos con Azure Open Datasets

[Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) son conjuntos de datos públicos mantenidos que puede usar para agregar características de escenarios específicos a soluciones de aprendizaje automático a fin de obtener modelos más precisos. Los conjuntos de datos incluyen datos de dominio público para el clima, censos, días festivos, seguridad pública y ubicación, que le ayudarán a entrenar los modelos de Machine Learning y enriquecer las soluciones predictivas. Open Datasets se encuentra en la nube en Microsoft Azure y se incluye en el SDK y en la interfaz de usuario del área de trabajo.

### <a name="use-the-sdk"></a>Uso del SDK

Para crear conjuntos de datos con Azure Open Datasets desde el SDK, asegúrese de que ha instalado el paquete con `pip install azureml-opendatasets`. Cada conjunto de datos discretos se representa mediante su propia clase en el SDK y ciertas clases están disponibles como `TabularDataset`, `FileDataset`, o ambos. Consulte la [documentación de referencia](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py) para obtener una lista completa de clases.

Puede recuperar ciertas clases como `TabularDataset` o `FileDataset`, lo que le permite manipular y descargar los archivos directamente. Otras clases **solo** pueden obtener un conjunto de datos mediante las funciones `get_tabular_dataset()` o `get_file_dataset()`. En el ejemplo de código siguiente se muestran algunos ejemplos de estos tipos de clases.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Cuando se registra un conjunto de datos creado desde Open Datasets, los datos no se descargan inmediatamente, pero se tendrá acceso a ellos más adelante cuando se soliciten (durante el entrenamiento, por ejemplo) desde una ubicación de almacenamiento central.

### <a name="use-the-ui"></a>Uso de la interfaz de usuario

También puede crear conjuntos de datos a partir de clases Open Datasets mediante la interfaz de usuario. En el área de trabajo, seleccione la pestaña **Conjunto de datos** en **Recursos**. En el menú desplegable **Crear conjunto de datos**, seleccione **From Open Datasets** (Desde Open Datasets).

![Abrir un conjunto de datos con la interfaz de usuario](./media/how-to-create-register-datasets/open-datasets-1.png)

Seleccione un conjunto de datos seleccionando su icono (tiene la opción de filtrar mediante la barra de búsqueda). Seleccione **Next** (Siguiente).

![Elegir conjunto de datos](./media/how-to-create-register-datasets/open-datasets-2.png)

Elija el nombre con que va a registrar el conjunto de datos y, opcionalmente, filtre los datos mediante los filtros disponibles. En este caso, para el conjunto de datos de días festivos público, filtre el período a un año y el código de país a solo EE. UU. Seleccione **Crear**.

![Establecer parámetros del conjunto de datos y crear conjunto de datos](./media/how-to-create-register-datasets/open-datasets-3.png)

El conjunto de valores ahora está disponible en su área de trabajo, en **Conjunto de datos**. Puede usarlo de la misma manera que otros conjuntos de valores que haya creado.

## <a name="version-datasets"></a>Conjuntos de datos de versión

Puede registrar un nuevo conjunto de datos con el mismo nombre mediante la creación de una nueva versión. La versión del conjunto de datos es una manera de delimitar el estado de los datos, con el fin de que pueda aplicar una versión específica del conjunto de datos para la experimentación o la reproducción futura. Obtenga más información sobre las [versiones del conjunto de datos](how-to-version-track-datasets.md).
```Python
# create a TabularDataset from Titanic training data
web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
             'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```

## <a name="access-datasets-in-your-script"></a>Obtener acceso a los conjuntos de datos del script

A los conjuntos de bases de datos registrados se puede acceder de local y remota en clústeres de proceso como el proceso de Azure Machine Learning. Para acceder a un conjunto de datos registrado en los experimentos, use el siguiente código para acceder al área de trabajo y al conjunto de datos registrado por nombre. De forma predeterminada, el método [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) de la clase `Dataset` devuelve la versión más reciente del conjunto de datos registrada en el área de trabajo.

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

## <a name="access-datasets-in-a-virtual-network"></a>Obtener acceso a los conjuntos de valores de una red virtual

Si el área de trabajo está en una red virtual, debe configurar el conjunto de datos para omitir la validación. Para más información sobre cómo usar los almacenes de datos y los conjuntos de datos en una red virtual, consulte [Aislamiento de red durante el entrenamiento e inferencia con redes virtuales privadas](how-to-enable-virtual-network.md#use-datastores-and-datasets).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [cómo entrenar con conjuntos de datos](how-to-train-with-datasets.md).
* Use el aprendizaje automático automatizado para [entrenar con TabularDatasets](https://aka.ms/automl-dataset).
* Para ver más ejemplos de entrenamiento de conjuntos de datos, consulte los [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
