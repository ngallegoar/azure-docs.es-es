---
title: Servicio del modelo de alto rendimiento con Triton (versión preliminar)
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo implementar un modelo con Triton Inference Server en Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.date: 09/23/2020
ms.topic: conceptual
ms.reviewer: larryfr
ms.custom: deploy
ms.openlocfilehash: 9a6e2de07921d05e123154f604c3d1b369b3b89d
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91998762"
---
# <a name="high-performance-serving-with-triton-inference-server-preview"></a>Servicio de alto rendimiento con Triton Inference Server (versión preliminar) 

Aprenda a usar [Triton Inference Server de NVIDIA](https://developer.nvidia.com/nvidia-triton-inference-server) para mejorar el rendimiento del servicio web que se usa para la inferencia de modelos.

Una de las formas de implementar un modelo para la inferencia es como servicio web. Por ejemplo, una implementación en Azure Kubernetes Service o Azure Container Instances. De manera predeterminada, Azure Machine Learning usa un marco web *de uso general* de un solo subproceso para las implementaciones de servicios web.

Triton es un marco que está *optimizado para la inferencia*. Ofrece un mejor uso de las GPU y una inferencia más rentable. Del lado servidor, procesa por lotes las solicitudes entrantes y envía estos lotes para inferencia. El procesamiento por lotes mejora el uso de los recursos de GPU y es una parte fundamental del rendimiento de Triton.

> [!IMPORTANT]
> El uso de Triton para la implementación desde Azure Machine Learning está actualmente en __versión preliminar__. Es posible que la funcionalidad de versión preliminar no esté incluida en el soporte técnico al cliente. Para obtener más información, consulte [Condiciones de uso complementarias de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

> [!TIP]
> Los fragmentos de código de este documento tienen fines ilustrativos y puede que no muestren una solución completa. Para ver el código de ejemplo funcional, consulte los [ejemplos integrales de Triton en Azure Machine Learning](https://aka.ms/aml-triton-sample).

## <a name="prerequisites"></a>Requisitos previos

* Una **suscripción de Azure**. Si no tiene una ya, pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* Familiaridad con [cómo y dónde se implementa un modelo](how-to-deploy-and-where.md) con Azure Machine Learning.
* El [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py) **o** la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) y la [extensión de Machine Learning](reference-azure-machine-learning-cli.md).
* Una instalación en funcionamiento de Docker para las pruebas locales. Para obtener información sobre la instalación y validación de Docker, consulte [Orientation and setup](https://docs.docker.com/get-started/) (Orientación e instalación) en la documentación de Docker.

## <a name="architectural-overview"></a>Introducción a la arquitectura

Antes de intentar usar Triton para su propio modelo, es importante comprender cómo funciona con Azure Machine Learning y cómo se compara con una implementación predeterminada.

**Implementación predeterminada sin Triton**

* Se inician varios trabajos de [Gunicorn](https://gunicorn.org/) para controlar las solicitudes entrantes simultáneamente.
* Estos trabajos controlan el procesamiento previo, la llamada al modelo y el procesamiento posterior. 
* Las solicitudes de inferencia usan el __URI de puntuación__. Por ejemplo, `https://myserevice.azureml.net/score`.

:::image type="content" source="./media/how-to-deploy-with-triton/normal-deploy.png" alt-text="Diagrama de la arquitectura de implementación normal, sin Triton":::

**Implementación de la configuración de inferencia con Triton**

* Se inician varios trabajos de [Gunicorn](https://gunicorn.org/) para controlar las solicitudes entrantes simultáneamente.
* Las solicitudes se reenvían al **servidor de Triton**. 
* Triton procesa las solicitudes por lotes para maximizar el uso de la GPU.
* El cliente usa el __URI de puntuación__ para hacer solicitudes. Por ejemplo, `https://myserevice.azureml.net/score`.

:::image type="content" source="./media/how-to-deploy-with-triton/inferenceconfig-deploy.png" alt-text="Diagrama de la arquitectura de implementación normal, sin Triton":::

El flujo de trabajo para usar Triton para la implementación del modelo es el siguiente:

1. Compruebe que Triton puede servir a su modelo.
1. Compruebe que puede enviar solicitudes al modelo implementado por Triton.
1. Incorpore el código específico de Triton en la implementación de AML.

## <a name="optional-define-a-model-config-file"></a>(Opcional) Definición de un archivo de configuración de modelo

El archivo de configuración del modelo indica a Triton el número de entradas que espera y qué dimensiones tendrán esas entradas. Para obtener más información sobre cómo crear el archivo de configuración, consulte [Configuración del modelo](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/model_configuration.html) en la documentación de NVIDIA.

> [!TIP]
> Usamos la opción `--strict-model-config=false` al iniciar Triton Inference Server, lo que significa que no es necesario proporcionar un archivo `config.pbtxt` para los modelos de ONNX ni TensorFlow.
> 
> Para obtener más información sobre esta opción, consulte [Configuración del modelo generada](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/model_configuration.html#generated-model-configuration) en la documentación de NVIDIA.

## <a name="directory-structure"></a>Estructura de directorios

Al registrar un modelo en Azure Machine Learning, puede registrar archivos individuales o una estructura de directorios. Para usar Triton, el registro del modelo debe ser para una estructura de directorios que contenga un directorio denominado `triton`. La estructura general de este directorio es:

```bash
models
    - triton
        - model_1
            - model_version
                - model_file
                - config_file
        - model_2
            ...
```

> [!IMPORTANT]
> Esta estructura de directorios es un repositorio de modelos de Triton y es necesario para que los modelos funcionen con Triton. Para obtener más información, consulte [Repositorios de modelos de Triton](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/model_repository.html) en la documentación de NVIDIA.

## <a name="test-with-triton-and-docker"></a>Pruebas con Triton y Docker

Para probar el modelo para asegurarse de que se ejecuta con Triton, puede usar Docker. Los siguientes comandos extraen el contenedor Triton en el equipo local y, a continuación, inician el servidor Triton:

1. Para extraer la imagen del servidor Triton en el equipo local, use el comando siguiente:

    ```bash
    docker pull nvcr.io/nvidia/tritonserver:20.09-py3
    ```

1. Para iniciar el servidor Triton, use el comando siguiente. Reemplace `<path-to-models/triton>` por la ruta de acceso al repositorio del modelo Triton que contiene los modelos:

    ```bash
    docker run --rm -ti -v<path-to-models/triton>:/models nvcr.io/nvidia/tritonserver:20.09-py3 tritonserver --model-repository=/models --strict-model-config=false
    ```

    > [!IMPORTANT]
    > Si usa Windows, es posible que se le pida que permita las conexiones de red a este proceso la primera vez que ejecute el comando. Si es así, seleccione para habilitar el acceso.

    Una vez iniciado, se registra información similar al siguiente texto en la línea de comandos:

    ```bash
    I0923 19:21:30.582866 1 http_server.cc:2705] Started HTTPService at 0.0.0.0:8000
    I0923 19:21:30.626081 1 http_server.cc:2724] Started Metrics Service at 0.0.0.0:8002
    ```

    La primera línea indica la dirección del servicio web. En este caso, `0.0.0.0:8000`, que es igual que `localhost:8000`.

1. Use una utilidad, como cURL, para acceder al punto de conexión de mantenimiento.

    ```bash
    curl -L -v -i localhost:8000/v2/health/ready
    ```

    Este comando devuelve información similar a la siguiente. Tenga en cuenta el `200 OK`; este estado significa que el servidor web se está ejecutando.

    ```bash
    *   Trying 127.0.0.1:8000...
    * Connected to localhost (127.0.0.1) port 8000 (#0)
    > GET /v2/health/ready HTTP/1.1
    > Host: localhost:8000
    > User-Agent: curl/7.71.1
    > Accept: */*
    >
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 200 OK
    HTTP/1.1 200 OK
    ```

Además de una comprobación de estado básica, puede crear un cliente para enviar datos a Triton para inferencia. Para obtener más información sobre la creación de un cliente, consulte los [ejemplos de cliente](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/client_example.html) en la documentación de NVIDIA. También hay [ejemplos de Python en GitHub de Triton](https://github.com/triton-inference-server/server/tree/master/src/clients/python/examples).

Para obtener más información sobre cómo ejecutar Triton mediante Docker, consulte [Ejecución de Triton en un sistema con una GPU](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/run.html#running-triton-on-a-system-with-a-gpu) y [Ejecución de Triton en un sistema sin una GPU](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/run.html#running-triton-on-a-system-without-a-gpu).

## <a name="register-your-model"></a>Registro del modelo

Ahora que ha comprobado que el modelo funciona con Triton, regístrelo en Azure Machine Learning. El registro del modelo almacena los archivos del modelo en el área de trabajo Azure Machine Learning y se usan al implementar con el SDK de Python y la CLI de Azure.

En los ejemplos siguientes se muestra cómo registrar los modelos:

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.model import Model

model = Model.register(
    model_path=os.path.join("..", "triton"),
    model_name="bidaf_onnx",
    tags={'area': "Natural language processing", 'type': "Question answering"},
    description="Question answering model from ONNX model zoo",
    workspace=ws
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
az ml model register --model-path='triton' \
--name='bidaf_onnx' \
--workspace-name='<my_workspace>'
```
---

<a id="processing"></a>

## <a name="add-pre-and-post-processing"></a>Adición de código anterior posterior al procesamiento

Después de comprobar que el servicio web funciona, puede agregar código anterior y posterior al procesamiento; para ello, defina un _script de entrada_. Este archivo se denomina `score.py`. Para obtener más información sobre los scripts de entrada, consulte [Definición de un script de entrada](how-to-deploy-and-where.md#define-an-entry-script).

Los dos pasos principales son inicializar un cliente HTTP de Triton en el método `init()` y llamar a ese cliente en la función `run()`.

### <a name="initialize-the-triton-client"></a>Inicialización del cliente de Triton

Incluya código como el del ejemplo siguiente en el archivo `score.py`. Triton en Azure Machine Learning espera ser atendido en localhost, puerto 8000. En este caso, localhost está dentro de la imagen de Docker para esta implementación, no es un puerto en la máquina local:

> [!TIP]
> El paquete `tritonhttpclient` de pip se incluye en el entorno de `AzureML-Triton` mantenido, por lo que no es necesario especificarlo como una dependencia de pip.

```python
import tritonhttpclient

def init():
    global triton_client
    triton_client = tritonhttpclient.InferenceServerClient(url="localhost:8000")
```

### <a name="modify-your-scoring-script-to-call-into-triton"></a>Modificación del script de puntuación para llamar a Triton

En el ejemplo siguiente se muestra cómo solicitar dinámicamente los metadatos para el modelo:

> [!TIP]
> Puede solicitar dinámicamente los metadatos de los modelos que se han cargado con Triton mediante el método `.get_model_metadata` del cliente de Triton. Para ver un ejemplo de uso, consulte los [cuadernos de ejemplo](https://aka.ms/triton-aml-sample).

```python
input = tritonhttpclient.InferInput(input_name, data.shape, datatype)
input.set_data_from_numpy(data, binary_data=binary_data)

output = tritonhttpclient.InferRequestedOutput(
         output_name, binary_data=binary_data, class_count=class_count)

# Run inference
res = triton_client.infer(model_name,
                          [input]
                          request_id='0',
                          outputs=[output])

```

<a id="redeploy"></a>

## <a name="redeploy-with-an-inference-configuration"></a>Reimplementación de una configuración de inferencia

Una configuración de inferencia le permite usar un script de entrada, así como el proceso de implementación de Azure Machine Learning mediante el SDK de Python o la CLI de Azure.

> [!IMPORTANT]
> Debe especificar el [entorno mantenido](./resource-curated-environments.md) `AzureML-Triton`.
>
> El ejemplo de código de Python clona `AzureML-Triton` en otro entorno denominado `My-Triton`. El código de la CLI de Azure también usa este entorno. Para obtener más información sobre la clonación de un entorno, consulte la referencia de [Environment.Clone()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py&preserve-view=true#clone-new-name-).

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.webservice import LocalWebservice
from azureml.core import Environment
from azureml.core.model import InferenceConfig


local_service_name = "triton-bidaf-onnx"
env = Environment.get(ws, "AzureML-Triton").clone("My-Triton")

for pip_package in ["nltk"]:
    env.python.conda_dependencies.add_pip_package(pip_package)

inference_config = InferenceConfig(
    entry_script="score_bidaf.py",  # This entry script is where we dispatch a call to the Triton server
    source_directory=os.path.join("..", "scripts"),
    environment=env
)

local_config = LocalWebservice.deploy_configuration(
    port=6789
)

local_service = Model.deploy(
    workspace=ws,
    name=local_service_name,
    models=[model],
    inference_config=inference_config,
    deployment_config=local_config,
    overwrite=True)

local_service.wait_for_deployment(show_output = True)
print(local_service.state)
# Print the URI you can use to call the local deployment
print(local_service.scoring_uri)
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

> [!TIP]
> Para obtener más información sobre la creación de una configuración de inferencia, consulte [Esquema de configuración de inferencia](./reference-azure-machine-learning-cli.md#inference-configuration-schema).

```azurecli
az ml model deploy -n triton-densenet-onnx \
-m densenet_onnx:1 \
--ic inference-config.json \
-e My-Triton --dc deploymentconfig.json \
--overwrite --compute-target=aks-gpu
```

---

Después de completar la implementación, se muestra el URI de puntuación. Para esta implementación local, será `http://localhost:6789/score`. Si implementa en la nube, puede usar el comando [az ml service show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext_azure_cli_ml_az_ml_service_show) de la CLI para obtener el URI de puntuación.

Para obtener información sobre cómo crear un cliente que envíe solicitudes de inferencia al URI de puntuación, consulte [Consumo de un modelo implementado como servicio web](how-to-consume-web-service.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si piensa seguir usando el área de trabajo de Azure Machine Learning, pero quiere deshacerse del servicio implementado, use una de las opciones siguientes:

# <a name="python"></a>[Python](#tab/python)

```python
local_service.delete()
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
az ml service delete -n triton-densenet-onnx
```

---

## <a name="next-steps"></a>Pasos siguientes

* [Consulte los ejemplos integrales de Triton en Azure Machine Learning](https://aka.ms/aml-triton-sample)
* Consulte los [ejemplos de cliente de Triton](https://github.com/triton-inference-server/server/tree/master/src/clients/python/examples)
* Lea la [documentación de Triton Inference Server](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.html)
* [Solución de problemas de implementaciones con errores](how-to-troubleshoot-deployment.md)
* [Implementación en Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md)
* [Actualización de servicios web](how-to-deploy-update-web-service.md)
* [Recopilar datos de modelos en producción](how-to-enable-data-collection.md)
