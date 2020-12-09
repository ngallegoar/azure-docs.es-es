---
title: Inicio, supervisión y cancelación de las ejecuciones de entrenamiento en Python
titleSuffix: Azure Machine Learning
description: Obtenga información acerca de cómo iniciar, supervisar el estado y administrar las ejecuciones de los experimentos de aprendizaje automático con el SDK de Azure Machine Learning para Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 12/04/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, devx-track-azurecli
ms.openlocfilehash: ec006636ed7e975b696aa32300b32089e3209bb5
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96600479"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Inicio, supervisión y cancelación de las ejecuciones de entrenamiento en Python

El [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py), la [CLI de Machine Learning](reference-azure-machine-learning-cli.md) y [Microsoft Azure Machine Learning Studio](https://ml.azure.com) proporcionan varios métodos para supervisar, organizar y administrar las ejecuciones de entrenamiento y experimentación.

En este artículo se muestran ejemplos de las tareas siguientes:

* Supervisión del rendimiento de la ejecución.
* Cancelación o ejecuciones erróneas.
* Creación de ejecuciones secundarias.
* Etiquetado y búsqueda de ejecuciones.

> [!TIP]
> Si busca información sobre cómo supervisar Azure Machine Learning Service y los servicios de Azure asociados, consulte [Supervisión de Azure Machine Learning](monitor-azure-machine-learning.md).
> Si está buscando información sobre la supervisión de los modelos implementados como servicios web o módulos de IoT Edge, consulte [Recopilación de datos de modelos](how-to-enable-data-collection.md) y [Supervisión con Application Insights](how-to-enable-app-insights.md).

## <a name="prerequisites"></a>Prerrequisitos

Necesitará los siguientes elementos:

* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* El SDK de Azure Machine Learning para Python (versión 1.0.21 o posterior). Para instalar o actualizar a la versión más reciente del SDK, consulte [Instalación o actualización del SDK](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).

    Para comprobar su versión del SDK de Azure Machine Learning, use el siguiente código:

    ```python
    print(azureml.core.VERSION)
    ```

* La [CLI de Azure](/cli/azure/?preserve-view=true&view=azure-cli-latest) y la [extensión de la CLI para Azure Machine Learning](reference-azure-machine-learning-cli.md).

## <a name="monitor-run-performance"></a>Supervisión del rendimiento de la ejecución

* Inicio de una ejecución y su proceso de registro

    # <a name="python"></a>[Python](#tab/python)
    
    1. Para configurar el experimento, importe las clases [Workspace](/python/api/azureml-core/azureml.core.workspace.workspace?preserve-view=true&view=azure-ml-py), [Experiment](/python/api/azureml-core/azureml.core.experiment.experiment?preserve-view=true&view=azure-ml-py), [Run](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py) y [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig?preserve-view=true&view=azure-ml-py) del paquete [azureml.core](/python/api/azureml-core/azureml.core?preserve-view=true&view=azure-ml-py).
    
        ```python
        import azureml.core
        from azureml.core import Workspace, Experiment, Run
        from azureml.core import ScriptRunConfig
        
        ws = Workspace.from_config()
        exp = Experiment(workspace=ws, name="explore-runs")
        ```
    
    1. Inicie una ejecución y su proceso de registro con el método [`start_logging()`](/python/api/azureml-core/azureml.core.experiment%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truestart-logging--args----kwargs-).
    
        ```python
        notebook_run = exp.start_logging()
        notebook_run.log(name="message", value="Hello from run!")
        ```
        
    # <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
    
    Para iniciar una ejecución del experimento, use los pasos siguientes:
    
    1. En el shell o el símbolo del sistema, use la CLI de Azure para autenticarse en la suscripción de Azure:
    
        ```azurecli-interactive
        az login
        ```
        
        [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 
    
    1. Adjunte una configuración de área de trabajo a la carpeta que contiene el script de entrenamiento. Reemplace `myworkspace` por el área de trabajo de Azure Machine Learning. Reemplace `myresourcegroup` por el grupo de recursos de Azure que contiene el área de trabajo:
    
        ```azurecli-interactive
        az ml folder attach -w myworkspace -g myresourcegroup
        ```
    
        Este comando crea un subdirectorio `.azureml` que contiene archivos de entorno de conda y runconfig de ejemplo. También contiene un archivo `config.json` que se usa para comunicarse con el área de trabajo de Azure Machine Learning.
    
        Para obtener más información, consulte [az ml folder attach](/cli/azure/ext/azure-cli-ml/ml/folder?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).
    
    2. Para iniciar la ejecución, use el comando siguiente. Cuando use este comando, especifique el nombre del archivo runconfig (el texto que va antes de \*.runconfig, si mira el sistema de archivo) con respecto al parámetro -c.
    
        ```azurecli-interactive
        az ml run submit-script -c sklearn -e testexperiment train.py
        ```
    
        > [!TIP]
        > El comando `az ml folder attach` ha creado un subdirectorio `.azureml`, que contiene dos archivos runconfig de ejemplo.
        >
        > Si tiene un script de Python que crea un objeto de configuración de ejecución mediante programación, puede usar [RunConfig.save()](/python/api/azureml-core/azureml.core.runconfiguration?preserve-view=true&view=azure-ml-py#&preserve-view=truesave-path-none--name-none--separate-environment-yaml-false-) para guardarlo como un archivo runconfig.
        >
        > Para ver más archivos runconfig de ejemplo, consulte [https://github.com/MicrosoftDocs/pipelines-azureml/](https://github.com/MicrosoftDocs/pipelines-azureml/).
    
        Para obtener más información, consulte [az ml run submit-script](/cli/azure/ext/azure-cli-ml/ml/run?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).
    
    # <a name="studio"></a>[Estudio](#tab/azure-studio)
    
    Para iniciar el envío de una ejecución de canalización en el diseñador, siga estos pasos:
    
    1. Establezca un destino de proceso predeterminado para la canalización.
    
    1. Seleccione **Ejecutar** en la parte superior del lienzo de la canalización.
    
    1. Seleccione un experimento para agrupar las ejecuciones de la canalización.
    
    ---

* Supervisión del estado de una ejecución

    # <a name="python"></a>[Python](#tab/python)
    
    * Obtenga el estado de una ejecución con el método [`get_status()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-status--).
    
        ```python
        print(notebook_run.get_status())
        ```
    
    * Para obtener el identificador de ejecución, el tiempo de ejecución y los detalles adicionales sobre la ejecución, use el método [`get_details()`](/python/api/azureml-core/azureml.core.workspace.workspace?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-details--).
    
        ```python
        print(notebook_run.get_details())
        ```
    
    * Cuando la ejecución finalice correctamente, use el método [`complete()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truecomplete--set-status-true-) para marcarla como completada.
    
        ```python
        notebook_run.complete()
        print(notebook_run.get_status())
        ```
    
    * Si usa el modelo de diseño `with...as` de Python, la ejecución se marcará automáticamente como completada cuando esta quede fuera del ámbito. No tiene que marcar la ejecución como completada de forma manual.
        
        ```python
        with exp.start_logging() as notebook_run:
            notebook_run.log(name="message", value="Hello from run!")
            print(notebook_run.get_status())
        
        print(notebook_run.get_status())
        ```
    
    # <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
    
    * Para ver una lista de ejecuciones del experimento, use el siguiente comando. Reemplace `experiment` por el nombre del experimento:
    
        ```azurecli-interactive
        az ml run list --experiment-name experiment
        ```
    
        Este comando devuelve un documento JSON que muestra información sobre las ejecuciones de este experimento.
    
        Para obtener más información, consulte [az ml experiment list](/cli/azure/ext/azure-cli-ml/ml/experiment?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).
    
    * Para ver información sobre una ejecución concreta, use el siguiente comando. Reemplace `runid` por el identificador de la ejecución:
    
        ```azurecli-interactive
        az ml run show -r runid
        ```
    
        Este comando devuelve un documento JSON que muestra información sobre la ejecución.
    
        Para obtener más información, consulte [az ml run show](/cli/azure/ext/azure-cli-ml/ml/run?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).
    
    
    # <a name="studio"></a>[Estudio](#tab/azure-studio)
    
    Para ver el número de ejecuciones activas del experimento en Studio, haga lo siguiente.
    
    1. Vaya a la sección **Experiments** (Experimentos).
    
    1. Seleccione un experimento.
    
        En la página del experimento, puede ver el número de destinos de proceso activos y la duración de cada ejecución. 
    
    1. Para hacer personalizaciones en el experimento, seleccione ejecuciones para comparar, agregue gráficos o aplique filtros. Estos cambios se pueden guardar como una **vista personalizada** para que pueda volver fácilmente a su trabajo. Los usuarios con permisos de área de trabajo pueden editar o ver la vista personalizada. Además, comparta la vista personalizada con otros usuarios copiando y pegando la dirección URL en el explorador.  
    
        :::image type="content" source="media/how-to-manage-runs/custom-views.gif" alt-text="Captura de pantalla: creación de una vista personalizada":::
    
    1. Seleccione un número de ejecución específico.
    
    1. En la pestaña **Registros**, puede encontrar los registros de diagnóstico y errores de la ejecución de la canalización.
    
    ---
    
## <a name="cancel-or-fail-runs"></a>Cancelación o ejecuciones erróneas

Si detecta un error o si la ejecución tarda demasiado en finalizar, puede cancelarla.

# <a name="python"></a>[Python](#tab/python)

Para cancelar una ejecución mediante el SDK, use el método [`cancel()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truecancel--):

```python
src = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_run = exp.submit(src)
print(local_run.get_status())

local_run.cancel()
print(local_run.get_status())
```

Si la ejecución finaliza, pero contiene un error (por ejemplo, se ha usado el script de entrenamiento incorrecto), puede usar el método [`fail()`](/python/api/azureml-core/azureml.core.run%28class%29#fail-error-details-none--error-code-none---set-status-true-) para marcarla como errónea.

```python
local_run = exp.submit(src)
local_run.fail()
print(local_run.get_status())
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para cancelar una ejecución con la CLI, use el siguiente comando. Reemplace `runid` por el identificador de la ejecución.

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Para obtener más información, consulte [az ml run cancel](/cli/azure/ext/azure-cli-ml/ml/run?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

# <a name="studio"></a>[Estudio](#tab/azure-studio)

Para cancelar una ejecución en Studio, siga estos pasos:

1. Vaya a la canalización en ejecución en la sección **Experimentos** o **Canalizaciones**. 

1. Seleccione el número de ejecución de la canalización que quiere cancelar.

1. En la barra de herramientas, seleccione **Cancelar**.

---

## <a name="create-child-runs"></a>Creación de ejecuciones secundarias

Cree ejecuciones secundarias para agrupar ejecuciones relacionadas (por ejemplo, para diferentes iteraciones de ajuste de hiperparámetros).

> [!NOTE]
> Las ejecuciones secundarias solo se pueden crear mediante el SDK.

En este ejemplo de código se usa el script `hello_with_children.py` para crear un lote de cinco ejecuciones secundarias desde una ejecución enviada mediante el método [`child_run()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truechild-run-name-none--run-id-none--outputs-none-):

```python
!more hello_with_children.py
src = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_run = exp.submit(src)
local_run.wait_for_completion(show_output=True)
print(local_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> A medida que quedan fuera del ámbito, las ejecuciones secundarias se marcan automáticamente como completadas.

Para crear muchas ejecuciones secundarias de forma eficaz, use el método [`create_children()`](/python/api/azureml-core/azureml.core.run.run?preserve-view=true&view=azure-ml-py#&preserve-view=truecreate-children-count-none--tag-key-none--tag-values-none-). Dado que cada creación da lugar a una llamada de red, la creación de un lote de ejecuciones es más eficaz que hacerlo una a una.

### <a name="submit-child-runs"></a>Envío de ejecuciones secundarias

Las ejecuciones secundarias también se pueden enviar desde una ejecución principal. Esto le permite crear jerarquías de ejecuciones primarias y secundarias. No se puede crear una ejecución secundaria sin elemento primario: aunque la ejecución primaria no hace más que iniciar ejecuciones secundarias, sigue siendo necesario crear la jerarquía. El estado de todas las ejecuciones es independiente: un elemento primario puede estar en el estado `"Completed"` correcto aunque una o varias ejecuciones secundarias se hayan cancelado o hayan dado error.  

Es posible que desee que las ejecuciones secundarias utilicen una configuración de ejecución diferente de la ejecución primaria. Por ejemplo, puede utilizar una configuración menos potente basada en CPU para el elemento primario, mientras usa configuraciones basadas en GPU para los elementos secundarios. Otro deseo habitual es pasar a cada ejecución secundaria argumentos y datos distintos. Para personalizar una ejecución secundaria, cree un objeto de `ScriptRunConfig` para la ejecución secundaria. El código que se muestra a continuación hace lo siguiente:

- Recuperación de un recurso de proceso denominado `"gpu-cluster"` del área de trabajo `ws`
- Recorre en iteración los diferentes valores de los argumentos que se van a pasar a los objetos `ScriptRunConfig` secundarios
- Crea y envía una nueva ejecución secundaria mediante el recurso de proceso personalizado y el argumento
- Se bloquea hasta que se completan todas las ejecuciones secundarias

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig

compute_target = ws.compute_targets["gpu-cluster"]

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], compute_target=compute_target)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Para crear muchas ejecuciones secundarias con configuraciones, argumentos y entradas idénticas de forma eficaz, use el método [`create_children()`](/python/api/azureml-core/azureml.core.run.run?preserve-view=true&view=azure-ml-py#&preserve-view=truecreate-children-count-none--tag-key-none--tag-values-none-). Dado que cada creación da lugar a una llamada de red, la creación de un lote de ejecuciones es más eficaz que hacerlo una a una.

Desde la ejecución secundaria, puede ver el identificador de la ejecución principal:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Consulta de ejecuciones secundarias

Para consultar las ejecuciones secundarias de un elemento primario específico, use el método [`get_children()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueget-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-). El argumento ``recursive = True`` permite consultar un árbol anidado de elementos secundarios y descendientes.

```python
print(parent_run.get_children())
```

### <a name="log-to-parent-or-root-run"></a>Registro en la ejecución primaria o raíz

Puede usar el campo `Run.parent` para acceder a la ejecución que inició la ejecución secundaria actual. Un caso de uso común para esto es cuando desea consolidar los resultados del registro en un único lugar. Tenga en cuenta que las ejecuciones secundarias se ejecutan de forma asincrónica y no hay ninguna garantía de ordenación o sincronización más allá de la capacidad del elemento primario de esperar a que se completen las ejecuciones secundarias.

```python
# in child (or even grandchild) run

def root_run(self : Run) -> Run :
    if self.parent is None : 
        return self
    return root_run(self.parent)

current_child_run = Run.get_context()
root_run(current_child_run).log("MyMetric", f"Data from child run {current_child_run.id}")

```


## <a name="tag-and-find-runs"></a>Etiquetado y búsqueda de ejecuciones

En Azure Machine Learning, puede usar etiquetas y propiedades para ayudar a organizar y consultar las ejecuciones a fin de obtener información importante.

* Adición de etiquetas y propiedades

    # <a name="python"></a>[Python](#tab/python)
    
    Para agregar metadatos de búsqueda a las ejecuciones, use el método [`add_properties()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueadd-properties-properties-). Por ejemplo, el código siguiente agrega la propiedad `"author"` a la ejecución:
    
    ```Python
    local_run.add_properties({"author":"azureml-user"})
    print(local_run.get_properties())
    ```
    
    Las propiedades son inmutables, por lo que crean un registro permanente con fines de auditoría. El siguiente ejemplo de código da como resultado un error porque ya hemos agregado `"azureml-user"` como el valor de propiedad `"author"` en el código anterior:
    
    ```Python
    try:
        local_run.add_properties({"author":"different-user"})
    except Exception as e:
        print(e)
    ```
    
    A diferencia de las propiedades, las etiquetas son mutables. Para agregar información que permite realizar búsquedas y que es significativa para los consumidores del experimento, use el método [`tag()`](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truetag-key--value-none-).
    
    ```Python
    local_run.tag("quality", "great run")
    print(local_run.get_tags())
    
    local_run.tag("quality", "fantastic run")
    print(local_run.get_tags())
    ```
    
    También puede agregar etiquetas de cadena simples. Cuando estas etiquetas aparecen en el diccionario de etiquetas como claves, tienen un valor de `None`.
    
    ```Python
    local_run.tag("worth another look")
    print(local_run.get_tags())
    ```
    
    # <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
    
    > [!NOTE]
    > Con la CLI, solo puede agregar o actualizar las etiquetas.
    
    Para agregar o actualizar una etiqueta, use el siguiente comando:
    
    ```azurecli-interactive
    az ml run update -r runid --add-tag quality='fantastic run'
    ```
    
    Para obtener más información, consulte [az ml run update](/cli/azure/ext/azure-cli-ml/ml/run?preserve-view=true&view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).
    
    # <a name="studio"></a>[Estudio](#tab/azure-studio)
    
    Puede ver las propiedades y las etiquetas en Studio, pero no puede modificarlas aquí.
    
    ---

* Consulta de etiquetas y propiedades

    Puede consultar las ejecuciones de un experimento para devolver una lista de ejecuciones que coinciden con etiquetas y propiedades específicas.

    # <a name="python"></a>[Python](#tab/python)
    
    ```Python
    list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
    list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
    ```
    
    # <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
    
    La CLI de Azure admite las consultas [JMESPath](http://jmespath.org), que se pueden usar para filtrar las ejecuciones en función de etiquetas y propiedades. Para usar una consulta JMESPath con la CLI de Azure, especifíquela con el parámetro `--query`. En los ejemplos siguientes se muestran algunas consultas que usan etiquetas y propiedades:
    
    ```azurecli-interactive
    # list runs where the author property = 'azureml-user'
    az ml run list --experiment-name experiment [?properties.author=='azureml-user']
    # list runs where the tag contains a key that starts with 'worth another look'
    az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
    # list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
    az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
    ```
    
    Para obtener más información sobre la consulta de resultados de la CLI de Azure, vea [Resultados de los comandos de consulta de la CLI de Azure](/cli/azure/query-azure-cli?preserve-view=true&view=azure-cli-latest).
    
    # <a name="studio"></a>[Estudio](#tab/azure-studio)
    
    1. Vaya a la sección **Canalizaciones**.
    
    1. Use la barra de búsqueda para filtrar las canalizaciones mediante etiquetas, descripciones, nombres de experimento y nombres de remitente.
    
    ---

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Los siguientes cuadernos muestran los conceptos de este artículo:

* Para obtener más información sobre las API de registro, vea el [cuaderno de API de registro](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Para obtener más información sobre cómo administrar ejecuciones con el SDK de Azure Machine Learning, consulte el [cuaderno de administración de ejecuciones](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo registrar las métricas de sus experimentos, consulte [Registrar métricas durante las ejecuciones de entrenamiento](how-to-track-experiments.md).
* Para aprender a supervisar los recursos y los registros desde Azure Machine Learning, consulte [Supervisión de Azure Machine Learning](monitor-azure-machine-learning.md).