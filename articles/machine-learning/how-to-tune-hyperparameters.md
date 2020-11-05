---
title: Ajuste de los hiperparámetros de un modelo
titleSuffix: Azure Machine Learning
description: Ajuste de forma eficaz los hiperparámetros de sus modelos de aprendizaje profundo o aprendizaje automático mediante Azure Machine Learning.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.date: 03/30/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: b65553edf61d8e646e7238a5385656fe75a3156f
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93311378"
---
# <a name="tune-hyperparameters-for-your-model-with-azure-machine-learning"></a>Ajuste de los hiperparámetros de un modelo mediante Azure Machine Learning


Automatice el ajuste eficaz de hiperparámetros con [paquetes HyperDrive](/python/api/azureml-train-core/azureml.train.hyperdrive?preserve-view=true&view=azure-ml-py) de Azure Machine Learning. Obtenga información sobre cómo completar los pasos necesarios para ajustar los hiperparámetros con el [SDK de Azure Machine Learning](/python/api/overview/azure/ml/?preserve-view=true&view=azure-ml-py):

1. Definir el espacio de búsqueda de parámetros
1. Especificar una métrica principal para optimizar  
1. Especificar la directiva de terminación anticipada para series de bajo rendimiento
1. Asignación de recursos
1. Iniciar un experimento con la configuración definida
1. Visualizar las series de entrenamiento
1. Seleccionar la mejor configuración para un modelo

## <a name="what-are-hyperparameters"></a>¿Qué son los hiperparámetros?

Los **hiperparámetros** son parámetros ajustables que permiten controlar el proceso de entrenamiento de un modelo. Por ejemplo, con redes neuronales, puede decidir el número de capas ocultas y el número de nodos de cada capa. El rendimiento de un modelo depende en gran medida de los hiperparámetros.

 El **ajuste de hiperparámetros** es el proceso de encontrar la configuración de hiperparámetros que produzca el mejor rendimiento. Normalmente, el proceso es manual y costoso desde el punto de vista computacional.

Azure Machine Learning permite automatizar el ajuste de hiperparámetros y ejecutar experimentos en paralelo para optimizar los hiperparámetros de forma eficaz.


## <a name="define-the-search-space"></a>Definición del espacio de búsqueda

Ajuste automáticamente los hiperparámetros explorando el rango de valores definidos para cada hiperparámetro.

Los hiperparámetros pueden ser discretos o continuos, y tienen una distribución de valores que se describe mediante una [expresión de parámetro](/python/api/azureml-train-core/azureml.train.hyperdrive.parameter_expressions?preserve-view=true&view=azure-ml-py).

### <a name="discrete-hyperparameters"></a>Hiperparámetros discretos 

Los hiperparámetros discretos se especifican con un objeto `choice` entre valores discretos. `choice` puede ser:

* uno o más valores separados por comas;
* un objeto `range`;
* cualquier objeto `list` arbitrario.


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

En este caso, `batch_size` toma uno de los valores [16, 32, 64, 128] y `number_of_hidden_layers`, uno de los valores [1, 2, 3, 4].

También se pueden especificar los siguientes hiperparámetros discretos avanzados mediante una distribución:

* `quniform(low, high, q)`: devuelve un valor como round(uniform(low, high) / q) * q
* `qloguniform(low, high, q)`: devuelve un valor como round(exp(uniform(low, high)) / q) * q
* `qnormal(mu, sigma, q)`: devuelve un valor como round(normal(mu, sigma) / q) * q
* `qlognormal(mu, sigma, q)`: devuelve un valor como round(exp(normal(mu, sigma)) / q) * q

### <a name="continuous-hyperparameters"></a>Hiperparámetros continuos 

Los hiperparámetros continuos se especifican como una distribución a través de un rango continuo de valores:

* `uniform(low, high)`: devuelve un valor distribuido uniformemente entre bajo y alto.
* `loguniform(low, high)`: devuelve un valor que se extrae según exp(uniform(low, high)) de forma que el logaritmo del valor devuelto se distribuye uniformemente.
* `normal(mu, sigma)`: devuelve un valor real que se distribuye normalmente con media mu y desviación estándar sigma.
* `lognormal(mu, sigma)`: devuelve un valor extraído según exp(normal(mu, sigma)) de forma que el logaritmo del valor devuelto se distribuye normalmente.

El siguiente es un ejemplo de definición de espacio de parámetros:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Este código define un espacio de búsqueda con dos parámetros: `learning_rate` y `keep_probability`. `learning_rate` tiene una distribución normal con un valor medio de 10 y una desviación estándar de 3. `keep_probability` tiene una distribución uniforme con un valor mínimo de 0,05 y un valor máximo de 0,1.

### <a name="sampling-the-hyperparameter-space"></a>Muestreo del espacio de hiperparámetros

Especifique el método de muestreo de parámetros que se usará en el espacio de hiperparámetros. Azure Machine Learning es compatible con los siguientes métodos:

* Muestreo aleatorio
* Muestreo de cuadrícula
* Muestreo bayesiano

#### <a name="random-sampling"></a>Muestreo aleatorio

El [muestreo aleatorio](/python/api/azureml-train-core/azureml.train.hyperdrive.randomparametersampling?preserve-view=true&view=azure-ml-py) admite hiperparámetros discretos y continuos. Admite la terminación anticipada de las series de bajo rendimiento. Algunos usuarios realizan una búsqueda inicial con muestreo aleatorio y luego restringen el espacio de búsqueda para mejorar los resultados.

En el muestreo aleatorio, los valores de hiperparámetro se seleccionan aleatoriamente del espacio de búsqueda definido. 

```Python
from azureml.train.hyperdrive import RandomParameterSampling
from azureml.train.hyperdrive import normal, uniform, choice
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Muestreo de cuadrícula

El [muestreo de cuadrícula](/python/api/azureml-train-core/azureml.train.hyperdrive.gridparametersampling?preserve-view=true&view=azure-ml-py) admite hiperparámetros discretos. Use el muestreo de cuadrícula si su presupuesto le permite buscar en el espacio de búsqueda de manera exhaustiva. Admite la terminación anticipada de las series de bajo rendimiento.

Realiza una búsqueda de cuadrícula simple de todos los valores posibles. El muestreo de cuadrícula solo se puede usar con hiperparámetros de `choice`. Por ejemplo, el siguiente espacio tiene seis muestras:

```Python
from azureml.train.hyperdrive import GridParameterSampling
from azureml.train.hyperdrive import choice
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Muestreo bayesiano

El [muestreo bayesiano](/python/api/azureml-train-core/azureml.train.hyperdrive.bayesianparametersampling?preserve-view=true&view=azure-ml-py) se basa en el algoritmo de optimización bayesiano. Este escoge muestras en función de cómo funcionaron las muestras anteriores, para que las nuevas mejoren la métrica principal.

Se recomienda el muestreo bayesiano si tiene suficiente presupuesto para explorar el espacio de hiperparámetros. Para obtener los mejores resultados, se recomienda que el número máximo de series sea mayor o igual que 20 veces el número de hiperparámetros que se está optimizando. 

El número de series simultáneas afecta a la eficacia del proceso de ajuste. Un menor número de series simultáneas puede provocar una mejor convergencia de muestreo, dado que el menor grado de paralelismo aumenta el número de series que se benefician de las series completadas previamente.

El muestreo bayesiano solo admite las distribuciones `choice`, `uniform` y `quniform` en el espacio de búsqueda.

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
from azureml.train.hyperdrive import uniform, choice
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```



## <a name="specify-primary-metric"></a><a name="specify-primary-metric-to-optimize"></a> Especificación de la métrica principal

Especifique la [métrica principal](/python/api/azureml-train-core/azureml.train.hyperdrive.primarymetricgoal?preserve-view=true&view=azure-ml-py) que quiere que se optimice con el ajuste de hiperparámetros. En cada serie de entrenamiento se evalúa la métrica principal. La directiva de terminación anticipada usa la métrica principal para identificar las series de bajo rendimiento.

Especifique los siguientes atributos para la métrica principal:

* `primary_metric_name`: el nombre de la métrica principal debe coincidir exactamente con el nombre de la métrica registrada por el script de entrenamiento.
* `primary_metric_goal`: puede ser `PrimaryMetricGoal.MAXIMIZE` o `PrimaryMetricGoal.MINIMIZE` y determina si la métrica principal se maximizará o minimizará al evaluar las ejecuciones. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

Esta muestra maximiza la "precisión".

### <a name="log-metrics-for-hyperparameter-tuning"></a><a name="log-metrics-for-hyperparameter-tuning"></a>Registrar métricas para el ajuste de hiperparámetros

El script de entrenamiento del modelo **debe** registrar la métrica principal durante el entrenamiento del modelo para que HyperDrive pueda acceder a ella con el fin de realizar el ajuste de hiperparámetros.

Registre la métrica principal del script de entrenamiento mediante el siguiente fragmento de código de ejemplo:

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

El script de entrenamiento calcula el parámetro `val_accuracy` y lo registra como la métrica principal de "precisión". Cada vez que se registra la métrica, el servicio de ajuste de hiperparámetros la recibe. Será usted quien tenga que determinar la frecuencia de los informes.

Para obtener más información sobre cómo registrar valores en series de entrenamiento de modelos, consulte [Habilitación del registro en series de entrenamiento de Azure ML](how-to-track-experiments.md).

## <a name="specify-early-termination-policy"></a><a name="early-termination"></a> Especificación de una directiva de terminación anticipada

Termine de forma automática las series con un bajo rendimiento con la ayuda de una directiva de terminación anticipada. La terminación anticipada mejora la eficacia computacional.

Puede configurar los siguientes parámetros que controlan cuándo se aplica una directiva:

* `evaluation_interval`: la frecuencia con que se aplica la directiva. Cada vez que el script de entrenamiento registra la métrica principal se considera un intervalo. Por lo tanto, un parámetro `evaluation_interval` de 1 aplicará la directiva cada vez que el script de entrenamiento informe de la métrica principal. Un parámetro `evaluation_interval` de 2 aplicará la directiva las demás veces. Si no se especifica, `evaluation_interval` está establecido como 1 de forma predeterminada.
* `delay_evaluation`: retrasa la primera evaluación de la directiva un número especificado de intervalos. Se trata de un parámetro opcional que permite que todas las configuraciones se ejecuten durante un número mínimo inicial de intervalos, lo que evita la terminación anticipada de series de entrenamiento. Si se especifica, la directiva aplica cada múltiplo de evaluation_interval que sea mayor o igual que delay_evaluation.

Azure Machine Learning admite las siguientes directivas de terminación anticipada:
* [Directiva de bandidos](#bandit-policy)
* [Directiva de mediana de detención](#median-stopping-policy)
* [Directiva de selección de truncamiento](#truncation-selection-policy)
* [Sin directiva de terminación](#no-termination-policy-default)


### <a name="bandit-policy"></a>Directiva de bandidos

La [directiva de bandidos](/python/api/azureml-train-core/azureml.train.hyperdrive.banditpolicy?preserve-view=true&view=azure-ml-py#&preserve-view=truedefinition) es una directiva de terminación basada en el factor de demora o la cantidad de demora y el intervalo de evaluación. Esta directiva termina aquellas series en las que la métrica principal no se encuentre dentro del factor o cantidad de demora especificadas con respecto a la serie con mejor rendimiento.

> [!NOTE]
> El muestreo bayesiano no admite la terminación anticipada. Al usar el muestreo bayesiano, establezca `early_termination_policy = None`.

Especifique los siguientes parámetros de configuración:

* `slack_factor` o `slack_amount`: la demora permitida con respecto a la serie de entrenamientos con el mejor rendimiento. `slack_factor` especifica la demora permitida como una relación. `slack_amount` especifica la demora permitida como una cantidad absoluta, en lugar de una relación.

    Por ejemplo, imagine que se aplica una directiva de bandidos en el intervalo 10. Suponga que la serie con el mejor rendimiento en el intervalo 10 informa de una métrica principal de 0,8 con el objetivo de maximizar esta. Si la directiva se especifica con un parámetro `slack_factor` de 0,2, se terminarán aquellas series de entrenamiento cuya mejor métrica en el intervalo 10 sea inferior a 0,66 (0,8/(1 +`slack_factor`)).
* `evaluation_interval`: la frecuencia con que se aplica la directiva (opcional)
* `delay_evaluation`: retrasa la primera evaluación de la directiva un número especificado de intervalos (opcional)


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

En este ejemplo, se aplica la directiva de terminación anticipada en cada intervalo cuando se notifican las métricas, comenzando en el intervalo de evaluación 5. Cualquier ejecución cuya mejor métrica sea inferior a (1/(1+0,1), o al 91 % de la ejecución con el mejor rendimiento, se terminará.

### <a name="median-stopping-policy"></a>Directiva de mediana de detención

La [mediana de detención](/python/api/azureml-train-core/azureml.train.hyperdrive.medianstoppingpolicy?preserve-view=true&view=azure-ml-py) es una directiva de terminación anticipada basada en la ejecución de valores medios de las métricas principales notificadas por las ejecuciones. Esta directiva calcula los valores medios de ejecución en todas las series de entrenamiento y termina las series cuyos valores de métrica sean peores que la mediana de los valores medios.

Esta directiva toma los parámetros de configuración siguientes:
* `evaluation_interval`: la frecuencia con que se aplica la directiva (parámetro opcional).
* `delay_evaluation`: retrasa la primera evaluación de directiva un número especificado de intervalos (parámetro opcional).


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

En este ejemplo, se aplica la directiva de terminación anticipada en cada intervalo, comenzando en el intervalo de evaluación 5. Una serie se termina en el intervalo 5 si su mejor métrica principal es peor que la mediana de los valores medios de ejecución durante los intervalos en una relación de 1 a 5 en todas las series de entrenamiento.

### <a name="truncation-selection-policy"></a>Directiva de selección de truncamiento

La [selección de truncamiento](/python/api/azureml-train-core/azureml.train.hyperdrive.truncationselectionpolicy?preserve-view=true&view=azure-ml-py) cancela un porcentaje dado de series con el rendimiento más bajo en cada intervalo de evaluación. Las series se comparan mediante la métrica principal. 

Esta directiva toma los parámetros de configuración siguientes:

* `truncation_percentage`: el porcentaje de ejecuciones con el rendimiento más bajo que se terminarán en cada intervalo de evaluación. Un valor entero comprendido entre 1 y 99.
* `evaluation_interval`: la frecuencia con que se aplica la directiva (opcional)
* `delay_evaluation`: retrasa la primera evaluación de la directiva un número especificado de intervalos (opcional)


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

En este ejemplo, se aplica la directiva de terminación anticipada en cada intervalo, comenzando en el intervalo de evaluación 5. Una serie se termina en el intervalo 5 si su rendimiento en este intervalo se encuentra en el 20 % del rendimiento más bajo de todas las series en el intervalo 5.

### <a name="no-termination-policy-default"></a>Sin directiva de terminación (predeterminado)

Si no se especifica ninguna directiva, el servicio de ajuste de hiperparámetros permitirá que todas las series de entrenamiento se ejecuten hasta completarse.

```Python
policy=None
```

### <a name="picking-an-early-termination-policy"></a>Selección de una directiva de terminación anticipada

* Si está buscando una directiva conservadora que proporcione ahorros sin finalizar trabajos prometedores, puede usar una directiva de mediana de detención con `evaluation_interval` en el valor 1 y `delay_evaluation` en el valor 5. Se trata de una configuración conservadora que puede proporcionar unos ahorros de entre un 25 % y un 35 % sin pérdidas de la métrica principal (según nuestros datos de evaluación).
* Si busca un ahorro más agresivo, use la directiva de bandidos con una directiva de selección de truncamiento o demora permisible más estricta con un porcentaje de truncamiento mayor.

## <a name="allocate-resources"></a>Asignación de recursos

Controle el presupuesto de recursos especificando el número máximo de series de entrenamiento.

* `max_total_runs`: el número máximo de series de entrenamiento. Debe ser un entero entre 1 y 1000.
* `max_duration_minutes`: duración máxima en minutos del experimento de ajuste de hiperparámetros (opcional). Se ejecuta después de que se cancele esta duración.

>[!NOTE] 
>Si se especifica `max_total_runs` y `max_duration_minutes`, el experimento de ajuste de hiperparámetros finaliza cuando se alcanza el primero de estos dos umbrales.

Además, especifique el número máximo de series de entrenamientos que se ejecutarán al mismo tiempo durante la búsqueda de ajuste de hiperparámetros.

* `max_concurrent_runs`: número máximo de series que se pueden ejecutar simultáneamente (opcional). Si no se especifica, todas las series se inician en paralelo. Si se especifica, el tiempo de espera debe ser un entero comprendido entre 1 y 100.

>[!NOTE] 
>El número de ejecuciones simultáneas viene determinado por los recursos disponibles en el destino de proceso especificado. Asegúrese de que el destino de proceso tenga los recursos disponibles para la simultaneidad deseada.

```Python
max_total_runs=20,
max_concurrent_runs=4
```

En este código se configura un experimento de ajuste de hiperparámetros para usar un máximo de 20 ejecuciones totales, de forma tal que se ejecutan cuatro configuraciones al mismo tiempo.

## <a name="configure-experiment"></a>Configuración del experimento

Para [configurar el experimento de ajuste de hiperparámetros](/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverunconfig?preserve-view=true&view=azure-ml-py), proporcione lo siguiente:
* El espacio de búsqueda de hiperparámetros definido
* Una directiva de terminación anticipada
* La métrica principal
* Configuración de asignación de recursos
* ScriptRunConfig `src`

ScriptRunConfig es el script de entrenamiento que se ejecutará con los hiperparámetros muestreados. Este define los recursos por trabajo (uno o varios nodos) y el destino de proceso que se va a usar.

> [!NOTE]
>El destino de proceso especificado en `src` debe tener suficientes recursos para satisfacer el nivel de simultaneidad. Para obtener más información sobre ScriptRunConfig, consulte [Configuración de series de entrenamiento](how-to-set-up-training-targets.md).

Configure el experimento de ajuste de hiperparámetros:

```Python
from azureml.train.hyperdrive import HyperDriveConfig
hd_config = HyperDriveConfig(run_config=src,
                             hyperparameter_sampling=param_sampling,
                             policy=early_termination_policy,
                             primary_metric_name="accuracy",
                             primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                             max_total_runs=100,
                             max_concurrent_runs=4)
```

## <a name="submit-experiment"></a>Envío del experimento

Tras definir la configuración de ajuste de hiperparámetros, [envíe el experimento](/python/api/azureml-core/azureml.core.experiment%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truesubmit-config--tags-none----kwargs-):

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hd_config)
```

## <a name="warm-start-your-hyperparameter-tuning-experiment-optional"></a>Inicio en caliente del experimento de ajuste de hiperparámetros (opcional)

La búsqueda de los mejores valores de hiperparámetros para un modelo puede ser un proceso iterativo. Puede reutilizar el conocimiento de las cinco series anteriores para acelerar el ajuste de hiperparámetros.


El inicio en caliente se administra de forma diferente en función del método de muestreo:
- **Muestreo bayesiano** : las pruebas de la serie anterior se usan como conocimiento previo para elegir nuevas muestras y mejorar la métrica principal.
- **Muestreo aleatorio** o **muestreo de cuadrícula** :  la terminación anticipada utiliza el conocimiento de las seriess anteriores para determinar las series de rendimiento deficiente. 

Especifique la lista de series primarias desde las que desea comenzar en caliente.

```Python
from azureml.train.hyperdrive import HyperDriveRun

warmstart_parent_1 = HyperDriveRun(experiment, "warmstart_parent_run_ID_1")
warmstart_parent_2 = HyperDriveRun(experiment, "warmstart_parent_run_ID_2")
warmstart_parents_to_resume_from = [warmstart_parent_1, warmstart_parent_2]
```

Si se cancela un experimento de ajuste de hiperparámetros, puede reanudar las series de entrenamiento desde el último punto de comprobación. Sin embargo, el script de entrenamiento debe administrar la lógica de punto de comprobación.

La serie de entrenamiento debe utilizar la misma configuración de hiperparámetros y tener montadas las carpetas de resultados. El script de entrenamiento debe aceptar el argumento `resume-from`, que contiene los archivos de punto de comprobación o de modelo a partir de los que se reanuda la serie de entrenamiento. Puede reanudar las ejecuciones de entrenamiento individuales mediante el siguiente fragmento de código:

```Python
from azureml.core.run import Run

resume_child_run_1 = Run(experiment, "resume_child_run_ID_1")
resume_child_run_2 = Run(experiment, "resume_child_run_ID_2")
child_runs_to_resume = [resume_child_run_1, resume_child_run_2]
```

Puede configurar el experimento de optimización de hiperparámetros para que se inicie en caliente a partir de un experimento anterior o para reanudar las ejecuciones de entrenamiento individuales usando los parámetros opcionales `resume_from` y `resume_child_runs` de la configuración:

```Python
from azureml.train.hyperdrive import HyperDriveConfig

hd_config = HyperDriveConfig(run_config=src,
                             hyperparameter_sampling=param_sampling,
                             policy=early_termination_policy,
                             resume_from=warmstart_parents_to_resume_from,
                             resume_child_runs=child_runs_to_resume,
                             primary_metric_name="accuracy",
                             primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                             max_total_runs=100,
                             max_concurrent_runs=4)
```

## <a name="visualize-experiment"></a>Visualización del experimento

Use el [widget del cuaderno](/python/api/azureml-widgets/azureml.widgets.rundetails?preserve-view=true&view=azure-ml-py) para visualizar el progreso de las series de entrenamiento. El siguiente fragmento de código visualiza todas las ejecuciones de ajuste de hiperparámetros en un solo lugar, un Jupyter Notebook:

```Python
from azureml.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

En este código se muestra una tabla con detalles sobre las series de entrenamientos de cada una de las configuraciones de hiperparámetros.

![tabla de ajuste de hiperparámetros](./media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

También puede visualizar el rendimiento de cada una de las ejecuciones a medida que progresa el entrenamiento. 

![trazado de ajuste de hiperparámetros](./media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Puede identificar visualmente la correlación entre el rendimiento y los valores de hiperparámetros individuales mediante un trazado de coordenadas paralelas. 

[![coordenadas paralelas de ajuste de hiperparámetros](./media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)](media/how-to-tune-hyperparameters/hyperparameter-tuning-parallel-coordinates-expanded.png)

Puede visualizar todas las series de ajuste de hiperparámetros en el portal web de Azure. Para obtener más información sobre cómo ver un experimento en el portal web, vea [cómo realizar el seguimiento de experimentos](how-to-monitor-view-training-logs.md#view-the-experiment-in-the-web-portal).

## <a name="find-the-best-model"></a>Identificación del mejor modelo

Una vez que se han completado todas las series de ajuste de hiperparámetros, identifique la configuración con el mejor rendimiento y los valores de hiperparámetro:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Cuaderno de ejemplo
Consulte los cuadernos train-hyperparameter-* en esta carpeta:
* [how-to-use-azureml/ml-frameworks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Seguimiento de experimentos](how-to-track-experiments.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)