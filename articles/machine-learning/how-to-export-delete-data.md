---
title: Exportación o eliminación de los datos del área de trabajo
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo exportar o eliminar el área de trabajo con Azure Machine Learning Studio, la CLI, el SDK y las API REST autenticadas.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: lobrien
ms.author: laobri
ms.date: 04/24/2020
ms.custom: seodec18
ms.openlocfilehash: 50234eb356314039b8023f0442207ae0b762676e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82191641"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Exportación o eliminación de los datos del área de trabajo de Machine Learning Service

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En Azure Machine Learning, puede exportar o eliminar los datos del área de trabajo con la API REST autenticada. En este artículo se indica cómo hacerlo.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Control de los datos del área de trabajo

Los datos del producto almacenados por Azure Machine Learning están disponibles para su exportación y eliminación por medio de Azure Machine Learning Studio, la CLI, el SDK y las API REST autenticadas. Se puede acceder a los datos de telemetría mediante el portal de privacidad de Azure. 

En Azure Machine Learning, los datos personales constan de información del usuario de documentos de historial de ejecución y registros de telemetría de algunas interacciones del usuario con el servicio.

## <a name="delete-workspace-data-with-the-rest-api"></a>Eliminación de datos del área de trabajo con la API REST

Para eliminar datos, se pueden realizar las siguientes llamadas API con el verbo HTTP DELETE. Estas llamadas están autorizadas al tener un encabezado `Authorization: Bearer <arm-token>` en la solicitud, donde `<arm-token>` es el token de acceso de AAD del punto de conexión `https://management.core.windows.net/`.  

Para saber cómo obtener este token y llamar a los puntos de conexión de Azure, consulte [Uso de REST para administrar recursos de Machine Learning](how-to-manage-rest.md) y la [documentación de la API de REST de Azure](https://docs.microsoft.com/rest/api/azure/).  

En los ejemplos siguientes, reemplace el texto en {} por los nombres de instancia que determinan el recurso asociado.

### <a name="delete-an-entire-workspace"></a>Eliminación de un área de trabajo completa

Use esta llamada para eliminar un área de trabajo entera.  
> [!WARNING]
> Se eliminará toda la información y el área de trabajo ya no podrá usarse.

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01

### <a name="delete-models"></a>Eliminación de modelos

Use esta llamada para obtener una lista de modelos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01

Los modelos individuales se pueden eliminar con:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01

### <a name="delete-assets"></a>Eliminación de recursos

Use esta llamada para obtener una lista de recursos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01

Los recursos individuales se pueden eliminar con:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01

### <a name="delete-images"></a>Eliminación de imágenes

Use esta llamada para obtener una lista de imágenes y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01

Las imágenes individuales se pueden eliminar con:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01

### <a name="delete-services"></a>Eliminación de servicios

Use esta llamada para obtener una lista de servicios y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01

Los servicios individuales se pueden eliminar con:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01

## <a name="export-service-data-with-the-rest-api"></a>Exportación de datos de servicio con la API REST

Para exportar datos, se pueden realizar las siguientes llamadas API con el verbo HTTP GET. Estas llamadas están autorizadas al tener un encabezado `Authorization: Bearer <arm-token>` en la solicitud, donde `<arm-token>` es el token de acceso de AAD del punto de conexión `https://management.core.windows.net/`.  

Para saber cómo obtener este token y llamar a los puntos de conexión de Azure, consulte [Uso de REST para administrar recursos de Machine Learning](how-to-manage-rest.md) y la [documentación de la API de REST de Azure](https://docs.microsoft.com/rest/api/azure/).   

En los ejemplos siguientes, reemplace el texto en {} por los nombres de instancia que determinan el recurso asociado.

### <a name="export-workspace-information"></a>Exportación de la información del área de trabajo

Use esta llamada para obtener una lista de todas las áreas de trabajo:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2019-11-01

Se puede obtener información sobre un área de trabajo individual mediante:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01

### <a name="export-compute-information"></a>Exportación de la información de proceso

Se pueden obtener todos los destinos de proceso asociados a un área de trabajo mediante:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2019-11-01

Se puede obtener información sobre un único destino de proceso mediante:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2019-11-01

### <a name="export-run-history-data"></a>Exportación de datos del historial de ejecución

Use esta llamada para obtener una lista de todos los experimentos y su información:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments 

Se pueden obtener todas las ejecuciones de un experimento determinado mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs 

Los elementos de historial de ejecución se pueden obtener mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId} 

Se pueden obtener todas las métricas de ejecución de un experimento mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics 

Se puede obtener una sola métrica de ejecución mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}

### <a name="export-artifacts"></a>Exportación de artefactos

Use esta llamada para obtener una lista de artefactos y sus rutas de acceso:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}

### <a name="export-notifications"></a>Exportación de notificaciones

Use esta llamada para obtener una lista de tareas almacenadas:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks

Se pueden obtener las notificaciones de una sola tarea mediante:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}

### <a name="export-data-stores"></a>Exportación de almacenes de datos

Use esta llamada para obtener una lista de almacenes de datos:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores

Se pueden obtener almacenes de datos individuales mediante:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}

### <a name="export-models"></a>Exportación de modelos

Use esta llamada para obtener una lista de modelos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01

Los modelos individuales pueden obtenerse mediante:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01

### <a name="export-assets"></a>Exportación de recursos

Use esta llamada para obtener una lista de recursos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01

Se pueden obtener recursos individuales mediante:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01

### <a name="export-images"></a>Exportación de imágenes

Use esta llamada para obtener una lista de imágenes y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01

Se pueden obtener imágenes individuales mediante:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01

### <a name="export-services"></a>Exportación de servicios

Use esta llamada para obtener una lista de servicios y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01

Los servicios individuales se pueden obtener mediante:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01

### <a name="export-pipeline-experiments"></a>Exportación de experimentos de canalización

Se pueden obtener experimentos individuales mediante:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}

### <a name="export-pipeline-graphs"></a>Exportación de grafos de canalización

Se pueden obtener grafos individuales mediante:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}

### <a name="export-pipeline-modules"></a>Exportación de módulos de canalización

Se pueden obtener módulos mediante:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}

### <a name="export-pipeline-templates"></a>Exportación de plantillas de canalización

Se pueden obtener plantillas mediante:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}

### <a name="export-pipeline-data-sources"></a>Exportación de orígenes de datos de canalización

Se pueden obtener orígenes de datos mediante:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}

## <a name="delete-assets-in-the-designer"></a>Eliminación de recursos en el diseñador

En el diseñador donde se ha creado el experimento, elimine los recursos individuales:

1. Vaya a diseñador.

    ![Eliminación de recursos](./media/how-to-export-delete-data/delete-experiment.png)

1. En la lista, seleccione el borrador de canalización individual que desea eliminar.

1. Seleccione **Eliminar**.

### <a name="delete-datasets-in-the-designer"></a>Eliminación de conjuntos de datos en el diseñador

Para eliminar conjuntos de datos en el diseñador, use Azure Portal o el Explorador de Storage para desplazarse a las cuentas de almacenamiento conectadas y eliminar los conjuntos de ellas. La anulación del registro de los conjuntos de registros en el diseñador solo quita el punto de referencia en el almacenamiento.

