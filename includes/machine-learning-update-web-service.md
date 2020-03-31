---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: d36bf2db05113656a77e76ff900d95910f313c73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477262"
---
Para actualizar un servicio web, utilice el método `update`. Puede actualizar el servicio web para que use un nuevo modelo, un nuevo script de entrada o nuevas dependencias que se pueden especificar en una configuración de inferencia. Para más información, consulte la documentación de [Webservice.update](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#update--args-).

> [!IMPORTANT]
> Cuando crea una nueva versión del modelo, debe actualizar manualmente cada servicio que quiera que la use.
>
> No puede usar el SDK para actualizar un servicio web publicado desde el diseñador de Azure Machine Learning.

**Uso del SDK**

El código siguiente muestra cómo usar el SDK para actualizar el modelo, entorno y script de entrada para un servicio web:

```python
from azureml.core import Environment
from azureml.core.webservice import Webservice
from azureml.core.model import Model, InferenceConfig

# Register new model.
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

# Use version 3 of the environment.
deploy_env = Environment.get(workspace=ws,name="myenv",version="3")
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=deploy_env)

service_name = 'myservice'
# Retrieve existing service.
service = Webservice(name=service_name, workspace=ws)



# Update to new model(s).
service.update(models=[new_model], inference_config=inference_config)
print(service.state)
print(service.get_logs())
```

**Uso de la CLI**

También puede actualizar un servicio Web con la CLI de ML. En el ejemplo siguiente se muestra cómo registrar un nuevo modelo y luego actualizar el servicio web para usarlo:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> En este ejemplo, se usa un documento JSON para pasar la información del modelo del comando de registro al comando de actualización.
>
> Para actualizar el servicio de modo que use un nuevo entorno o script de entrada, cree un [archivo de configuración de inferencia](/azure/machine-learning/service/reference-azure-machine-learning-cli#inference-configuration-schema) y especifíquelo con el parámetro `ic`.

Para más información, consulte la documentación de [az ml service update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-update).