---
title: Creación de flujos de trabajo de aprendizaje automático controlados por eventos
titleSuffix: Azure Machine Learning
description: Aprenda a usar Event Grid con Azure Machine Learning para habilitar soluciones controladas por eventos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shipatel
author: shivp950
ms.reviewer: larryfr
ms.date: 03/11/2020
ms.openlocfilehash: fe6125682f669e453100488b7e0afc4c49409588
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79129731"
---
# <a name="create-event-driven-machine-learning-workflows-preview"></a>Creación de flujos de trabajo de aprendizaje automático controlados por eventos (versión preliminar)

[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/) admite eventos de Azure Machine Learning. Puede suscribirse y consumir eventos como el cambio en el estado de ejecución, la finalización de la ejecución, el registro de modelos, la implementación de modelos y la detección de desfase de datos dentro de un área de trabajo.

Para más información sobre los tipos de eventos, vea [Integración de Azure Machine Learning con Event Grid](concept-event-grid-integration.md) y [Esquema de Event Grid de Azure Machine Learning](/azure/event-grid/event-schema-machine-learning).

Use Event Grid para habilitar escenarios comunes, como:

* Envío de correos electrónicos en caso de error de ejecución y al finalizar la ejecución
* Uso de una función de Azure una vez registrado un modelo
* Streaming de eventos desde Azure Machine Learning a varios puntos de conexión
* Desencadenamiento de una canalización de Machine Learning cuando se detecta un desfase

> [!NOTE] 
> Actualmente, los eventos runStatusChanged solo se desencadenan cuando el estado de ejecución es **failed**
>

## <a name="prerequisites"></a>Prerequisites
* Acceso de colaborador o propietario al área de trabajo de Azure Machine Learning para la que se crearán los eventos.

### <a name="configure-eventgrid-using-the-azure-portal"></a>Configuración de EventGrid mediante Azure Portal

1. Abra [Azure Portal](https://portal.azure.com) y vaya al área de trabajo de Azure Machine Learning.

1. En la barra de la izquierda, seleccione __Eventos__ y después **Suscripciones a eventos**. 

    ![select-events-in-workspace.png](./media/how-to-use-event-grid/select-event.png)

1. Seleccione el tipo de evento que se va a consumir. Por ejemplo, en la captura de pantalla siguiente se ha seleccionado __Modelo registrado__, __Modelo implementado__, __Ejecución completada__ y __Desfase del conjunto de datos detectado__:

    ![add-event-type](./media/how-to-use-event-grid/add-event-type-updated.png)

1. Seleccione el punto de conexión en el que se va a publicar el evento. En la captura de pantalla siguiente, __Centro de eventos__ es el punto de conexión seleccionado:

    ![select-event-handler](./media/how-to-use-event-grid/select-event-handler.png)

Una vez que haya confirmado la selección, haga clic en __Crear__. Después de la configuración, estos eventos se insertarán en el punto de conexión.


### <a name="configure-eventgrid-using-the-cli"></a>Configuración de EventGrid mediante la CLI

Puede instalar la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) más reciente, o bien usar Azure Cloud Shell que se proporciona como parte de la suscripción de Azure.

Para instalar la extensión Event Grid, use el siguiente comando de la CLI:

```azurecli-interactive
az add extension --name eventgrid
```

En el ejemplo siguiente se muestra cómo seleccionar una suscripción de Azure y, después, crear una suscripción a eventos para Azure Machine Learning:

```azurecli-interactive
# Select the Azure subscription that contains the workspace
az account set --subscription "<name or ID of the subscription>"

# Subscribe to the machine learning workspace.
az eventgrid event-subscription create \
  --name {eventGridFilterName} \
  --source-resource-id "/subscriptions/{subId}/resourceGroups/{rgName}/ \providers/Microsoft.MachineLearningServices/workspaces/{wsName}" \
  --endpoint {event handler endpoint} \
  --included-event-types Microsoft.MachineLearningServices.ModelRegistered \
  --subject-begins-with "models/mymodelname"
```

## <a name="filter-events"></a>Filtrado de eventos

Al configurar eventos, puede aplicar filtros para que se desencadenen únicamente con datos de eventos específicos. En el ejemplo siguiente, para eventos de cambio de estado de ejecución, puede filtrar por tipos de ejecución. El evento solo se desencadena cuando se cumplen los criterios. Consulte el [Esquema de Event Grid de Azure Machine Learning](/azure/event-grid/event-schema-machine-learning) para obtener información sobre los datos de eventos que puede filtrar. 

1. Vaya a Azure Portal y seleccione una suscripción nueva o existente. 

1. Seleccione la pestaña de filtros y desplácese hacia abajo hasta los filtros avanzados. En **Clave** y **Valor**, proporcione los tipos de propiedad por los que desee filtrar. Aquí puede ver que el evento solo se desencadenará cuando el tipo de ejecución sea una ejecución de canalización o de paso de canalización.  

    :::image type="content" source="media/how-to-use-event-grid/select-event-filters.png" alt-text="filtrado de eventos":::

## <a name="sample-scenarios"></a>Escenarios de ejemplo

### <a name="use-a-logic-app-to-send-email-alerts"></a>Uso de una aplicación lógica para enviar alertas por correo electrónico

Aproveche [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) para configurar mensajes de correo electrónico para todos los eventos. Personalice con condiciones y especifique destinatarios para permitir la colaboración y el reconocimiento entre los equipos que trabajan juntos.

1. En Azure Portal, vaya al área de trabajo Azure Machine Learning y seleccione la pestaña Eventos en la barra de la izquierda. Desde ahí, seleccione __Aplicaciones lógicas__. 

    ![select-logic-ap](./media/how-to-use-event-grid/select-logic-ap.png)

1. Inicie sesión en la interfaz de usuario de la aplicación lógica y seleccione Machine Learning Service como el tipo de tema. 

    ![select-topic-type](./media/how-to-use-event-grid/select-topic-type.png)

1. Seleccione los eventos que quiere que le notifiquen. Por ejemplo, en la captura de pantalla siguiente, __RunCompleted__.

    ![select-event-runcomplete](./media/how-to-use-event-grid/select-event-runcomplete.png)

1. Puede usar el método de filtrado de la sección anterior o agregar filtros para desencadenar solo la aplicación lógica en un subconjunto de tipos de evento. En la captura de pantalla siguiente, se usa un __filtro de prefijo__ de __/datadriftID/runs/__ .

    ![filter-events](./media/how-to-use-event-grid/filtering-events.png)

1. A continuación, agregue un paso para consumir este evento y buscar el correo electrónico. Hay varias cuentas de correo electrónico que puede usar para recibir eventos. También puede configurar condiciones sobre cuándo enviar una alerta de correo electrónico.

    ![select-email-action](./media/how-to-use-event-grid/select-email-action.png)

1. Seleccione __Enviar un correo electrónico__ y rellene los parámetros. En el asunto, puede incluir el __tipo de evento__ y el __tema__ para ayudar a filtrar los eventos. También puede incluir un vínculo a la página del área de trabajo para las ejecuciones en el cuerpo del mensaje. 

    ![configure-email-body](./media/how-to-use-event-grid/configure-email-body.png)

1. Para guardar esta acción, seleccione **Guardar como** en la esquina izquierda de la página. En la barra de la derecha que aparece, confirme la creación de esta acción.

    ![confirm-logic-app-create](./media/how-to-use-event-grid/confirm-logic-app-create.png)


### <a name="use-a-logic-app-to-trigger-retraining-workflows-when-data-drift-occurs"></a>Uso de una aplicación lógica para desencadenar flujos de trabajo de nuevo entrenamiento cuando se produce el desfase de datos

Los modelos quedan obsoletos a lo largo del tiempo y conservan la utilidad en el contexto en el que se ejecutan. Una manera de saber si es el momento de volver a entrenar el modelo consiste en detectar el desfase de los datos. 

En este ejemplo se muestra cómo usar Event Grid con una aplicación lógica de Azure para desencadenar el nuevo entrenamiento. El ejemplo desencadena una canalización de Azure Data Factory cuando se produce el desfase de datos entre los conjuntos de datos de entrenamiento y de servicio de un modelo.

Antes de empezar, realice las acciones siguientes:

* Configure un monitor de conjunto de datos para [detectar el desfase de datos]( https://aka.ms/datadrift) en un área de trabajo
* Cree una [canalización de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) publicada.

En este ejemplo, se usa una canalización de Data Factory simple para copiar archivos en un almacén de blobs y ejecutar una canalización de Machine Learning publicada. Para más información sobre este escenario, vea cómo configurar un [paso de Machine Learning en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-machine-learning-service)

![adf-mlpipeline-stage](./media/how-to-use-event-grid/adf-mlpipeline-stage.png)

1. Comience con la creación de la aplicación lógica. Vaya a [Azure Portal](https://portal.azure.com), busque Aplicaciones lógicas y seleccione Crear.

    ![search-logic-app](./media/how-to-use-event-grid/search-for-logic-app.png)

1. Rellene la información solicitada. Para simplificar la experiencia, use la misma suscripción y el mismo grupo de recursos de la canalización de Azure Data Factory y el área de trabajo de Azure Machine Learning.

    ![set-up-logic-app-for-adf](./media/how-to-use-event-grid/set-up-logic-app-for-adf.png)

1. Una vez que haya creado la aplicación lógica, seleccione __Cuando se produce un evento de recursos de Event Grid__. 

    ![select-event-grid-trigger](./media/how-to-use-event-grid/select-event-grid-trigger.png)

1. Inicie sesión y rellene los detalles del evento. Establezca el __nombre del recurso__ en el nombre del área de trabajo. Establezca el __tipo de evento__ en __DatasetDriftDetected__.

    ![login-and-add-event](./media/how-to-use-event-grid/login-and-add-event.png)

1. Agregue un nuevo paso y busque __Azure Data Factory__. Seleccione __Crear una ejecución de canalización__. 

    ![create-adfpipeline-run](./media/how-to-use-event-grid/create-adfpipeline-run.png)

1. Inicie sesión y especifique la canalización de Azure Data Factory publicada que se va a ejecutar.

    ![specify-adf-pipeline](./media/how-to-use-event-grid/specify-adf-pipeline.png)

1. Guarde y cree la aplicación lógica con el botón **Guardar** de la parte superior izquierda de la página. Para ver la aplicación, vaya al área de trabajo en [Azure Portal](https://portal.azure.com) y haga clic en **Eventos**.

    ![show-logic-app-webhook](./media/how-to-use-event-grid/show-logic-app-webhook.png)

Ahora la canalización de Data Factory se desencadena cuando se produce el desfase. Vea los detalles de la ejecución de desfase de datos y la canalización de aprendizaje automático en el [nuevo portal de áreas de trabajo](https://ml.azure.com). 

![view-in-workspace](./media/how-to-use-event-grid/view-in-workspace.png)

### <a name="use-azure-functions-to-deploy-a-model-based-on-tags"></a>Uso de Azure Functions para implementar un modelo basado en etiquetas

Un objeto de modelo de Azure Machine Learning contiene parámetros sobre los que puede dinamizar las implementaciones, como el nombre del modelo, la versión, la etiqueta y la propiedad. El evento de registro del modelo puede desencadenar un punto de conexión y puede usar una función de Azure para implementar un modelo en función del valor de esos parámetros.

Para obtener un ejemplo, vea el repositorio [https://github.com/Azure-Samples/MachineLearningSamples-NoCodeDeploymentTriggeredByEventGrid](https://github.com/Azure-Samples/MachineLearningSamples-NoCodeDeploymentTriggeredByEventGrid) y siga los pasos del archivo **Léame**.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre los eventos disponibles, vea el [Esquema de eventos de Azure Machine Learning](/azure/event-grid/event-schema-machine-learning)
