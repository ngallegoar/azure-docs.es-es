---
title: Extensión de la CLI
titleSuffix: Azure Machine Learning
description: Información acerca de la extensión de la CLI de Azure Machine Learning para la CLI de Azure. La CLI de Azure es una utilidad de la línea de comandos multiplataforma que le permite trabajar con recursos de la nube de Azure. La extensión de Machine Learning permite trabajar con Azure Machine Learning. La CLI de ML crea y administra recursos, como el área de trabajo, los almacenes de datos, los conjuntos de datos, las canalizaciones, los modelos y las implementaciones.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 471b26ebc4bd4aecb814ec43c7eba56e3d764fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402496"
---
# <a name="use-the-cli-extension-for-azure-machine-learning"></a>Use la extensión de la CLI para Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

La CLI de Azure Machine Learning es una extensión de la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), una interfaz de la línea de comandos multiplataforma para la plataforma Azure. Esta extensión proporciona comandos para trabajar con Azure Machine Learning. Permite automatizar las actividades de aprendizaje automático. En esta lista se muestran algunas acciones de ejemplo que puede realizar con la extensión de la CLI:

+ Realizar experimentos para crear modelos de aprendizaje automático

+ Registrar los modelos de aprendizaje automático para el uso del cliente

+ Empaquetar, implementar y realizar un seguimiento del ciclo de vida de los modelos de aprendizaje automático

La CLI no sustituye al SDK de Azure Machine Learning. Es una herramienta complementaria que está optimizada para administrar tareas con muchos parámetros y que están bien adaptadas para la automatización.

## <a name="prerequisites"></a>Prerequisites

* Para usar la CLI, debe tener una suscripción de Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* La[CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

## <a name="full-reference-docs"></a>Documentos de referencia completos

Busque los [documentos de referencia completos sobre la extensión azure-cli-ml de la CLI de Azure](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Instalación de la extensión

Para instalar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> [Aquí](https://aka.ms/azml-deploy-cloud) puede encontrar algunos archivos de ejemplo que puede usar con los comandos siguientes.

Cuando se le solicite, seleccione `y` para instalar la extensión.

Para comprobar que se ha instalado la extensión, utilice el siguiente comando para mostrar una lista de los subcomandos específicos de ML:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>Actualización de la extensión

Para actualizar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Eliminación de la extensión

Para eliminar la extensión de la CLI, use el siguiente comando:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Administración de recursos

Los siguientes comandos muestran cómo utilizar la CLI para administrar los recursos que usa Azure Machine Learning.

+ Cree un grupo de recursos si todavía no tiene uno:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Cree un área de trabajo de Azure Machine Learning:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    > [!TIP]
    > Este comando crea un área de trabajo de edición básica. Para crear un área de trabajo empresarial, use el modificador `--sku enterprise` con el comando `az ml workspace create`. Para más información sobre las ediciones de Azure Machine Learning, consulte [¿Qué es Azure Machine Learning?](overview-what-is-azure-ml.md#sku).

    Para más información, consulte [az ml workspace create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ Adjunte una configuración de área de trabajo a una carpeta para permitir el reconocimiento contextual de la CLI.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Este comando crea un subdirectorio `.azureml` que contiene archivos de entorno de conda y runconfig de ejemplo. También contiene un archivo `config.json` que se usa para comunicarse con el área de trabajo de Azure Machine Learning.

    Para obtener más información, consulte [az ml folder attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

+ Adjunte un contenedor de blobs de Azure como almacén de datos.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Para más información, consulte [az ml datastore attach-blob](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob).

+ Cargue archivos a un almacén de archivos.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Para más información, consulte [az ml datastore upload](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-upload).

+ Adjunte un clúster de AKS como destino de proceso.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Para más información, consulte [az ml computetarget attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks)

+ Cree un nuevo destino AMLcompute.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Para más información, consulte [az ml computetarget create amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute).

## <a name="run-experiments"></a><a id="experiments"></a>Ejecución de los experimentos

* Inicie la ejecución del experimento. Cuando use este comando, especifique el nombre del archivo runconfig (el texto que va antes de \*.runconfig, si mira el sistema de archivo) con respecto al parámetro -c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > El comando `az ml folder attach` crea un subdirectorio `.azureml`, que contiene dos archivos runconfig de ejemplo. 
    >
    > Si tiene un script de Python que crea un objeto de configuración de ejecución mediante programación, puede usar [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) para guardarlo como un archivo runconfig.
    >
    > El esquema runconfig completo se puede encontrar en este [archivo JSON](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json). El esquema se documenta automáticamente mediante la clave `description` de cada objeto. Además, hay enumeraciones de valores posibles y un fragmento de código de plantilla al final.

    Para obtener más información, consulte [az ml run submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Vea una lista de los experimentos:

    ```azurecli-interactive
    az ml experiment list
    ```

    Para obtener más información, consulte [az ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="dataset-management"></a>Administración de conjuntos de datos

Los siguientes comandos muestran cómo trabajar con conjuntos de datos en Azure Machine Learning:

+ Registre un conjunto de datos:

    ```azurecli-interactive
    az ml dataset register -f mydataset.json
    ```

    Para obtener información sobre el formato del archivo JSON que se usa para definir el conjunto de datos, use `az ml dataset register --show-template`.

    Para más información, consulte [az ml dataset register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Archive un conjunto de datos activo o en desuso:

    ```azurecli-interactive
    az ml dataset archive -n dataset-name
    ```

    Para más información, consulte [az ml dataset archive](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Deje en desuso un conjunto de datos:

    ```azurecli-interactive
    az ml dataset deprecate -d replacement-dataset-id -n dataset-to-deprecate
    ```

    Para más información, consulte [az ml dataset deprecate](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Enumerar todos los conjuntos de datos de un área de trabajo:

    ```azurecli-interactive
    az ml dataset list
    ```

    Para más información, consulte [az ml dataset list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Obtenga los detalles de un conjunto de datos:

    ```azurecli-interactive
    az ml dataset show -n dataset-name
    ```

    Para más información, consulte [az ml dataset show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Reactive un conjunto de datos archivado o en desuso:

    ```azurecli-interactive
    az ml dataset reactivate -n dataset-name
    ```

    Para más información, consulte [az ml dataset reactivate](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

+ Anule el registro de un conjunto de datos:

    ```azurecli-interactive
    az ml dataset unregister -n dataset-name
    ```

    Para más información, consulte [az ml dataset unregister](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/dataset?view=azure-cli-latest#ext-azure-cli-ml-az-ml-dataset-archive).

## <a name="environment-management"></a>Administrador de entornos

Los comandos siguientes muestran cómo crear, registrar y enumerar los [entornos](how-to-configure-environment.md) de Azure Machine Learning para el área de trabajo:

+ Creación de archivos de scaffolding para un entorno:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Para más información, consulte [az ml environment scaffold](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-scaffold).

+ Registro de un entorno:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Para más información, consulte [az ml environment register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-register).

+ Lista de entornos registrados:

    ```azurecli-interactive
    az ml environment list
    ```

    Para más información, consulte [az ml environment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-list).

+ Descargue un entorno registrado:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Para más información, consulte [az ml environment download](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-download).

### <a name="environment-configuration-schema"></a>Esquema de configuración del entorno

Si usa el comando `az ml environment scaffold`, se genera un archivo de plantilla `azureml_environment.json` que puede modificarse y usarse para crear configuraciones de entorno personalizadas con la CLI. El objeto de nivel superior se asigna de forma flexible a la clase [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py) en el SDK de Python. 

```json
{
    "name": "testenv",
    "version": null,
    "environmentVariables": {
        "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
    },
    "python": {
        "userManagedDependencies": false,
        "interpreterPath": "python",
        "condaDependenciesFile": null,
        "baseCondaEnvironment": null
    },
    "docker": {
        "enabled": false,
        "baseImage": "mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04",
        "baseDockerfile": null,
        "sharedVolumes": true,
        "shmSize": "2g",
        "arguments": [],
        "baseImageRegistry": {
            "address": null,
            "username": null,
            "password": null
        }
    },
    "spark": {
        "repositories": [],
        "packages": [],
        "precachePackages": true
    },
    "databricks": {
        "mavenLibraries": [],
        "pypiLibraries": [],
        "rcranLibraries": [],
        "jarLibraries": [],
        "eggLibraries": []
    },
    "inferencingStackVersion": null
}
```

En la tabla que aparece más abajo se detallan todos los campos de nivel superior del archivo JSON, su tipo y su descripción. Si un tipo de objeto está vinculado a una clase del SDK de Python, hay una coincidencia 1:1 débil entre cada campo JSON y el nombre de la variable pública en la clase de Python. En algunos casos, el campo puede asignarse a un argumento de constructor en lugar de a una variable de clase. Por ejemplo, el campo `environmentVariables` se asigna a la variable `environment_variables` en la clase [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py).

| Campo JSON | Tipo | Descripción |
|---|---|---|
| `name` | `string` | Nombre del entorno. No escriba **Microsoft** o **AzureML** al principio del nombre. |
| `version` | `string` | Versión del entorno. |
| `environmentVariables` | `{string: string}` | Mapa hash de nombres y valores de variables de entorno. |
| `python` | [`PythonSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py) | Objeto que define el entorno de Python y el intérprete que se va a usar en el recurso de proceso de destino. |
| `docker` | [`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) | Sección que define la configuración para personalizar la imagen de Docker compilada según las especificaciones del entorno. |
| `spark` | [`SparkSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.sparksection?view=azure-ml-py) | Sección que define la configuración de Spark. Solo se usa cuando el marco se establece en PySpark. |
| `databricks` | [`DatabricksSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.databricks.databrickssection?view=azure-ml-py) | Sección que define la configuración de las dependencias de las bibliotecas de Databricks. |
| `inferencingStackVersion` | `string` | Campo que especifica la versión de la pila de inferencia agregada a la imagen. Para evitar agregar una pila de inferencia, deje este campo como `null`. Valor válido: "latest" (más reciente). |

## <a name="ml-pipeline-management"></a>Administración de canalizaciones de aprendizaje automático

Los siguientes comandos muestran cómo trabajar con canalizaciones de aprendizaje automático:

+ Creación de una canalización de aprendizaje automático:

    ```azurecli-interactive
    az ml pipeline create -n mypipeline -y mypipeline.yml
    ```

    Para obtener más información, consulte [az ml pipeline create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create).

    Para obtener más información sobre el archivo de canalización YAML, vea [Definición de canalizaciones de aprendizaje automático en YAML](reference-pipeline-yaml.md).

+ Ejecución de una canalización:

    ```azurecli-interactive
    az ml run submit-pipeline -n myexperiment -y mypipeline.yml
    ```

    Para obtener más información, consulte [az ml run submit-pipeline](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-pipeline).

    Para obtener más información sobre el archivo de canalización YAML, vea [Definición de canalizaciones de aprendizaje automático en YAML](reference-pipeline-yaml.md).

+ Programación de una canalización:

    ```azurecli-interactive
    az ml pipeline create-schedule -n myschedule -e myexpereiment -i mypipelineid -y myschedule.yml
    ```

    Para obtener más información, consulte [az ml pipeline create-schedule](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create-schedule).

    Para obtener más información sobre el archivo YAML de programación de canalización, vea [Definición de canalizaciones de aprendizaje automático en YAML](reference-pipeline-yaml.md#schedules).

## <a name="model-registration-profiling-deployment"></a>Registro del modelo, generación de perfiles, implementación

Los siguientes comandos muestran cómo registrar un modelo entrenado e implementarlo como servicio de producción:

+ Registre un modelo con Azure Machine Learning:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Para más información, consulte [az ml model register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register).

+ **OPCIONAL** Genere un perfil del modelo para obtener los valores óptimos de CPU y memoria para la implementación.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Para más información, consulte [az ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

+ Implementación del modelo en AKS
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Para más información sobre el esquema del archivo de configuración de inferencia, consulte [Esquema de configuración de inferencia](#inferenceconfig).
    
    Para más información sobre el esquema del archivo de configuración de implementación, consulte [Esquema de configuración de implementación](#deploymentconfig).

    Para más información, consulte [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Esquema de configuración de inferencia

[!INCLUDE [inferenceconfig](../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Esquema de configuración de implementación

### <a name="local-deployment-configuration-schema"></a>Esquema de configuración de implementación local

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Esquema de configuración de implementación de Azure Container Instances 

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Esquema de configuración de implementación de Azure Kubernetes Service

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de comandos para la extensión de la CLI de Machine Learning](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest).

* [Entrenamiento e implementación de modelos de aprendizaje automático mediante Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning)
