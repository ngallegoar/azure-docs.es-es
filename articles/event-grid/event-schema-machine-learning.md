---
title: Esquema de eventos de Machine Learning de Azure Event Grid
description: Describe las propiedades que se proporcionan para los eventos del área de trabajo de Machine Learning con Azure Event Grid
services: event-grid
author: jenns
ms.service: event-grid
ms.topic: reference
ms.date: 10/18/2019
ms.author: jenns
ms.openlocfilehash: 4051598a9abd787f6707e67a8c4dab12fc6d626a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79202151"
---
# <a name="azure-event-grid-event-schema-for-azure-machine-learning"></a>Esquema de eventos de Azure Event Grid para Azure Machine Learning

En este artículo se proporcionan las propiedades y los eventos del área de trabajo de aprendizaje automático. Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md).

Para obtener una lista de scripts de ejemplo y tutoriales, vea los [orígenes de eventos de AzureML](event-sources.md#azure-machine-learning).

## <a name="available-event-types"></a>Tipos de eventos disponibles

Azure Machine Learning emite los tipos de eventos siguientes:

| Tipo de evento | Descripción |
| ---------- | ----------- |
| Microsoft.MachineLearningServices.ModelRegistered | Se genera cuando se ha registrado correctamente un modelo nuevo o una versión de modelo nueva. |
| Microsoft.MachineLearningServices.ModelDeployed | Se genera cuando los modelos se han implementado correctamente en un punto de conexión. |
| Microsoft.MachineLearningServices.RunCompleted | Se genera cuando una ejecución se ha completado correctamente. |
| Microsoft.MachineLearningServices.DatasetDriftDetected | Se genera cuando un monitor de desfase de datos detecta el desfase. |
| Microsoft.MachineLearningServices.RunStatusChanged | Se genera cuando el estado de ejecución cambia a "Erróneo". |

## <a name="the-contents-of-an-event-response"></a>El contenido de una respuesta de evento

Cuando se desencadena un evento, el servicio Event Grid envía datos sobre ese evento al punto de conexión correspondiente.

Esta sección contiene un ejemplo del aspecto que deben tener los datos para cada evento.

### <a name="microsoftmachinelearningservicesmodelregistered-event"></a>Evento Microsoft.MachineLearningServices.ModelRegistered

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "models/sklearn_regression_model:20",
  "eventType": "Microsoft.MachineLearningServices.ModelRegistered",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ModelName": "sklearn_regression_model",
    "ModelVersion": 20,
    "ModelTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ModelProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesmodeldeployed-event"></a>Evento Microsoft.MachineLearningServices.ModelDeployed

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "endpoints/my-sklearn-service",
  "eventType": "Microsoft.MachineLearningServices.ModelDeployed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ServiceName": "my-sklearn-service",
    "ServiceComputeType": "ACI",
    "ModelIds": "sklearn_regression_model:1,sklearn_regression_model:2",
    "ServiceTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ServiceProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesruncompleted-event"></a>Evento Microsoft.MachineLearningServices.RunCompleted

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesdatasetdriftdetected-event"></a>Evento Microsoft.MachineLearningServices.DatasetDriftDetected

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "datadrifts/{}/runs/{}",
  "eventType": "Microsoft.MachineLearningServices.DatasetDriftDetected",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "DataDriftId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef",
    "DataDriftName": "myDriftMonitor",
    "RunId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef_1571590300380",
    "BaseDatasetId": "3c56d136-0f64-4657-a0e8-5162089a88a3",
    "TargetDatasetId": "d7e74d2e-c972-4266-b5fb-6c9c182d2a74",
    "DriftCoefficient": 0.83503490684792081,
    "StartTime": "2019-07-04T00:00:00+00:00",
    "EndTime": "2019-07-05T00:00:00+00:00"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesrunstatuschanged-event"></a>Evento Microsoft.MachineLearningServices.RunStatusChanged

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    },
   "RunStatus": "failed"
   },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```




## <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| topic | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| subject | string | Ruta al asunto del evento definida por el anunciante. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| datos | object | Datos de eventos de Blob Storage. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

El objeto de datos tiene las siguientes propiedades para cada tipo de evento:

### <a name="microsoftmachinelearningservicesmodelregistered"></a>Microsoft.MachineLearningServices.ModelRegistered

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| ModelName | string | Nombre del modelo que se ha registrado. |
| ModelVersion | string | Versión del modelo que se ha registrado. |
| ModelTags | object | Etiquetas del modelo que se ha registrado. |
| ModelProperties | object | Propiedades del modelo que se ha registrado. |

### <a name="microsoftmachinelearningservicesmodeldeployed"></a>Microsoft.MachineLearningServices.ModelDeployed

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| ServiceName | string | Nombre del servicio implementado. |
| ServiceComputeType | string | Tipo de proceso (por ejemplo, ACI, AKS) del servicio implementado. |
  | ModelIds | string | Lista de identificadores de modelo separados por comas. Identificadores de los modelos implementados en el servicio. |
| ServiceTags | object | Etiquetas del servicio implementado. |
| ServiceProperties | object | Propiedades del servicio implementado. |

### <a name="microsoftmachinelearningservicesruncompleted"></a>Microsoft.MachineLearningServices.RunCompleted

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| ExperimentId | string | Identificador del experimento al que pertenece la ejecución. |
| ExperimentName | string | Nombre del experimento al que pertenece la ejecución. |
| RunId | string | Identificador de la ejecución que se ha completado. |
| RunType | string | Tipo de ejecución de la ejecución completada. |
| RunTags | object | Etiquetas de la ejecución completada. |
| RunProperties | object | Propiedades de la ejecución completada. |

### <a name="microsoftmachinelearningservicesdatasetdriftdetected"></a>Microsoft.MachineLearningServices.DatasetDriftDetected

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| DataDriftId | string | Identificador del monitor de desfase de datos que ha desencadenado el evento. |
| DataDriftName | string | Nombre del monitor de desfase de datos que ha desencadenado el evento. |
| RunId | string | Identificador de la ejecución que ha detectado el desfase de datos. |
| BaseDatasetId | string | Identificador del conjunto de datos base que se ha usado para detectar el desfase de datos. |
| TargetDatasetId | string | Identificador del conjunto de datos de destino que se ha usado para detectar el desfase de datos. |
| DriftCoefficient | double | Resultado del coeficiente que ha desencadenado el evento. |
| StartTime | datetime | Hora de inicio de la serie temporal del conjunto de datos de destino que ha dado como resultado la detección del desfase de datos.  |
| EndTime | datetime | Hora de finalización de la serie temporal del conjunto de datos de destino que ha dado como resultado la detección del desfase de datos. |

### <a name="microsoftmachinelearningservicesrunstatuschanged"></a>Microsoft.MachineLearningServices.RunStatusChanged

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| ExperimentId | string | Identificador del experimento al que pertenece la ejecución. |
| ExperimentName | string | Nombre del experimento al que pertenece la ejecución. |
| RunId | string | Identificador de la ejecución que se ha completado. |
| RunType | string | Tipo de ejecución de la ejecución completada. |
| RunTags | object | Etiquetas de la ejecución completada. |
| RunProperties | object | Propiedades de la ejecución completada. |
| RunStatus | string | Estado de la ejecución. |

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para obtener más información sobre la creación de una suscripción de Azure Event Grid, vea [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
* Para obtener una introducción sobre cómo usar Azure Event Grid con Azure Machine Learning, vea [Consumo de eventos de Azure Machine Learning](/azure/machine-learning/service/concept-event-grid-integration).
* Para obtener un ejemplo de cómo usar Azure Event Grid con Azure Machine Learning, vea [Creación de flujos de trabajo de aprendizaje automático basados en eventos](/azure/machine-learning/service/how-to-use-event-grid).
