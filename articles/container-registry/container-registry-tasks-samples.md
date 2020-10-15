---
title: Ejemplos de ACR Tasks
description: Azure Container Registry Tasks (ACR Tasks) de ejemplo para compilar y ejecutar imágenes de contenedor, y aplicarles revisiones
ms.topic: article
ms.date: 11/14/2019
ms.openlocfilehash: 49df3bf565052a729ac3c587bd2ba11a299d05f1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74456086"
---
# <a name="acr-tasks-samples"></a>Ejemplos de ACR Tasks

En este artículo se ofrecen vínculos a archivos `task.yaml` de ejemplo y Dockerfiles asociados para varios escenarios de [Azure Container Registry](container-registry-tasks-overview.md) (ACR Tasks). 

Para ver otros ejemplos, consulte el repositorio de [ejemplos de Azure][task-examples].

## <a name="scenarios"></a>Escenarios

* **Compilación de imagen** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Ejecución de contenedor** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml)

* **Compilación e inserción de imagen** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Compilación e ejecución de Compilación e inserción de imágenes** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Compilación e inserción de varias imágenes** -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world-multi.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Compilación y prueba de imágenes en paralelo** -  [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.dockerfile)

* **Compilación e inserción de imágenes en varios registros** - [YAML](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/testtask.yaml), [Dockerfile](https://github.com/Azure-Samples/acr-tasks/blob/master/multipleRegistries/hello-world.dockerfile)


## <a name="next-steps"></a>Pasos siguientes

Más información sobre ACR Tasks:

* [Tareas de varios pasos](container-registry-tasks-multi-step.md): flujos de trabajo basados en ACR Tasks para compilar y probar imágenes de contenedor en la nube, y aplicarles revisiones.
* [Referencia de tareas](container-registry-tasks-reference-yaml.md): tipos de pasos de tareas, sus propiedades y uso.
* [Repositorio de cmd](https://github.com/AzureCR/cmd): colección de contenedores que se usan como comandos de ACR Tasks.


<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks
