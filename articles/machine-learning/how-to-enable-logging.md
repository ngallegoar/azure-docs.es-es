---
title: Habilitación del registro en Azure Machine Learning
description: Aprenda a habilitar el registro en Azure Machine Learning mediante el paquete de registros de Python predeterminado y la funcionalidad específica del SDK.
ms.author: trbye
author: trevorbye
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye
ms.date: 03/05/2020
ms.openlocfilehash: 73b9ae6bc3c15526bfdafd74330c7b86286631b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78396150"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Habilitación del registro en Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

El SDK de Python de Azure Machine Learning le permite habilitar el registro mediante el paquete de registros de Python predeterminado y mediante la funcionalidad específica del SDK, en el registro local y en el registro en el área de trabajo del portal. Los registros proporcionan a los desarrolladores información en tiempo real sobre el estado de la aplicación y pueden ayudarles a diagnosticar los errores o advertencias. En este artículo aprenderá diferentes formas de habilitar registros en las siguientes áreas:

> [!div class="checklist"]
> * Entrenamiento de modelos y destinos de proceso
> * Creación de imágenes
> * Modelos implementados
> * Configuración de `logging` en Python

[Cree un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md). Use la [guía](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) para más información sobre el SDK.

## <a name="training-models-and-compute-target-logging"></a>Entrenamiento de modelos y registro de destinos de proceso

Hay varias maneras de habilitar el registro durante el proceso de entrenamiento de modelos y los ejemplos que aparecen muestran patrones de diseño habituales. Puede registrar fácilmente datos relacionados con la ejecución en el área de trabajo en la nube mediante la función `start_logging` de la clase `Experiment`.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Consulte la documentación de referencia de la clase [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) para conocer funciones de registro adicionales.

Para habilitar el registro local del estado de la aplicación durante el transcurso del entrenamiento, use el parámetro `show_output`. Si habilita el registro detallado podrá ver detalles del proceso de entrenamiento así como información acerca de los recursos remotos o los destinos de proceso. Use el código siguiente para habilitar el registro tras el envío del experimento.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

También puede usar el mismo parámetro en la función `wait_for_completion` de la ejecución resultante.

```python
run.wait_for_completion(show_output=True)
```

El SDK también admite el uso del paquete de registros de Python predeterminado en determinados escenarios de entrenamiento. En el ejemplo siguiente se habilita un nivel de registro de `INFO` en un objeto `AutoMLConfig`.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

También puede usar el parámetro `show_output` al crear un destino de proceso persistente. Especifique el parámetro en la función `wait_for_completion` para habilitar el registro durante la creación del destino de proceso.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Registro para modelos implementados

Para recuperar los registros de un servicio web implementado anteriormente, cargue el servicio y use la función `get_logs()`. Los registros pueden contener información detallada sobre los errores que se produjeron durante la implementación.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

También puede registrar seguimientos de la pila personalizados para el servicio web mediante la habilitación de Application Insights, lo cual le permite supervisar las horas de solicitud y respuesta, las frecuencias de errores y las excepciones. Llame a la función `update()` en un servicio web existente para habilitar Application Insights.

```python
service.update(enable_app_insights=True)
```

Para obtener más información, vea [Supervisión y recopilación de datos de los puntos de conexión del servicio web ML](how-to-enable-app-insights.md).

## <a name="python-native-logging-settings"></a>Configuración del registro nativo de Python

Determinados registros del SDK pueden contener un error que le insta a establecer el nivel de registro en DEBUG. Para establecer el nivel de registro, agregue el código siguiente al script.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión y recopilación de datos de los puntos de conexión del servicio web ML](how-to-enable-app-insights.md)