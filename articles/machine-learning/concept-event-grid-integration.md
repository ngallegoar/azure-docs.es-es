---
title: Consumo de eventos de Azure Machine Learning
titleSuffix: Azure Machine Learning
description: En este artículo, aprenderá a usar Azure Event Grid para suscribirse, reaccionar y cancelar la suscripción a eventos generados por Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: andyxu
author: gogowings
ms.date: 11/04/2019
ms.openlocfilehash: d8a975487c68a21b2c8b6fa2f07d86c312243f12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139052"
---
# <a name="consume-azure-machine-learning-events-preview"></a>Consumo de eventos de Azure Machine Learning (versión preliminar)

Azure Machine Learning administra todo el ciclo de vida del proceso de aprendizaje automático, incluido el entrenamiento del modelo, la implementación de modelo y la supervisión. Los eventos de Azure Machine Learning permiten a las aplicaciones reaccionar a los eventos durante el ciclo de vida de aprendizaje automático, como la finalización de las ejecuciones de entrenamiento, el registro y la implementación de modelos, y la detección de desfases de datos, mediante el uso de arquitecturas modernas sin servidor. 

Estos eventos se publican a través de [Azure Event Grid](https://azure.microsoft.com/services/event-grid/). Con Azure Portal, PowerShell o la CLI de Azure, los clientes pueden suscribir eventos fácilmente [especificando uno o varios tipos de eventos y las condiciones de filtrado](/azure/event-grid/event-filtering). Los clientes también tienen la opción de compilar una amplia gama de controladores de eventos tales como Azure Functions, Azure Logic Apps o webhooks genéricos. Azure Machine Learning, junto con Azure Event Grid, ofrece una plataforma de entrega de eventos de alta disponibilidad, confiable y con tolerancia a errores para que se compilen aplicaciones controladas por eventos.

Para obtener información sobre el uso de Azure Event Grid con Azure Machine Learning, consulte [Creación de flujos de trabajo de aprendizaje automático controlados por eventos (versión preliminar)](how-to-use-event-grid.md).

## <a name="the-event-model"></a>Modelo de evento 

Azure Event Grid le eventos de orígenes tales como Azure Machine Learning y otros servicios de Azure. Estos eventos se envían después a controladores de eventos tales como Azure Event Hubs, Azure Functions, Logic Apps y otros. En el diagrama siguiente se muestra cómo se conectan los orígenes y los controladores en Event Grid, pero no proporciona una lista completa de las integraciones admitidas.

![Modelo funcional de Azure Event Grid](./media/concept-event-grid-integration/azure-event-grid-functional-model.png)

Para obtener más información sobre los orígenes de eventos y los controladores de eventos, consulte [¿Qué es Event Grid?](/azure/event-grid/overview).

## <a name="azure-machine-learning-event-types"></a>Tipos de eventos de Azure Machine Learning

Azure Machine Learning proporciona eventos en los distintos puntos del ciclo de vida de aprendizaje automático: 

| Tipo de evento | Descripción |
| ---------- | ----------- |
| `Microsoft.MachineLearningServices.RunCompleted` | Se genera cuando se completa la ejecución de un experimento de aprendizaje automático. |
| `Microsoft.MachineLearningServices.ModelRegistered` | Se genera cuando se registra un modelo de aprendizaje automático en el área de trabajo |
| `Microsoft.MachineLearningServices.ModelDeployed` | Se genera cuando se completa una implementación de servicio de inferencia con uno o varios modelos. |
| `Microsoft.MachineLearningServices.DatasetDriftDetected` | Se genera cuando se completa un trabajo de detección de desfase de datos para dos conjuntos de datos |
| `Microsoft.MachineLearningServices.RunStatusChanged` | Se genera cuando cambia un estado de ejecución, que actualmente solo se produce cuando un estado de ejecución es "incorrecto". |

## <a name="subscribe-to-machine-learning-events"></a>Suscripción a eventos de Machine Learning

Las suscripciones para eventos de Azure Machine Learning están protegidas por el control de acceso basado en rol (RBAC). Solo un [colaborador o propietario](how-to-assign-roles.md#default-roles) de un área de trabajo puede crear, actualizar y eliminar suscripciones a eventos.

Las suscripciones a eventos se pueden filtrar en función de una serie de condiciones. Los filtros pueden aplicarse a las suscripciones de eventos, ya sea durante la [creación](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) de la suscripción de eventos o [en un momento posterior](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest). 

### <a name="filter-by-event-type"></a>Filtrar por tipo de evento
Una suscripción de eventos puede especificar uno o varios tipos de evento de Azure Machine Learning.

### <a name="filter-by-event-subject"></a>Filtrado por asunto del evento
Azure Event Grid admite filtros de asunto en función de las coincidencias de __comienza por__ y __termina en__, con el fin de que los eventos con un asunto coincidente se entreguen al suscriptor. Distintos eventos de aprendizaje automático tienen un formato de asunto diferente.

| Tipo de evento | Formato de asunto | Asunto de ejemplo |
| ---------- | ----------- | ----------- |
| `Microsoft.MachineLearningServices.RunCompleted` | `experiments/{ExperimentId}/runs/{RunId}` | `experiments/b1d7966c-f73a-4c68-b846-992ace89551f/runs/my_exp1_1554835758_38dbaa94` |
| `Microsoft.MachineLearningServices.ModelRegistered` | `models/{modelName}:{modelVersion}` | `models/sklearn_regression_model:3` |
| `Microsoft.MachineLearningServices.ModelDeployed` | `endpoints/{serviceId}` | `endpoints/my_sklearn_aks` |
| `Microsoft.MachineLearningServices.DatasetDriftDetected` | `datadrift/{data.DataDriftId}/run/{data.RunId}` | `datadrift/4e694bf5-712e-4e40-b06a-d2a2755212d4/run/my_driftrun1_1550564444_fbbcdc0f` |
| `Microsoft.MachineLearningServices.RunStatusChanged` | `experiments/{ExperimentId}/runs/{RunId}` | `experiments/b1d7966c-f73a-4c68-b846-992ace89551f/runs/my_exp1_1554835758_38dbaa94` | 

### <a name="advanced-filtering"></a>Filtrado avanzado

Azure Event Grid también admite el filtrado avanzado basado en el esquema de eventos publicados. Los detalles del esquema de eventos de Azure Machine Learning se pueden encontrar en [Esquema de eventos de Azure Event Grid para Azure Machine Learning](../event-grid/event-schema-machine-learning.md).

Entre los filtrados avanzados de ejemplo que puede realizar se incluyen:

* En el caso del evento `Microsoft.MachineLearningServices.ModelRegistered`, filtrar el valor de etiqueta del modelo:

    ```
    --advanced-filter data.ModelTags.key1 StringIn ('value1')
    ```

Para más información sobre cómo aplicar filtros, consulte el artículo de [Filtrado de eventos para Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

## <a name="consume-machine-learning-events"></a>Consumo de eventos de Machine Learning

Las aplicaciones que controlan los eventos de Machine Learning deben seguir algunos procedimientos recomendados:

> [!div class="checklist"]
> * Dado que se pueden configurar varias suscripciones para enrutar eventos al mismo controlador, es importante no asumir que los eventos provienen de un origen determinado, sino comprobar el tema del mensaje para asegurarse de que proviene del área de trabajo de aprendizaje automático prevista.
> * De igual forma, compruebe que eventType es uno de los que está preparado para procesar y no asuma que todos los eventos que reciba van a ser los tipos que espera.
> * Dado que los mensajes pueden llegar desordenados y con cierto retraso, utilice los campos de etag para saber si la información acerca de los objetos sigue estando actualizada.  Además, utilice los campos del secuenciador para conocer el orden de los eventos en cualquier objeto determinado.
> * Ignore los campos que no comprenda. Esta práctica le ayudará a mantenerse resistente a las nuevas características que puedan agregarse en el futuro.
> * Las operaciones de Azure Machine Learning con errores o canceladas no desencadenarán un evento. Por ejemplo, si se produce un error en la implementación de un modelo, no se desencadenará Microsoft.MachineLearningServices.ModelDeployed. Tenga en cuenta este modo de error al diseñar las aplicaciones. Siempre puede usar el SDK de Azure Machine Learning, la CLI o el portal para comprobar el estado de una operación y comprender los motivos de error detallados.

Azure Event Grid permite a los clientes compilar controladores de mensajes desacoplados, que se pueden desencadenar mediante eventos de Azure Machine Learning. Algunos ejemplos importantes de controladores de mensajes son:
* Azure Functions
* Azure Logic Apps
* Azure Event Hubs
* Canalización de Azure Data Factory
* Webhooks genéricos, que se pueden hospedar en la plataforma Azure o en otro lugar

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Event Grid y pruebe los eventos de Azure Machine Learning:

- [Una introducción a Azure Event Grid](../event-grid/overview.md)
- [Esquema de eventos de Azure Event Grid para Azure Machine Learning](../event-grid/event-schema-machine-learning.md)
- [Creación de flujos de trabajo controlados por eventos con Azure Machine Learning](how-to-use-event-grid.md)
