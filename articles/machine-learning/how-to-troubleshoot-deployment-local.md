---
title: Solución de problemas de implementación de servicios web en el entorno local
titleSuffix: Azure Machine Learning
description: Aprenda a abordar, solucionar y resolver los errores comunes de implementación de Docker con Azure Machine Learning en el entorno local.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: gvashishtha
ms.author: gopalv
ms.reviewer: luquinta
ms.date: 11/25/2020
ms.topic: troubleshooting
ms.custom: devx-track-python, deploy, contperfq2
ms.openlocfilehash: 18de43cb170ccdede8f7fe13dc0c6c0c2fa4e7a3
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188639"
---
# <a name="troubleshoot-model-deployment-locally"></a>Solución de problemas de implementación de modelos en el entorno local

Aprenda a abordar, solucionar y resolver los errores comunes de implementación del servicio web Docker con Azure Machine Learning en el entorno local.

## <a name="prerequisites"></a>Requisitos previos

* Una **suscripción de Azure**. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
* El [SDK de Azure Machine Learning](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* La[CLI de Azure](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).
* La [extensión de la CLI para Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Para depurar localmente, debe tener una instalación de Docker en funcionamiento en el sistema local.

    Para comprobar la instalación de Docker, use el comando `docker run hello-world` desde un símbolo del sistema o terminal. Para obtener información sobre la instalación de Docker o la solución de problemas de Docker, consulte la [Documentación de Docker](https://docs.docker.com/).

## <a name="debug-locally"></a>Depuración local

Si tiene problemas al implementar un modelo en ACI o AKS, impleméntelo como servicio web local. El uso de un servicio web local facilita la solución de problemas.

Puede encontrar un ejemplo de [cuaderno de implementación local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local.ipynb) en el repositorio de [MachineLearningNotebooks](https://github.com/Azure/MachineLearningNotebooks) para explorar un ejemplo ejecutable.

> [!WARNING]
> No se admiten las implementaciones de servicios web locales en escenarios de producción.

Para implementar de forma local, modifique el código para usar `LocalWebservice.deploy_configuration()` con el fin de crear una configuración de implementación. Luego use `Model.deploy()` para implementar el servicio. En el ejemplo siguiente se implementa un modelo (incluido en la variable de modelo) como un servicio web local:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import LocalWebservice


# Create inference configuration based on the environment definition and the entry script
myenv = Environment.from_conda_specification(name="env", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)
# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(
    ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Si va a definir su propio YAML de especificación de Conda, muestre azureml-defaults con la versión >= 1.0.45 como dependencia de PIP. Este paquete es necesario para hospedar el modelo como servicio web.

En este punto, se puede trabajar con el servicio de forma habitual. En el código siguiente se muestra cómo enviar datos al servicio:

```python
import json

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

test_sample = bytes(test_sample, encoding='utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

Para obtener más información sobre cómo personalizar el entorno de Python, consulte el tema sobre la [creación y administración de entornos de entrenamiento e implementación](how-to-use-environments.md). 

### <a name="update-the-service"></a>Actualizar el servicio

Durante las pruebas locales, es posible que deba actualizar el archivo `score.py` para agregar un registro o intentar resolver los problemas que haya descubierto. Para recargar los cambios realizados en el archivo `score.py`, use `reload()`. Por ejemplo, el código siguiente recarga el script para el servicio y luego envía datos. Los datos se puntúan con el archivo `score.py` actualizado:

> [!IMPORTANT]
> El método `reload` solo está disponible para las implementaciones locales. Para información sobre cómo actualizar una implementación en otro destino de proceso, consulte [cómo actualizar el servicio web](how-to-deploy-update-web-service.md).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> El script se recarga desde la ubicación que especifica el objeto `InferenceConfig` que usa el servicio.

Para cambiar el modelo, las dependencias de Conda o la configuración de implementación, use [update()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=trueupdate--args-). En el siguiente ejemplo se actualiza el modelo que usa el servicio:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Eliminación del servicio

Para eliminar el servicio, use [delete()](/python/api/azureml-core/azureml.core.webservice%28class%29?preserve-view=true&view=azure-ml-py#&preserve-view=truedelete--).

### <a name="inspect-the-docker-log"></a><a id="dockerlog"></a> Inspección del registro de Docker

Puede imprimir los mensajes detallados de registro del motor de Docker desde el objeto de servicio. Puede ver el registro para ACI, AKS y las implementaciones locales. En el ejemplo siguiente se muestra cómo imprimir los registros.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

Si ve que la línea `Booting worker with pid: <pid>` se produce varias veces en los registros, significa que no hay suficiente memoria para iniciar el trabajo.
Puede solucionar el error aumentando el valor de `memory_gb` en `deployment_config`.

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de la implementación:

* [Solución de problemas de implementación remota](how-to-troubleshoot-deployment.md)
* [Cómo implementar y dónde](how-to-deploy-and-where.md)
* [Tutorial: Entrenamiento e implementación de modelos](tutorial-train-models-with-aml.md)
* [Ejecución y depuración de experimentos en el entorno local](./how-to-debug-visual-studio-code.md)
