---
title: Detección del desfase de datos en implementaciones de AKS
titleSuffix: Azure Machine Learning
description: Detecte el desfase de datos (versión preliminar) en modelos implementados de Azure Kubernetes Service en Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 11/04/2019
ms.openlocfilehash: 0f56ab853983ebf9b3e27f38ae1737c0c2bce4ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84430297"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service-aks"></a>Detección del desfase de datos (versión preliminar) en modelos implementados en Azure Kubernetes Service (AKS)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

En este artículo, aprenderá a supervisar el desfase de datos entre el conjunto de datos de entrenamiento y los datos de inferencia de un modelo implementado. En el contexto del aprendizaje automático, los modelos de aprendizaje automático entrenados pueden experimentar un rendimiento de predicción degradado por el desfase. Con Azure Machine Learning, puede supervisar el desfase de datos y el servicio enviará una alerta de correo electrónico en cuanto lo detecte.

## <a name="what-is-data-drift"></a>¿Qué es el desfase de datos?

En el contexto de aprendizaje automático, el desfase de datos es el cambio en los datos de entrada del modelo que conduce a la degradación del rendimiento del modelo. Es uno de los principales motivos por los que la precisión del modelo se degrada con el tiempo. Por lo tanto, la supervisión del desfase de datos ayuda a detectar problemas de rendimiento del modelo. 

## <a name="what-can-i-monitor"></a>¿Qué se puede supervisar?

Con Azure Machine Learning, puede supervisar las entradas en un modelo implementado en AKS y comparar estos datos con el conjunto de datos de entrenamiento del modelo. A intervalos regulares, se realiza una [instantánea y un perfil](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) de los datos de inferencia y, después, estos se calculan con respecto al conjunto de datos de línea base para generar un análisis del desfase de datos con el objeto de: 

+ Medir la magnitud del desfase de datos, denominada "coeficiente de desfase".
+ Medir la contribución al desfase de datos por característica, lo que indica las características que provocan el desfase de datos.
+ Medir las métricas de distancia. Actualmente, las métricas que se calculan son Wasserstein y Distancia energética.
+ Medir las distribuciones de las características. Actualmente, se miden la estimación de la densidad del kernel y los histogramas.
+ Enviar alertas del desfase de datos por correo electrónico.

> [!Note]
> Este servicio está en versión preliminar y sus opciones de configuración están limitadas. Consulte la [documentación de API](https://docs.microsoft.com/python/api/azureml-datadrift/) y las [notas de la versión](azure-machine-learning-release-notes.md) para obtener detalles y actualizaciones. 

### <a name="how-data-drift-is-monitored-in-azure-machine-learning"></a>Cómo se supervisa el desfase de datos en Azure Machine Learning

Al usar Azure Machine Learning, el desfase de datos se supervisa a través de conjuntos de datos o implementaciones. Para supervisar el desfase de datos, se especifica un conjunto de datos de base de referencia, normalmente el conjunto de datos de entrenamiento para un modelo. Un segundo conjunto de datos (normalmente los datos de entrada de modelo recopilados de una implementación) se prueba en el conjunto de datos de base de referencia. Se han generado perfiles para ambos conjuntos de datos y se han especificado en el servicio de supervisión del desfase de datos. Un modelo de Machine Learning se entrena para detectar diferencias entre los dos conjuntos de datos. El rendimiento del modelo se convierte en el coeficiente de desfase, que mide la magnitud del desfase entre los dos conjuntos de datos. Al usar la [interpretabilidad del modelo](how-to-machine-learning-interpretability.md), se calculan las características que han contribuido al coeficiente de desfase. En el perfil del conjunto de datos, se realiza un seguimiento de la información estadística sobre cada característica. 

## <a name="prerequisites"></a>Prerequisites

- Suscripción a Azure. Si no tiene una, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

- El SDK de Azure Machine Learning para Python instalado. Siga las instrucciones que se indican en el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) para hacer lo siguiente:

    - Crear un entorno Miniconda
    - Instalar el SDK de Azure Machine Learning para Python

- Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

- Un [archivo de configuración](how-to-configure-environment.md#workspace) del área de trabajo.

- Instale el SDK de desfase de datos con el comando siguiente:

    ```shell
    pip install azureml-datadrift
    ```

- Cree un [conjunto de datos](how-to-create-register-datasets.md) a partir de los datos de entrenamiento del modelo.

- Especifique el conjunto de datos de entrenamiento al [registrar](concept-model-management-and-deployment.md) el modelo. En el ejemplo siguiente se muestra cómo se usa el parámetro `datasets` para especificar el conjunto de datos de entrenamiento:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- [Habilite la recopilación de datos de modelos](how-to-enable-data-collection.md) para recopilar datos de la implementación de AKS del modelo y confirmar que los datos se recopilan en el contenedor de blobs `modeldata`.

## <a name="configure-data-drift"></a>Configuración del desfase de datos
Para configurar el desfase de datos para el experimento, importe las dependencias como se muestra en el siguiente ejemplo de Python. 

En este ejemplo se muestra cómo se configura el objeto [`DataDriftDetector`](/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector):

```python
# Import Azure ML packages
from azureml.core import Experiment, Run, RunDetails
from azureml.datadrift import DataDriftDetector, AlertConfiguration

# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DataDriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

## <a name="submit-a-datadriftdetector-run"></a>Envío de una ejecución DataDriftDetector

Con el objeto `DataDriftDetector` configurado, puede enviar una [ejecución de desfase de datos](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#run-target-date--services-none--compute-target-none--create-compute-target-false--feature-list-none--drift-threshold-none-) en una fecha determinada para el modelo. Como parte de la ejecución, habilite las alertas de DataDriftDetector mediante el establecimiento del parámetro `drift_threshold`. Si [datadrift_coefficient](#visualize-drift-metrics) es superior al valor de `drift_threshold` especificado, se envía un correo electrónico.

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run.id)
RunDetails(dd_run).show()
```

## <a name="visualize-drift-metrics"></a>Visualización de las métricas de desfase

<a name="metrics"></a>

Después de enviar la ejecución de DataDriftDetector, podrá ver las métricas de desfase que se guardan en cada iteración de ejecución de una tarea de desfase de datos:


|Métrica|Descripción|
--|--|
wasserstein_distance|Distancia estadística definida para la distribución numérica unidimensional.|
energy_distance|Distancia estadística definida para la distribución numérica unidimensional.|
datadrift_coefficient|Se calcula del mismo modo que el coeficiente de correlación de Matthew, pero esta salida es un número real comprendido entre 0 y 1. En el contexto de desfase, 0 indica que no hay desfase y 1 indica desfase máximo.|
datadrift_contribution|Importancia de las características que contribuyen al desfase.|

Existen varias formas de ver las métricas de desfase:

* Usar el `RunDetails`[widget de Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py).
* Usar la función [`get_metrics()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#get-metrics-name-none--recursive-false--run-type-none--populate-false-) en cualquier objeto de ejecución de `datadrift`.
* Vea las métricas en la sección **Modelos** de su área de trabajo en [Azure Machine Learning Studio](https://ml.azure.com).

En el siguiente ejemplo de Python se muestra cómo trazar métricas pertinentes para el desfase de datos. Puede usar las métricas devueltas para crear visualizaciones personalizadas:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per service.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Visualización del desfase de datos detectado por Azure Machine Learning](./media/how-to-monitor-data-drift/drift_show.png)


## <a name="schedule-data-drift-scans"></a>Programación de exámenes de desfase de datos 

Al habilitar la detección del desfase de datos, se ejecuta un objeto DataDriftDetector con la frecuencia especificada y programada. Si datadrift_coefficient alcanza el valor de `drift_threshold` determinado, se envía un correo electrónico con cada ejecución programada. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

La configuración del detector del desfase de datos se puede ver en **Modelos** en la pestaña **Detalles** del área de trabajo en [Azure Machine Learning Studio](https://ml.azure.com).

[![Desfase de datos de Azure Machine Learning Studio](./media/how-to-monitor-data-drift/drift-config.png)](media/how-to-monitor-data-drift/drift-config-expanded.png)

## <a name="view-results-in-your-azure-machine-learning-studio"></a>Visualización de los resultados en Azure Machine Learning Studio

Para ver los resultados en el área de trabajo en [Azure Machine Learning Studio](https://ml.azure.com), vaya a la página del modelo. En la pestaña Detalles del modelo, se muestra la configuración del desfase de datos. Actualmente, está disponible la pestaña **Data Drift** (Desfase de datos) con las métricas del desfase de datos. 

[![Desfase de datos de Azure Machine Learning Studio](./media/how-to-monitor-data-drift/drift-ui.png)](media/how-to-monitor-data-drift/drift-ui-expanded.png)

## <a name="receiving-drift-alerts"></a>Recepción de alertas del desfase

Si establece el umbral de alerta del coeficiente de desfase y proporciona una dirección de correo electrónico, se enviará automáticamente por correo electrónico una alerta de [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) cada vez que el coeficiente de desfase supere el umbral. 

Para que pueda configurar acciones y alertas personalizadas, todas las métricas del desfase de datos se almacenan en el recurso de [Application Insights](how-to-enable-app-insights.md) que se creó con el área de trabajo de Azure Machine Learning. Puede seguir el vínculo incluido en el correo electrónico de la alerta para ver la consulta de Application Insights.

![Correo electrónico de la alerta del desfase de datos](./media/how-to-monitor-data-drift/drift_email.png)

## <a name="retrain-your-model-after-drift"></a>Nuevo entrenamiento del modelo después del desfase

Cuando el desfase de datos afecta negativamente al rendimiento del modelo implementado, es el momento de volver a entrenar el modelo. Para ello, siga los pasos que se indican a continuación.

* Investigue los datos recopilados y prepare los datos para entrenar el nuevo modelo.
* Divídalos en datos de entrenamiento y de prueba.
* Entrene el modelo de nuevo con los nuevos datos.
* Evalúe el rendimiento del modelo recién generado.
* Implemente un nuevo modelo si el rendimiento es mejor que el modelo de producción.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener un ejemplo completo del uso del desfase de datos, consulte el [cuaderno de desfase de datos de Azure ML](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/monitor-models/data-drift/drift-on-aks.ipynb). En este cuaderno de Jupyter Notebook se muestra cómo se usa una instancia de [Azure Open Datasets](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) con el objetivo de entrenar un modelo para predecir el tiempo, implementarlo en AKS y supervisar el desfase de datos. 

* Detecte el desfase de datos con [monitores de conjuntos de datos](how-to-monitor-datasets.md).

* Nos encantaría que nos hiciera llegar sus preguntas, comentarios o sugerencias a medida que el desfase de datos evoluciona hacia la disponibilidad general. Use el botón de comentarios sobre el producto que aparece más abajo. 
