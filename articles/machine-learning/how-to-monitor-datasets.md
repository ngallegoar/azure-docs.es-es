---
title: Analizar y supervisar el desfase de datos en conjuntos de datos (versión preliminar)
titleSuffix: Azure Machine Learning
description: Cree monitores de conjuntos de datos de Azure Machine Learning (versión preliminar), supervise el desfase de datos en los conjuntos de datos y configure alertas.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: nibaccam
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/04/2019
ms.openlocfilehash: 15cfa56f718290af3ae5fb87aadab70016cc8594
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84430245"
---
# <a name="detect-data-drift-preview-on-datasets"></a>Detección del desfase de datos (versión preliminar) en los conjuntos de datos
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a crear monitores de conjuntos de datos de Azure Machine Learning (versión preliminar), a supervisar el desfase de datos y los cambios estadísticos en los conjuntos de datos, así como a configurar alertas.

Con los monitores de conjuntos de datos de Azure Machine Learning, puede:
* **Analizar el desfase de los datos** para comprender cómo cambian con el tiempo.
* **Supervisar los datos del modelo** para conocer las diferencias entre los conjuntos de datos de entrenamiento y de servicio.
* **Supervisar los datos nuevos** para conocer las diferencias entre los conjuntos de datos de destino y los de referencia.
* **Perfilar características en los datos** para realizar un seguimiento de cómo cambian las propiedades estadísticas con el tiempo.
* **Configurar alertas sobre el desfase de datos** para tener advertencias tempranas de posibles problemas. 

Se pueden encontrar métricas e información detallada a través del recurso de [Azure Aplicación Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) asociado al área de trabajo de Azure Machine Learning.

> [!Important]
> Tenga en cuenta que la supervisión del desfase de datos con el SDK está disponible en todas las ediciones, mientras que hacerlo a través Studio en la web solo lo está en la edición Enterprise.

## <a name="prerequisites"></a>Prerrequisitos

Para crear y trabajar con conjuntos de datos, necesita:
* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
* El [SDK de Azure Machine Learning para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que incluye el paquete azureml-datasets.
* Datos estructurados (tabulares) con una marca de tiempo especificada en la ruta de acceso del archivo, el nombre de archivo o la columna de los datos.

## <a name="what-is-data-drift"></a>¿Qué es el desfase de datos?

En el contexto de aprendizaje automático, el desfase de datos es el cambio en los datos de entrada del modelo que conduce a la degradación del rendimiento del modelo. Es uno de los principales motivos por los que la precisión del modelo se degrada con el tiempo. Por lo tanto, la supervisión del desfase de datos ayuda a detectar problemas de rendimiento del modelo.

Entre las causas del desfase de datos se incluyen: 

- Cambios del proceso ascendente, como un sensor que se reemplaza que cambia las unidades de medida de pulgadas a centímetros. 
- Los problemas de calidad de los datos, como un sensor roto que siempre lee 0.
- Desfase natural en los datos, como la temperatura media que cambia con las estaciones.
- Cambio en relación entre las características o el turno covariable. 

Con los monitores de conjunto de datos Azure Machine Learning, puede configurar alertas que ayuden en la detección de desfase de datos en conjuntos de datos a lo largo del tiempo. 

### <a name="dataset-monitors"></a>Monitores de conjuntos de datos 

Puede crear un monitor de conjunto de datos para detectar y notificar el desfase de los datos en los nuevos datos de un conjunto de datos, analizar los datos históricos para desfasarlos y generar perfiles de datos nuevos a lo largo del tiempo. El algoritmo de desfase de datos proporciona una medida global del cambio en los datos y la indicación de qué características son responsables de una investigación más detallada. Los monitores de conjuntos de datos generan otras métricas mediante la generación de perfiles de nuevos datos en el conjunto de datos de `timeseries`. Mediante [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) se pueden configurar alertas personalizadas en todas las métricas generadas por el monitor. Los monitores de conjuntos de datos se pueden usar para detectar rápidamente problemas de datos y reducir el tiempo necesario para depurar el problema mediante la identificación de las causas probables.  

Conceptualmente, hay tres escenarios principales para configurar los monitores de conjunto de datos en Azure Machine Learning.

Escenario | Descripción
---|---
Supervisión de datos de servicio de un modelo para el desfase de los datos de entrenamiento del modelo | Los resultados de este escenario se pueden interpretar como la supervisión de un proxy para la precisión del modelo, dado que la precisión del modelo se degrada si los datos de servicio se desfasan de los datos de entrenamiento.
Supervisión de un conjunto de datos de una serie temporal para el desfase desde un período de tiempo anterior. | Este escenario es más general y se puede usar para supervisar los conjuntos de datos implicados en el flujo ascendente o descendente de la creación del modelo.  El conjunto de elementos de destino debe tener una columna de marca de tiempo, mientras que el conjunto de datos de referencia puede ser cualquier conjunto de datos tabular que tenga características en común con el conjunto de datos de destino.
Análisis de datos pasados. | Este escenario se puede utilizar para comprender los datos históricos e informar de las decisiones en la configuración de los monitores de conjunto de datos.

## <a name="how-dataset-can-monitor-data"></a>Supervisión de datos por parte del conjunto de datos

Al usar Azure Machine Learning, el desfase de datos se supervisa a través de conjuntos de datos. Para supervisar el desfase de datos, se especifica un conjunto de datos de base de referencia, normalmente el conjunto de datos de entrenamiento para un modelo. Un conjunto de datos de destino (normalmente datos de entrada del modelo) con el tiempo se compara con el conjunto de datos de referencia. Esta comparación significa que el conjunto de datos de destino debe tener especificada una columna de marca de tiempo.

### <a name="set-the-timeseries-trait-in-the-target-dataset"></a>Establecimiento del rasgo `timeseries` en el conjunto de datos de destino

El conjunto de datos de destino debe tener configurada el rasgo `timeseries` especificando la columna de marca de tiempo de una columna de los datos o en una columna virtual derivada del patrón de ruta de los archivos. Esto puede realizarse a través del SDK de Python o de Azure Machine Learning Studio. Tiene que especificarse una columna que represente una marca de tiempo de "grano fino" para poder agregar el rasgo `timeseries` al conjunto de datos. Si los datos se particionan en la estructura de carpetas con información de hora, como '{AAAA/MM/DD}', puede crear una columna virtual mediante la configuración del patrón de ruta de acceso y establecerla como la marca de tiempo de "grano grueso" para mejorar la importancia de la funcionalidad de serie temporal. 

#### <a name="python-sdk"></a>SDK de Python

El método de la clase de [`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-)[`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) define la columna de marca de tiempo del conjunto de datos. 

```python 
from azureml.core import Workspace, Dataset, Datastore

# get workspace object
ws = Workspace.from_config()

# get datastore object 
dstore = Datastore.get(ws, 'your datastore name')

# specify datastore paths
dstore_paths = [(dstore, 'weather/*/*/*/*/data.parquet')]

# specify partition format
partition_format = 'weather/{state}/{date:yyyy/MM/dd}/data.parquet'

# create the Tabular dataset with 'state' and 'date' as virtual columns 
dset = Dataset.Tabular.from_parquet_files(path=dstore_paths, partition_format=partition_format)

# assign the timestamp attribute to a real or virtual column in the dataset
dset = dset.with_timestamp_columns('date')

# register the dataset as the target dataset
dset = dset.register(ws, 'target')
```

Para obtener un ejemplo completo de cómo usar el rasgo de `timeseries` de conjuntos de datos, vea el [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb) o la [documentación del SDK de conjuntos de datos](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-).

#### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Si crea un conjunto de datos mediante Azure Machine Learning Studio, asegúrese de que la ruta de acceso a los datos contiene información de marca de tiempo, incluya todas las subcarpetas con datos y establezca el formato de la partición. 

En el ejemplo siguiente, se toman todos los datos de la subcarpeta *NoaaIsdFlorida/2019*, y el formato de la partición especifica el año, mes y día de la marca de tiempo. 

[![Formato de partición](./media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

En la configuración de **Esquema**, especifique la columna de marca de tiempo de una columna virtual o real del conjunto de datos especificado:

![Timestamp](./media/how-to-monitor-datasets/timestamp.png)

## <a name="dataset-monitor-settings"></a>Configuración del monitor del conjunto de datos

Una vez creado el conjunto de datos con la configuración de marca de tiempo especificada, está listo para configurar el monitor de conjunto de datos.

Las diversas configuraciones del monitor de conjunto de datos se dividen en tres grupos: **Información básica, Configuración de monitor** y **Configuración de reposición**.

### <a name="basic-info"></a>Información básica

Esta tabla contiene la configuración básica usada para el monitor de conjunto de datos.

| Configuración | Descripción | Sugerencias | Mutable | 
| ------- | ----------- | ---- | ------- | 
| Nombre | Nombre del monitor del conjunto de datos. | | No |
| Conjunto de datos de referencia | Conjunto de datos tabular que se usará como referencia para la comparación del conjunto de datos de destino a lo largo del tiempo. | El conjunto de datos de referencia debe compartir características con el conjunto de datos de destino. Por lo general, la referencia debe establecerse en el conjunto de datos de entrenamiento del modelo o en un segmento del conjunto de datos de destino. | No |
| Conjunto de datos de destino | Conjunto de datos tabular con columna de marca de tiempo especificada, que se analizará para el desfase de datos. | El conjunto de datos de destino debe compartir características con el de referencia y debe ser un conjunto de datos de `timeseries` al que se anexan los nuevos datos. Los datos históricos del conjunto de datos de destino se pueden analizar o se pueden supervisar nuevos datos. | No | 
| Frecuencia | Frecuencia que se usará para programar el trabajo de canalización y analizar los datos históricos si se ejecuta una reposición. Puede ser diario, semanal o mensual. | Ajuste esta opción para incluir un tamaño comparable de los datos en la referencia. | No | 
| Características | Lista de características que se analizarán para el desfase de datos a lo largo del tiempo. | Establézcalas en las características de salida de un modelo para medir el desfase del concepto. No incluya características que se desfasan de forma natural a lo largo del tiempo (mes, año, índice, etc.). Puede reposicionar el monitor de desfase de datos existente después de ajustar la lista de características. | Sí | 
| Destino de proceso | Destino de proceso de Azure Machine Learning para ejecutar los trabajos del monitor de conjunto de datos. | | Sí | 

### <a name="monitor-settings"></a>Configuración del monitor

Esta configuración es para la canalización del monitor de conjunto de datos programado que se creará. 

| Configuración | Descripción | Sugerencias | Mutable | 
| ------- | ----------- | ---- | ------- |
| Habilitar | Habilitar o deshabilitar la programación en la canalización del monitor de conjunto de datos | Deshabilite esta programación para analizar los datos históricos con la configuración de reposición. Se puede habilitar una vez creado el monitor de conjunto de datos. | Sí | 
| Latencia | Tiempo, en horas, para que lleguen los datos en el conjunto de datos. Por ejemplo, si los datos tardan tres días en llegar a la instancia de SQL DB en la que se encapsula el conjunto de datos, establezca la latencia en 72. | No se puede cambiar una vez creado el monitor de conjunto de datos | No | 
| Direcciones de correo | Direcciones de correo para alertas en función de la brecha del umbral de porcentaje del desfase de datos. | Los correos se envían a través de Azure Monitor. | Sí | 
| Umbral | Umbral de porcentaje de desfase de datos para alertas de correo. | Se pueden establecer más alertas y eventos en muchas otras métricas del recurso de Application Insights asociado del área de trabajo. | Sí | 

### <a name="backfill-settings"></a>Configuración de reposición

Estos valores son para ejecutar una reposición en los datos pasados de las métricas de desplazamiento de datos.

| Configuración | Descripción | Sugerencias |
| ------- | ----------- | ---- |
| Fecha de inicio | Fecha de inicio del trabajo de reposición. | | 
| Fecha de finalización | Fecha de finalización del trabajo de reposición. | La fecha de finalización no puede ser superior a 31 * unidades de tiempo de frecuencia desde la fecha de inicio. En un monitor de conjunto de datos existente, las métricas se pueden reposicionar para analizar datos históricos o reemplazar métricas con la configuración actualizada. |

## <a name="create-dataset-monitors"></a>Creación de monitores de conjunto de datos 

Cree monitores de conjunto de datos para detectar y notificar el desfase de datos en un nuevo conjunto de datos con Azure Machine Learning Studio o el SDK de Python. 

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

Para configurar alertas en el monitor de conjunto de datos, el área de trabajo que contiene el conjunto de datos, para el que desea crear un monitor, debe tener capacidades de edición Enterprise. 

Una vez confirmada la funcionalidad del área de trabajo, vaya a la página principal de Studio y elija la pestaña Conjuntos de datos de la izquierda. Elija Monitores de conjuntos de datos.

![Lista de monitores](./media/how-to-monitor-datasets/monitor-list.png)

Haga clic en el botón **+Crear monitor** y continúe con el asistente; para ello, haga clic en **Siguiente**.

![Asistente](./media/how-to-monitor-datasets/wizard.png)

El monitor de conjunto de datos aparecerá en la lista. Selecciónelo para ir a la página de detalles de ese monitor.

### <a name="from-python-sdk"></a>Desde SDK de Python

Consulte la [Documentación de referencia de Python SDK sobre el desfase de datos](/python/api/azureml-datadrift/azureml.datadrift) para obtener información completa. 

En el siguiente ejemplo se muestra cómo crear un monitor de conjunto de datos mediante el SDK de Python

```python
from azureml.core import Workspace, Dataset
from azureml.datadrift import DataDriftDetector
from datetime import datetime

# get the workspace object
ws = Workspace.from_config()

# get the target dataset
dset = Dataset.get_by_name(ws, 'target')

# set the baseline dataset
baseline = target.time_before(datetime(2019, 2, 1))

# set up feature list
features = ['latitude', 'longitude', 'elevation', 'windAngle', 'windSpeed', 'temperature', 'snowDepth', 'stationName', 'countryOrRegion']

# set up data drift detector
monitor = DataDriftDetector.create_from_datasets(ws, 'drift-monitor', baseline, target, 
                                                      compute_target='cpu-cluster', 
                                                      frequency='Week', 
                                                      feature_list=None, 
                                                      drift_threshold=.6, 
                                                      latency=24)

# get data drift detector by name
monitor = DataDriftDetector.get_by_name(ws, 'drift-monitor')

# update data drift detector
monitor = monitor.update(feature_list=features)

# run a backfill for January through May
backfill1 = monitor.backfill(datetime(2019, 1, 1), datetime(2019, 5, 1))

# run a backfill for May through today
backfill1 = monitor.backfill(datetime(2019, 5, 1), datetime.today())

# disable the pipeline schedule for the data drift detector
monitor = monitor.disable_schedule()

# enable the pipeline schedule for the data drift detector
monitor = monitor.enable_schedule()
```

Para ver un ejemplo completo de cómo configurar un conjunto de datos de `timeseries` y el detector de desfase de datos, consulte nuestro [cuaderno de ejemplos](https://aka.ms/datadrift-notebook).

## <a name="understanding-data-drift-results"></a>Reconocimiento de los resultados del desfase de datos

El monitor de datos genera dos grupos de resultados: Información general de los desfases y detalles de las características. En la animación siguiente se muestran los gráficos del monitor de desfase disponibles en función de la característica seleccionada y la métrica. 

![Vídeo de demostración](./media/how-to-monitor-datasets/video.gif)

### <a name="drift-overview"></a>Información general sobre el desfase

La sección **Información general sobre el desfase** contiene información de nivel superior sobre la magnitud del desfase de datos y qué características se deben investigar más. 

| Métrica | Descripción | Sugerencias | 
| ------ | ----------- | ---- | 
| Magnitud del desfase de datos | Se especifica como un porcentaje entre la referencia y el conjunto de datos de destino a lo largo del tiempo. De 0 a 100, donde 0 indica conjuntos de datos idénticos y 100 indica que la capacidad de desfase de datos de Azure Machine Learning puede indicar por completo los dos conjuntos de datos. | Se espera un ruido en el porcentaje exacto medido debido a las técnicas de aprendizaje automático que se usan para generar esta magnitud. | 
| Contribución de desfase por característica | La contribución de cada característica del conjunto de datos de destino a la magnitud de desfase medido. |  Debido al cambio de covariable, no es necesario que la distribución subyacente de una característica cambie para tener una importancia de la característica relativamente alta. | 

La imagen siguiente es un ejemplo de gráficos que se muestran en los resultados de **Información general sobre el desfase** en Azure Machine Learning Studio, lo que resulta de una reposición de [Datos de la superficie integrada de NOAA](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/). Los datos se muestrearon para `stationName contains 'FLORIDA'`, con enero de 2019 como conjunto de datos de referencia y todos los datos de 2019 usados como destino.
 
![Información general sobre el desfase](./media/how-to-monitor-datasets/drift-overview.png)

### <a name="feature-details"></a>Detalles de la característica

La sección **Detalles de las características** contiene información a nivel de características sobre el cambio en la distribución de la característica seleccionada, así como otras estadísticas, a lo largo del tiempo. 

El conjunto de datos de destino también se perfila a lo largo del tiempo. La distancia estadística entre la distribución de la línea base de cada característica se compara con la del conjunto de datos de destino a lo largo del tiempo, que es conceptualmente similar a la magnitud del desfase de datos, con la excepción de que se trata de una característica individual. También están disponibles Mín, Máx y Media. 

En el Azure Machine Learning Studio, si hace clic en un punto de datos del gráfico, la distribución de la característica que se muestra se ajustará en consecuencia. De forma predeterminada, muestra la distribución del conjunto de datos de referencia y la distribución de la ejecución más reciente de la misma característica. 

Estas métricas también se pueden recuperar en el SDK de Python a través del método `get_metrics()` en un objeto `DataDriftDetector`. 

#### <a name="numeric-features"></a>Características numéricas 

Las características numéricas se perfilan en cada ejecución del monitor de conjunto de datos. Los siguientes elementos se exponen en Azure Machine Learning Studio. Se muestra la densidad de probabilidad para la distribución.

| Métrica | Descripción |  
| ------ | ----------- |  
| Distancia de Wasserstein | Cantidad mínima de trabajo para transformar la distribución de la línea base en la distribución de destino. |
| Valor medio | Valor promedio de la característica. |
| Valor mínimo | Valor mínimo de la característica. |
| Valor máximo | Valor máximo de la característica. |

![Detalles numéricos de la función](./media/how-to-monitor-datasets/feature-details.png)

#### <a name="categorical-features"></a>Características de categorías 

Las características numéricas se perfilan en cada ejecución del monitor de conjunto de datos. Los siguientes elementos se exponen en Azure Machine Learning Studio. Se muestra un histograma para la distribución.

| Métrica | Descripción |  
| ------ | ----------- |  
| Distancia euclidiana | Distancia geométrica entre las distribuciones de destino y de línea base. |
| Valores únicos | Número de valores únicos (cardinalidad) de la característica. |


![Categoría de detalles de la función](./media/how-to-monitor-datasets/feature-details2.png)

## <a name="metrics-alerts-and-events"></a>Métricas, alertas y eventos

Las métricas se pueden consultar en el recurso de [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) asociado con el área de trabajo de aprendizaje automático. Lo que da acceso a todas las características de Application Insights, entre las que se incluye la configuración de reglas de alertas personalizadas y grupos de acciones para desencadenar una acción como un correo electrónico/SMS/notificación push/mensaje de voz o una función de Azure. Para más información, consulte la documentación de Application Insights completa. 

Para empezar, vaya a Azure Portal y seleccione la página **Información general** de su área de trabajo.  El recurso de Application Insights asociado está en el extremo derecho:

[![Información general de Azure Portal](./media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

Seleccione Registros (Analytics) en Supervisión, en el panel izquierdo:

![Información general de Application Insights](./media/how-to-monitor-datasets/ai-overview.png)

Las métricas del monitor de conjuntos de datos se almacenan como `customMetrics`. Para verlas, puede escribir y ejecutar una consulta después de configurar un monitor de conjuntos de datos:

[![Consulta de Log Analytics](./media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

Después de identificar las métricas para configurar reglas de alerta, cree una regla:

![Nueva alerta de reglas](./media/how-to-monitor-datasets/alert-rule.png)

Para definir la acción que se debe realizar cuando se cumplan las condiciones establecidas, puede usar un grupo de acciones existente, o bien crear uno:

![Grupo de acciones nuevo](./media/how-to-monitor-datasets/action-group.png)

## <a name="troubleshooting"></a>Solución de problemas

Limitaciones y problemas conocidos:

* El intervalo de tiempo de los trabajos de reposición se limita a 31 intervalos de la configuración de frecuencia del monitor. 
* Limitación de 200 características, a menos que no se especifique ninguna lista de características (se usan todas las características).
* El tamaño de proceso debe ser lo suficientemente grande como para controlar los datos. 
* Asegúrese de que el conjunto de datos tiene datos dentro de las fechas de inicio y finalización de una ejecución de monitor determinada.
* Los monitores del conjunto de datos solo funcionarán en conjuntos de datos que contengan 50 filas, o más. 

Las columnas, o características, del conjunto de datos se clasifican como categóricas o numéricas en función de las condiciones de la tabla siguiente. Si la característica no cumple estas condiciones, por ejemplo, una columna de tipo cadena con > 100 valores únicos, la característica se quita de nuestro algoritmo de desfase de datos, pero se perfila aún así. 

| Tipo de característica | Tipo de datos | Condición | Limitaciones | 
| ------------ | --------- | --------- | ----------- |
| Categorías | string, bool, int, float | El número de valores únicos de la característica es menor que 100 y menor que el 5 % del número de filas. | NULL se trata como su propia categoría. | 
| Numérico | int, float | Los valores de la característica son de un tipo de datos numérico y no cumplen la condición de una característica de categoría. | Característica quitada si más del 15 % de los valores son NULL. | 

## <a name="next-steps"></a>Pasos siguientes

* Vaya a [Azure Machine Learning Studio](https://ml.azure.com) o al [cuaderno de Python](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datadrift-tutorial/datadrift-tutorial.ipynb) para configurar un monitor de conjunto de datos.
* Vea cómo configurar un desfase de datos en los [modelos implementados en Azure Kubernetes Service](how-to-monitor-data-drift.md).
* Configure los monitores de desfase de un conjunto de datos con [Event Grid](how-to-use-event-grid.md). 
