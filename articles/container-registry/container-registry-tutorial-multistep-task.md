---
title: 'Tutorial: Tarea de ACR de varios pasos'
description: En este tutorial, aprenderá a configurar una tarea de Azure Container Registry que desencadena automáticamente un flujo de trabajo de varios pasos para compilar, ejecutar e insertar imágenes de contenedor en la nube cuando confirma el código fuente en un repositorio de Git.
ms.topic: tutorial
ms.date: 05/09/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: ff32b3095638af6b2b246b99a5dc9219e0020782
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78402306"
---
# <a name="tutorial-run-a-multi-step-container-workflow-in-the-cloud-when-you-commit-source-code"></a>Tutorial: Ejecución de un flujo de trabajo de contenedor de varios pasos en la nube al confirmar el código fuente

Además de [tareas rápidas](container-registry-tutorial-quick-task.md), ACR Tasks admite flujos de trabajo basados en varios pasos y varios contenedores que se pueden desencadenar automáticamente cuando se confirma el código fuente en un repositorio de Git. 

En este tutorial, aprenderá a usar archivos YAML de ejemplo para definir tareas de varios pasos que compilan, ejecutan e insertan una o varias imágenes de contenedor en un registro cuando confirma el código fuente. Para crear una tarea que solo se automatice la compilación de una única imagen al confirmar el código, consulte [Tutorial: Automatización de las compilaciones de imágenes de contenedor en la nube al confirmar código fuente](container-registry-tutorial-build-task.md) Para información general sobre ACR Tasks, consulte [Automatización de la aplicación de revisiones de sistema operativo y plataforma con ACR Tasks](container-registry-tasks-overview.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Definir una tarea de varios pasos con un archivo YAML
> * Crea una tarea.
> * Opcionalmente, agregar credenciales a la tarea para habilitar el acceso a otro registro
> * Prueba de la tarea
> * Visualización del estado de la tarea
> * Desencadenamiento de la tarea con una confirmación de código

En este tutorial se da por hecho que ya ha realizado los pasos de los [tutoriales anteriores](container-registry-tutorial-quick-task.md). Si aún no lo ha hecho, complete los pasos de la sección [Requisitos previos](container-registry-tutorial-quick-task.md#prerequisites) del tutorial anterior antes de continuar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si quiere usar la CLI de Azure de forma local, debe tener la versión **2.0.62** u otra posterior instalada y registrada con [az login][az-login]. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli].

[!INCLUDE [container-registry-task-tutorial-prereq.md](../../includes/container-registry-task-tutorial-prereq.md)]

## <a name="create-a-multi-step-task"></a>Creación de una tarea de varios pasos

Ahora que ha realizado los pasos necesarios para permitir que ACR Tasks lea el estado de confirmación y cree webhooks en un repositorio, cree una tarea de varios pasos que desencadene la compilación, la ejecución y la inserción de una imagen de contenedor.

### <a name="yaml-file"></a>Archivo YAML

Definirá los pasos de una tarea de varios pasos en un [archivo YAML](container-registry-tasks-reference-yaml.md). La primera tarea de varios pasos de ejemplo de este tutorial se define en el archivo `taskmulti.yaml`, que se encuentra en la raíz del repositorio de GitHub que ha clonado:

```yml
version: v1.0.0
steps:
# Build target image
- build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} -f Dockerfile .
# Run image 
- cmd: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}
  id: test
  detach: true
  ports: ["8080:80"]
- cmd: docker stop test
# Push image
- push:
  - {{.Run.Registry}}/hello-world:{{.Run.ID}}
```

Esta tarea de varios pasos hace lo siguiente:

1. Realiza un paso `build` para crear una imagen del archivo Dockerfile en el directorio de trabajo. La imagen se dirige a `Run.Registry`, el registro donde se ejecuta la tarea, y se etiqueta con un identificador de ejecución único de ACR Tasks. 
1. Realiza un paso `cmd` para ejecutar la imagen en un contenedor temporal. En este ejemplo se inicia un contenedor de larga ejecución en segundo plano y se devuelve el identificador del contenedor; luego, el contenedor se detiene. En un escenario real, podría incluir pasos para probar el contenedor en ejecución y así asegurarse de que se ejecuta correctamente.
1. En un paso `push`, inserta la imagen que se creó en el registro de ejecución.

### <a name="task-command"></a>Comando de tarea

Primero, rellene estas variables de entorno de shell con valores adecuados para el entorno. Este paso no es estrictamente necesario, pero hace que la ejecución de los comandos de varias líneas de la CLI de Azure en este tutorial sea un poco más fácil. Si no rellena estas variables de entorno, debe reemplazar manualmente cada valor siempre que aparezca en los comandos de ejemplo.

[![Insertar inicio](https://shell.azure.com/images/launchcloudshell.png "Inicio de Azure Cloud Shell")](https://shell.azure.com)

```console
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the previous section
```

Ahora, cree la tarea mediante la ejecución del siguiente comando [az acr task create][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name example1 \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file taskmulti.yaml \
    --git-access-token $GIT_PAT
```

Esta tarea especifica que siempre que se confirme código en la rama *maestra* del repositorio especificado por `--context`, ACR Tasks ejecutará la tarea de varios pasos desde el código de esa rama. En el archivo YAML especificado por `--file` desde el repositorio raíz se definen los pasos. 

La salida de un comando [az acr task create][az-acr-task-create] que se ha ejecutado correctamente debe ser parecida a la siguiente:

```output
{
  "agentConfiguration": {
    "cpu": 2
  },
  "creationDate": "2019-05-03T03:14:31.763887+00:00",
  "credentials": null,
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myregistry/providers/Microsoft.ContainerRegistry/registries/myregistry/tasks/taskmulti",
  "location": "westus",
  "name": "example1",
  "platform": {
    "architecture": "amd64",
    "os": "linux",
    "variant": null
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": "Enabled",
  "step": {
    "baseImageDependencies": null,
    "contextAccessToken": null,
    "contextPath": "https://github.com/gituser/acr-build-helloworld-node.git",
    "taskFilePath": "taskmulti.yaml",
    "type": "FileTask",
    "values": [],
    "valuesFilePath": null
  },
  "tags": null,
  "timeout": 3600,
  "trigger": {
    "baseImageTrigger": {
      "baseImageTriggerType": "Runtime",
      "name": "defaultBaseimageTriggerName",
      "status": "Enabled"
    },
    "sourceTriggers": [
      {
        "name": "defaultSourceTriggerName",
        "sourceRepository": {
          "branch": "master",
          "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node.git",
          "sourceControlAuthProperties": null,
          "sourceControlType": "Github"
        },
        "sourceTriggerEvents": [
          "commit"
        ],
        "status": "Enabled"
      }
    ]
  },
  "type": "Microsoft.ContainerRegistry/registries/tasks"
}
```

## <a name="test-the-multi-step-workflow"></a>Prueba del flujo de trabajo de varios pasos

Para probar la tarea de varios pasos, debe desencadenarla manualmente mediante la ejecución del comando [az acr task run][az-acr-task-run]:

```azurecli-interactive
az acr task run --registry $ACR_NAME --name example1
```

De forma predeterminada, el comando `az acr task run` hace streaming de la salida del registro a la consola cuando se ejecuta el comando. La salida muestra el progreso de la ejecución de cada uno de los pasos de la tarea. La salida siguiente está condensada para mostrar los pasos claves.

```output
Queued a run with ID: cf19
Waiting for an agent...
2019/05/03 03:03:31 Downloading source code...
2019/05/03 03:03:33 Finished downloading source code
2019/05/03 03:03:33 Using acb_vol_cfe6bd55-3076-4215-8091-6a81aec3d1b1 as the home volume
2019/05/03 03:03:33 Creating Docker network: acb_default_network, driver: 'bridge'
2019/05/03 03:03:34 Successfully set up Docker network: acb_default_network
2019/05/03 03:03:34 Setting up Docker configuration...
2019/05/03 03:03:34 Successfully set up Docker configuration
2019/05/03 03:03:34 Logging in to registry: myregistry.azurecr.io
2019/05/03 03:03:35 Successfully logged into myregistry.azurecr.io
2019/05/03 03:03:35 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:35 Scanning for dependencies...
2019/05/03 03:03:36 Successfully scanned dependencies
2019/05/03 03:03:36 Launching container with name: acb_step_0
Sending build context to Docker daemon  24.06kB

[...]

Successfully built f669bfd170af
Successfully tagged myregistry.azurecr.io/hello-world:cf19
2019/05/03 03:03:43 Successfully executed container: acb_step_0
2019/05/03 03:03:43 Executing step ID: acb_step_1. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:43 Launching container with name: acb_step_1
279b1cb6e092b64c8517c5506fcb45494cd5a0bd10a6beca3ba97f25c5d940cd
2019/05/03 03:03:44 Successfully executed container: acb_step_1
2019/05/03 03:03:44 Executing step ID: acb_step_2. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:44 Pushing image: myregistry.azurecr.io/hello-world:cf19, attempt 1

[...]

2019/05/03 03:03:46 Successfully pushed image: myregistry.azurecr.io/hello-world:cf19
2019/05/03 03:03:46 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 7.425169)
2019/05/03 03:03:46 Populating digests for step ID: acb_step_0...
2019/05/03 03:03:47 Successfully populated digests for step ID: acb_step_0
2019/05/03 03:03:47 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 0.827129)
2019/05/03 03:03:47 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 2.112113)
2019/05/03 03:03:47 The following dependencies were found:
2019/05/03 03:03:47
- image:
    registry: myregistry.azurecr.io
    repository: hello-world
    tag: cf19
    digest: sha256:6b981a8ca8596e840228c974c929db05c0727d8630465de536be74104693467a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 1a3065388a0238e52865db1c8f3e97492a43444c

Run ID: cf19 was successful after 18s
```

## <a name="trigger-a-build-with-a-commit"></a>Desencadenamiento de una compilación con una confirmación

Ahora que ha probado la tarea ejecutándola manualmente, desencadénela automáticamente con un cambio en el código fuente.

En primer lugar, asegúrese de que se encuentra en el directorio que contiene el clon local del [repositorio][sample-repo]:

```console
cd acr-build-helloworld-node
```

A continuación, ejecute los siguientes comandos para crear, confirmar e insertar un nuevo archivo en la bifurcación del repositorio en GitHub:

```console
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Tasks"
git push origin master
```

Se le pedirá que proporcione sus credenciales de GitHub cuando ejecute el comando `git push`. Proporcione el nombre de usuario de GitHub y escriba el token de acceso personal (PAT) que creó anteriormente para la contraseña.

```azurecli-interactive
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Una vez que ha insertado una confirmación en el repositorio, se desencadena el webhook que creó ACR Tasks y se inicia la tarea en Azure Container Registry. Muestre los registros de la tarea que se está ejecutando actualmente para comprobar y supervisar el progreso de la compilación:

```azurecli-interactive
az acr task logs --registry $ACR_NAME
```

La salida es parecida a la siguiente, que muestra la tarea que se está ejecutando actualmente (o la última que se ejecutó):

```output
Showing logs of the last created run.
Run ID: cf1d

[...]

Run ID: cf1d was successful after 37s
```

## <a name="list-builds"></a>Lista de compilaciones

Para ver una lista de las tareas que ACR Tasks ha completado para el registro, ejecute el comando [az acr task list-runs][az-acr-task-list-runs]:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

La salida del comando debe ser parecida a la siguiente. Se muestran las ejecuciones que ha ejecutado ACR Tasks y aparece "Confirmación de Git" en la columna DESENCADENADOR de la tarea más reciente:

```output
RUN ID    TASK       PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  ---------  ----------  ---------  ---------  --------------------  ----------
cf1d      example1   linux       Succeeded  Commit     2019-05-03T04:16:44Z  00:00:37
cf1c      example1   linux       Succeeded  Commit     2019-05-03T04:16:44Z  00:00:39
cf1b      example1   linux       Succeeded  Manual     2019-05-03T03:10:30Z  00:00:31
cf1a      example1   linux       Succeeded  Commit     2019-05-03T03:09:32Z  00:00:31
cf19      example1   linux       Succeeded  Manual     2019-05-03T03:03:30Z  00:00:21
```

## <a name="create-a-multi-registry-multi-step-task"></a>Creación de una tarea de varios pasos y varios registros

ACR Tasks tiene permisos de forma predeterminada para insertar imágenes en el registro donde se ejecuta la tarea, y extraerlas de este. Puede que quiera ejecutar una tarea de varios pasos dirigida a uno o varios registros además del registro de ejecución. Por ejemplo, podría tener que compilar imágenes en un registro y almacenar las imágenes con etiquetas diferentes en un segundo registro al que accede un sistema de producción. En este ejemplo se muestra cómo crear una tarea de este tipo y proporcionar las credenciales para otro registro.

Si aún no tiene un segundo registro, cree uno para este ejemplo. Si necesita un registro, consulte el [tutorial anterior](container-registry-tutorial-quick-task.md), o [Inicio rápido: Creación de un registro de contenedor con la CLI de Azure](container-registry-get-started-azure-cli.md).

Para crear la tarea, se necesita el nombre del servidor de inicio de sesión del registro, que tiene el formato *mycontainerregistrydate.azurecr.io* (todo en minúsculas). En este ejemplo, el segundo registro se usa para almacenar las imágenes etiquetadas por fecha de compilación.

### <a name="yaml-file"></a>Archivo YAML

La segunda tarea de varios pasos de ejemplo de este tutorial se define en el archivo `taskmulti-multiregistry.yaml`, que se encuentra en la raíz del repositorio de GitHub que ha clonado:

```yml
version: v1.0.0
steps:
# Build target images
- build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} -f Dockerfile .
- build: -t {{.Values.regDate}}/hello-world:{{.Run.Date}} -f Dockerfile .
# Run image 
- cmd: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}
  id: test
  detach: true
  ports: ["8080:80"]
- cmd: docker stop test
# Push images
- push:
  - {{.Run.Registry}}/hello-world:{{.Run.ID}}
  - {{.Values.regDate}}/hello-world:{{.Run.Date}}
```

Esta tarea de varios pasos hace lo siguiente:

1. Ejecuta dos pasos `build` para compilar imágenes del archivo Dockerfile en el directorio de trabajo:
    * La primera se dirige a `Run.Registry`, el registro donde se ejecuta la tarea, y se etiqueta con el identificador de ejecución de ACR Tasks. 
    * La segunda se dirige al registro identificado por el valor de `regDate`, que se establece al crear la tarea (o que se proporciona mediante un archivo externo `values.yaml` que se pasa a `az acr task create`). Esta imagen se etiqueta con la fecha de ejecución.
1. Realiza un paso `cmd` para ejecutar uno de los contenedores generados. En este ejemplo se inicia un contenedor de larga ejecución en segundo plano y se devuelve el identificador del contenedor; luego, el contenedor se detiene. En un escenario real, podría probar un contenedor en ejecución para asegurarse de que se ejecuta correctamente.
1. En un paso `push`, inserta las imágenes que se compilaron, la primera en el registro de ejecución y la segunda en el registro identificado por `regDate`.

### <a name="task-command"></a>Comando de tarea

Con las variables de entorno del shell definidas anteriormente, cree la tarea con el siguiente comando [az acr task create][az-acr-task-create]. Sustituya el nombre del registro por *mycontainerregistrydate*.

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name example2 \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file taskmulti-multiregistry.yaml \
    --git-access-token $GIT_PAT \
    --set regDate=mycontainerregistrydate.azurecr.io
```

### <a name="add-task-credential"></a>Adición de la credencial de la tarea

Para insertar imágenes en el registro identificado por el valor de `regDate`, use el comando [az acr task credential add][az-acr-task-credential-add] para agregar las credenciales de inicio de sesión de ese registro a la tarea.

En este ejemplo, se recomienda crear una [entidad de servicio](container-registry-auth-service-principal.md) con acceso al registro que tenga como ámbito el rol *AcrPush*. Para crear la entidad de servicio, consulte este [script de la CLI de Azure](https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh).

Pase le identificador y la contraseña de aplicación de la entidad de servicio en el siguiente comando `az acr task credential add`:

```azurecli-interactive
az acr task credential add --name example2 \
    --registry $ACR_NAME \
    --login-server mycontainerregistrydate.azurecr.io \
    --username <service-principal-application-id> \
    --password <service-principal-password>
```

La CLI devuelve el nombre del servidor de inicio de sesión del registro que agregó.

### <a name="test-the-multi-step-workflow"></a>Prueba del flujo de trabajo de varios pasos

Como en el ejemplo anterior, para probar la tarea de varios pasos, debe desencadenarla manualmente mediante la ejecución del comando [az acr task run][az-acr-task-run]. Para desencadenar la tarea con una confirmación en el repositorio de Git, consulte la sección [Desencadenamiento de una compilación con una confirmación](#trigger-a-build-with-a-commit).

```azurecli-interactive
az acr task run --registry $ACR_NAME --name example2
```

De forma predeterminada, el comando `az acr task run` hace streaming de la salida del registro a la consola cuando se ejecuta el comando. Como antes, la salida muestra el progreso de la ejecución de cada uno de los pasos de la tarea. La salida está condensada para mostrar los pasos principales.

Salida:

```output
Queued a run with ID: cf1g
Waiting for an agent...
2019/05/03 04:33:39 Downloading source code...
2019/05/03 04:33:41 Finished downloading source code
2019/05/03 04:33:42 Using acb_vol_4569b017-29fe-42bd-83b2-25c45a8ac807 as the home volume
2019/05/03 04:33:42 Creating Docker network: acb_default_network, driver: 'bridge'
2019/05/03 04:33:43 Successfully set up Docker network: acb_default_network
2019/05/03 04:33:43 Setting up Docker configuration...
2019/05/03 04:33:44 Successfully set up Docker configuration
2019/05/03 04:33:44 Logging in to registry: mycontainerregistry.azurecr.io
2019/05/03 04:33:45 Successfully logged into mycontainerregistry.azurecr.io
2019/05/03 04:33:45 Logging in to registry: mycontainerregistrydate.azurecr.io
2019/05/03 04:33:47 Successfully logged into mycontainerregistrydate.azurecr.io
2019/05/03 04:33:47 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:47 Scanning for dependencies...
2019/05/03 04:33:47 Successfully scanned dependencies
2019/05/03 04:33:47 Launching container with name: acb_step_0
Sending build context to Docker daemon  25.09kB

[...]

Successfully tagged mycontainerregistry.azurecr.io/hello-world:cf1g
2019/05/03 04:33:55 Successfully executed container: acb_step_0
2019/05/03 04:33:55 Executing step ID: acb_step_1. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:55 Scanning for dependencies...
2019/05/03 04:33:56 Successfully scanned dependencies
2019/05/03 04:33:56 Launching container with name: acb_step_1
Sending build context to Docker daemon  25.09kB

[...]

Successfully tagged mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z
2019/05/03 04:33:57 Successfully executed container: acb_step_1
2019/05/03 04:33:57 Executing step ID: acb_step_2. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:57 Launching container with name: acb_step_2
721437ff674051b6be63cbcd2fa8eb085eacbf38d7d632f1a079320133182101
2019/05/03 04:33:58 Successfully executed container: acb_step_2
2019/05/03 04:33:58 Executing step ID: acb_step_3. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:58 Launching container with name: acb_step_3
test
2019/05/03 04:34:09 Successfully executed container: acb_step_3
2019/05/03 04:34:09 Executing step ID: acb_step_4. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:34:09 Pushing image: mycontainerregistry.azurecr.io/hello-world:cf1g, attempt 1
The push refers to repository [mycontainerregistry.azurecr.io/hello-world]

[...]

2019/05/03 04:34:12 Successfully pushed image: mycontainerregistry.azurecr.io/hello-world:cf1g
2019/05/03 04:34:12 Pushing image: mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z, attempt 1
The push refers to repository [mycontainerregistrydate.azurecr.io/hello-world]

[...]

2019/05/03 04:34:19 Successfully pushed image: mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z
2019/05/03 04:34:19 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 8.125744)
2019/05/03 04:34:19 Populating digests for step ID: acb_step_0...
2019/05/03 04:34:21 Successfully populated digests for step ID: acb_step_0
2019/05/03 04:34:21 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.009281)
2019/05/03 04:34:21 Populating digests for step ID: acb_step_1...
2019/05/03 04:34:23 Successfully populated digests for step ID: acb_step_1
2019/05/03 04:34:23 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 0.795440)
2019/05/03 04:34:23 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.446775)
2019/05/03 04:34:23 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 9.734973)
2019/05/03 04:34:23 The following dependencies were found:
2019/05/03 04:34:23
- image:
    registry: mycontainerregistry.azurecr.io
    repository: hello-world
    tag: cf1g
    digest: sha256:75354e9edb995e8661438bad9913deed87a185fddd0193811f916d684b71a5d2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 9d9023473c46a5e2c315681b11eb4552ef0faccc
- image:
    registry: mycontainerregistrydate.azurecr.io
    repository: hello-world
    tag: 20190503-043342z
    digest: sha256:75354e9edb995e8661438bad9913deed87a185fddd0193811f916d684b71a5d2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 9d9023473c46a5e2c315681b11eb4552ef0faccc

Run ID: cf1g was successful after 46s
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a crear tareas basadas en varios pasos y varios contenedores que se desencadenan automáticamente cuando se confirma código fuente en un repositorio de Git. Para información sobre las características avanzadas de las tareas de varios pasos, como la ejecución de pasos dependientes y en paralelo, consulte la [referencia de YAML de ACR Tasks](container-registry-tasks-reference-yaml.md). Pase al siguiente tutorial para aprender a crear tareas que desencadenan compilaciones cuando se actualiza la imagen base de la imagen de contenedor.

> [!div class="nextstepaction"]
> [Automatización de compilaciones después de una actualización de imagen base](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add    
[az-login]: /cli/azure/reference-index#az-login

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
