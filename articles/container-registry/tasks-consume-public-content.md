---
title: Flujo de trabajo de las tareas para administrar contenido de registro público
description: Cree un flujo de trabajo de Azure Container Registry Tasks para hacer seguimiento, administrar y consumir contenido de imágenes públicas en un registro de contenedor de Azure privado.
author: SteveLasker
ms.topic: article
ms.author: stevelas
ms.date: 10/29/2020
ms.custom: ''
ms.openlocfilehash: 261604b66d393723b35b472415b8840b047bc36e
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133459"
---
# <a name="how-to-consume-and-maintain-public-content-with-azure-container-registry-tasks"></a>Consumo y mantenimiento de contenido público con Azure Container Registry Tasks

En este artículo se proporciona un flujo de trabajo de ejemplo de Azure Container Registry para ayudarlo a administrar el consumo y mantenimiento de contenido público:

1. Importar copias locales de imágenes públicas dependientes.
1. Validar las imágenes públicas a través de un examen de seguridad y pruebas funcionales.
1. Promover las imágenes a registros privados para uso interno.
1. Desencadenar actualizaciones de imagen base para aplicaciones que dependen del contenido público.
1. Usar [Azure Container Registry Tasks](container-registry-tasks-overview.md) para automatizar este flujo de trabajo.

El flujo de trabajo se resume en la imagen siguiente:

![Flujo de trabajo del consumo de contenido público](./media/tasks-consume-public-content/consuming-public-content-workflow.png)

El flujo de trabajo de importación controlado le permite administrar las dependencias de su organización con respecto a artefactos administrados de manera externa; por ejemplo, imágenes originadas a partir de registros públicos, incluidos [Docker Hub][docker-hub], [GCR][gcr], [Quay][quay], [GitHub Container Registry][ghcr], [Microsoft Container Registry][mcr] o, incluso, otras instancias de [Azure Container Registry][acr]. 

Para información sobre los riesgos que presentan las dependencias de contenido público y cómo usar Azure Container Registry para mitigarlos, consulte esta [entrada del blog de OCI sobre el consumo de contenido público][oci-consuming-public-content] y el artículo [Administración del contenido público con Azure Container Registry](buffer-gate-public-content.md).

Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar este tutorial. Se recomienda la CLI de Azure versión 2.10 o posterior. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-cli].

## <a name="scenario-overview"></a>Información general de escenario

![Componentes del flujo de trabajo de importación](./media/tasks-consume-public-content/consuming-public-content-objects.png)

En este tutorial se realiza la configuración de lo siguiente:

1. Tres **registros de contenedor** , que representan:
   * Una instancia de [Docker Hub][docker-hub] (`publicregistry`) simulada para admitir el cambio de la imagen base.
   * El registro de equipo (`contoso`) para compartir imágenes privadas.
   * El registro compartido de la empresa o del equipo (`baseartifacts`) para el contenido público importado.
1. Una **tarea de ACR** en cada registro. Las tareas:  
   1. Crean una imagen `node` pública simulada.
   1. Importan y validan la imagen `node` al registro compartido de la empresa o del equipo.
   1. Crean e implementan la imagen `hello-world`.
1. **Definiciones de las tareas de ACR** , incluidas las configuraciones de:
1. Una colección de **credenciales de registro** , que son punteros a un almacén de claves.
1. Una colección de **secretos** , disponible dentro de un archivo `acr-task.yaml`, que son punteros a un almacén de claves.
1. Una colección de **valores configurados** que se usan dentro de un archivo `acr-task.yaml`.
1. Un **almacén de claves de Azure** para proteger todos los secretos.
1. Una **instancia de contenedor de Azure** , que hospeda la aplicación de compilación `hello-world`.

## <a name="prerequisites"></a>Requisitos previos

En los pasos siguientes se configuran los valores de los recursos que se crean y usan en el tutorial.

### <a name="set-environment-variables"></a>Establecimiento de variables de entorno

Configure variables únicas para su entorno. Seguimos los procedimientos recomendados para colocar recursos con contenido duradero en su propio grupo de recursos a fin de minimizar la eliminación accidental. Sin embargo, puede colocarlos en un solo grupo de recursos si así lo desea.

Los ejemplos que aparecen en este artículo tienen un formato para el shell de Bash.

```bash
# Set the three registry names, must be globally unique:
REGISTRY_PUBLIC=publicregistry
REGISTRY_BASE_ARTIFACTS=contosobaseartifacts
REGISTRY=contoso

# set the location all resources will be created in:
RESOURCE_GROUP_LOCATION=eastus

# default resource groups
REGISTRY_PUBLIC_RG=${REGISTRY_PUBLIC}-rg
REGISTRY_BASE_ARTIFACTS_RG=${REGISTRY_BASE_ARTIFACTS}-rg
REGISTRY_RG=${REGISTRY}-rg

# fully qualified registry urls
REGISTRY_DOCKERHUB_URL=docker.io
REGISTRY_PUBLIC_URL=${REGISTRY_PUBLIC}.azurecr.io
REGISTRY_BASE_ARTIFACTS_URL=${REGISTRY_BASE_ARTIFACTS}.azurecr.io
REGISTRY_URL=${REGISTRY}.azurecr.io

# Azure key vault for storing secrets, name must be globally unique
AKV=acr-task-credentials
AKV_RG=${AKV}-rg

# ACI for hosting the deployed application
ACI=hello-world-aci
ACI_RG=${ACI}-rg
```

### <a name="git-repositories-and-tokens"></a>Tokens y repositorios de Git

Para simular su entorno, debe bifurcar cada uno de estos repositorios de Git en repositorios que pueda administrar. 

* https://github.com/importing-public-content/base-image-node.git
* https://github.com/importing-public-content/import-baseimage-node.git
* https://github.com/importing-public-content/hello-world.git

Luego, actualice estas variables para los repositorios bifurcados.

El `:main` anexado al final de las direcciones URL de Git representa la rama predeterminada del repositorio.

```bash
GIT_BASE_IMAGE_NODE=https://github.com/<your-fork>/base-image-node.git#main
GIT_NODE_IMPORT=https://github.com/<your-fork>/import-baseimage-node.git#main
GIT_HELLO_WORLD=https://github.com/<your-fork>/hello-world.git#main
```

Necesita un [token de acceso de GitHub(PAT)][git-token] para que ACR Tasks clone y establezca webhooks de Git. Para conocer los pasos que se siguen para crear un token con los permisos necesarios para un repositorio privado, consulte [Creación de un token de acceso personal de GitHub](container-registry-tutorial-build-task.md#create-a-github-personal-access-token). 

```bash
GIT_TOKEN=<set-git-token-here>
```

### <a name="docker-hub-credentials"></a>Credenciales de Docker Hub  
Para evitar las solicitudes de identidad y limitación al extraer imágenes de Docker Hub, cree un [token de Docker Hub][docker-hub-tokens]. Luego, agregue estas variables de entorno:

```bash
REGISTRY_DOCKERHUB_USER=<yourusername>
REGISTRY_DOCKERHUB_PASSWORD=<yourtoken>
```

### <a name="create-registries"></a>Creación de registros

Use comandos de la CLI de Azure para crear tres registros de contenedor de nivel Premium, cada uno en su propio grupo de recursos:

```azurecli-interactive
az group create --name $REGISTRY_PUBLIC_RG --location $RESOURCE_GROUP_LOCATION
az acr create --resource-group $REGISTRY_PUBLIC_RG --name $REGISTRY_PUBLIC --sku Premium

az group create --name $REGISTRY_BASE_ARTIFACTS_RG --location $RESOURCE_GROUP_LOCATION
az acr create --resource-group $REGISTRY_BASE_ARTIFACTS_RG --name $REGISTRY_BASE_ARTIFACTS --sku Premium

az group create --name $REGISTRY_RG --location $RESOURCE_GROUP_LOCATION
az acr create --resource-group $REGISTRY_RG --name $REGISTRY --sku Premium
```

### <a name="create-key-vault-and-set-secrets"></a>Creación de un almacén de claves y establecimiento de secretos

Cree un almacén de claves:

```azurecli-interactive
az group create --name $AKV_RG --location $RESOURCE_GROUP_LOCATION
az keyvault create --resource-group $AKV_RG --name $AKV
```

Establezca el nombre de usuario y el token de Docker Hub en el almacén de claves:

```azurecli-interactive
az keyvault secret set \
--vault-name $AKV \
--name registry-dockerhub-user \
--value $REGISTRY_DOCKERHUB_USER

az keyvault secret set \
--vault-name $AKV \
--name registry-dockerhub-password \
--value $REGISTRY_DOCKERHUB_PASSWORD
```

Establezca y compruebe un PAT de Git en el almacén de claves:

```azurecli-interactive
az keyvault secret set --vault-name $AKV --name github-token --value $GIT_TOKEN

az keyvault secret show --vault-name $AKV --name github-token --query value -o tsv
```

### <a name="create-resource-group-for-an-azure-container-instance"></a>Creación de un grupo de recursos para una instancia de contenedor de Azure

Este grupo de recursos se usa en una tarea posterior al implementar la imagen `hello-world`.

```azurecli-interactive
az group create --name $ACI_RG --location $RESOURCE_GROUP_LOCATION
```

## <a name="create-public-node-base-image"></a>Creación de una imagen `node` base pública

Para simular la imagen `node` en Docker Hub, cree una [tarea de ACR][acr-task] para crear y mantener la imagen pública. Esta configuración permite simular los cambios que realizan los mantenedores de la imagen `node`.

```azurecli-interactive
az acr task create \
  --name node-public \
  -r $REGISTRY_PUBLIC \
  -f acr-task.yaml \
  --context $GIT_BASE_IMAGE_NODE \
  --git-access-token $(az keyvault secret show \
                        --vault-name $AKV \
                        --name github-token \
                        --query value -o tsv) \
  --set REGISTRY_FROM_URL=${REGISTRY_DOCKERHUB_URL}/ \
  --assign-identity
```

Para evitar la limitación de Docker, agregue [credenciales de Docker Hub][docker-hub-tokens] a la tarea. Es posible usar el comando [acr task credentials][acr-task-credentials] para pasar credenciales de Docker a cualquier registro, incluido Docker Hub.

```azurecli-interactive
az acr task credential add \
  -n node-public \
  -r $REGISTRY_PUBLIC \
  --login-server $REGISTRY_DOCKERHUB_URL \
  -u https://${AKV}.vault.azure.net/secrets/registry-dockerhub-user \
  -p https://${AKV}.vault.azure.net/secrets/registry-dockerhub-password \
  --use-identity [system]
```

Conceda a la tarea acceso para leer los valores del almacén de claves:

```azurecli-interactive
az keyvault set-policy \
  --name $AKV \
  --resource-group $AKV_RG \
  --object-id $(az acr task show \
                  --name node-public \
                  --registry $REGISTRY_PUBLIC \
                  --query identity.principalId --output tsv) \
  --secret-permissions get
```

Las [tareas las pueden desencadenar][acr-task-triggers] confirmaciones de Git, actualizaciones de la imagen base, temporizadores o ejecuciones manuales. 

Ejecute la tarea manualmente para generar la imagen `node`:

```azurecli-interactive
az acr task run -r $REGISTRY_PUBLIC -n node-public
```

Muestre la imagen en el registro público simulado:

```azurecli-interactive
az acr repository show-tags -n $REGISTRY_PUBLIC --repository node
```

## <a name="create-the-hello-world-image"></a>Creación de la imagen `hello-world`

Cree una imagen `hello-world` en función de la imagen `node` pública simulada.

### <a name="create-token-for-pull-access-to-simulated-public-registry"></a>Creación de un token para acceso de extracción en un registro público simulado

Cree un [token de acceso][acr-tokens] al registro público simulado, limitado a `pull`. Luego, establézcalo en el almacén de claves:

```azurecli-interactive
az keyvault secret set \
  --vault-name $AKV \
  --name "registry-${REGISTRY_PUBLIC}-user" \
  --value "registry-${REGISTRY_PUBLIC}-user"

az keyvault secret set \
  --vault-name $AKV \
  --name "registry-${REGISTRY_PUBLIC}-password" \
  --value $(az acr token create \
              --name "registry-${REGISTRY_PUBLIC}-user" \
              --registry $REGISTRY_PUBLIC \
              --scope-map _repositories_pull \
              -o tsv \
              --query credentials.passwords[0].value)
```

### <a name="create-token-for-pull-access-by-azure-container-instances"></a>Creación de un token para acceso de extracción a través de Azure Container Instances

Cree un [token de acceso][acr-tokens] al registro en que se hospeda la imagen `hello-world`, limitado a la extracción. Luego, establézcalo en el almacén de claves:

```azurecli-interactive
az keyvault secret set \
  --vault-name $AKV \
  --name "registry-${REGISTRY}-user" \
  --value "registry-${REGISTRY}-user"

az keyvault secret set \
  --vault-name $AKV \
  --name "registry-${REGISTRY}-password" \
  --value $(az acr token create \
              --name "registry-${REGISTRY}-user" \
              --registry $REGISTRY \
              --repository hello-world content/read \
              -o tsv \
              --query credentials.passwords[0].value)
```

### <a name="create-task-to-build-and-maintain-hello-world-image"></a>Creación de una tarea para crear y mantener una imagen `hello-world`

El comando siguiente crea una tarea a partir de la definición que se encuentra en el archivo `acr-tasks.yaml` del repositorio `hello-world`. Los pasos de la tarea crean la imagen `hello-world` y, luego, la implementan en Azure Container Instances. El grupo de recursos para Azure Container Instances se creó en una sección anterior. Al llamar a `az container create` en la tarea con solo una diferencia en `image:tag`, la tarea se implementa en la misma instancia a lo largo de todo este tutorial.

```azurecli-interactive
az acr task create \
  -n hello-world \
  -r $REGISTRY \
  -f acr-task.yaml \
  --context $GIT_HELLO_WORLD \
  --git-access-token $(az keyvault secret show \
                        --vault-name $AKV \
                        --name github-token \
                        --query value -o tsv) \
  --set REGISTRY_FROM_URL=${REGISTRY_PUBLIC_URL}/ \
  --set KEYVAULT=$AKV \
  --set ACI=$ACI \
  --set ACI_RG=$ACI_RG \
  --assign-identity
```

Agregue las credenciales a la tarea para el registro público simulado:

```azurecli-interactive
az acr task credential add \
  -n hello-world \
  -r $REGISTRY \
  --login-server $REGISTRY_PUBLIC_URL \
  -u https://${AKV}.vault.azure.net/secrets/registry-${REGISTRY_PUBLIC}-user \
  -p https://${AKV}.vault.azure.net/secrets/registry-${REGISTRY_PUBLIC}-password \
  --use-identity [system]
```

Conceda a la tarea acceso para leer los valores del almacén de claves:

```azurecli-interactive
az keyvault set-policy \
  --name $AKV \
  --resource-group $AKV_RG \
  --object-id $(az acr task show \
                  --name hello-world \
                  --registry $REGISTRY \
                  --query identity.principalId --output tsv) \
  --secret-permissions get
```

Conceda a la tarea acceso para crear y administrar Azure Container Instances mediante la concesión de acceso al grupo de recursos:

```azurecli-interactive
az role assignment create \
  --assignee $(az acr task show \
  --name hello-world \
  --registry $REGISTRY \
  --query identity.principalId --output tsv) \
  --scope $(az group show -n $ACI_RG --query id -o tsv) \
  --role owner
```

Una vez creada y configurada la tarea, ejecútela para crear e implementar la imagen `hello-world`:

```azurecli-interactive
az acr task run -r $REGISTRY -n hello-world
```

Una vez creada, obtenga la dirección IP del contenedor en el que se hospeda la imagen `hello-world`.

```azurecli-interactive
az container show \
  --resource-group $ACI_RG \
  --name ${ACI} \
  --query ipAddress.ip \
  --out tsv
```

En el explorador, vaya a la dirección IP para ver la aplicación en ejecución.

## <a name="update-the-base-image-with-a-questionable-change"></a>Actualización de la imagen base con un cambio "cuestionable"

En esta sección se simula un cambio en la imagen base que podría causar problemas en el entorno.

1. Abra `Dockerfile` en el repositorio `base-image-node` bifurcado.
1. Cambie `BACKGROUND_COLOR` a `Orange` para simular el cambio.

```Dockerfile
ARG REGISTRY_NAME=
FROM ${REGISTRY_NAME}node:15-alpine
ENV NODE_VERSION 15-alpine
ENV BACKGROUND_COLOR Orange
```

Confirme el cambio y use un comando de inspección en ACR Tasks para empezar a hacer automáticamente la creación.

Ejecute un comando de inspección para ver cuando la tarea se empieza a ejecutar:

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY_PUBLIC -o table
```

A la larga, debería ver el estado STATUS `Succeeded` en función de un desencadenador TRIGGER de `Commit`:

```azurecli-interactive
RUN ID    TASK      PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  --------  ----------  ---------  ---------  --------------------  ----------
ca4       hub-node  linux       Succeeded  Commit     2020-10-24T05:02:29Z  00:00:22
```

Escriba **Ctrl+C** para salir del comando de inspección y consulte los registros de la ejecución más reciente:

```azurecli-interactive
az acr task logs -r $REGISTRY_PUBLIC
```

Una vez completada la imagen `node`, use el comando `watch` para que ACR Tasks empiece automáticamente a crear la imagen `hello-world`:

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY -o table
```

A la larga, debería ver el estado STATUS `Succeeded` en función de un desencadenador TRIGGER de `Image Update`:

```azurecli-interactive
RUN ID    TASK         PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  -----------  ----------  ---------  ------------  --------------------  ----------
dau       hello-world  linux       Succeeded  Image Update  2020-10-24T05:08:45Z  00:00:31
```

Escriba **Ctrl+C** para salir del comando de inspección y consulte los registros de la ejecución más reciente:

```azurecli-interactive
az acr task logs -r $REGISTRY
```

Cuando termine, obtenga la dirección IP del sitio en el que se hospeda la imagen `hello-world` actualizada:

```azurecli-interactive
az container show \
  --resource-group $ACI_RG \
  --name ${ACI} \
  --query ipAddress.ip \
  --out tsv
```

En el explorador, vaya al sitio, el que debería tener un fondo naranja (cuestionable).

### <a name="checking-in"></a>Registro

En este punto, tiene creada una imagen `hello-world` que se genera automáticamente en las confirmaciones de Git y cambios en la imagen `node` base. En este ejemplo, la tarea se crea según una imagen base de Azure Container Registry, pero se podría usar cualquier registro compatible.

La actualización de la imagen base vuelve a desencadenar automáticamente la ejecución de la tarea cuando se actualiza la imagen `node`. Como se ve aquí, no todas las actualizaciones son deseadas.

## <a name="gated-imports-of-public-content"></a>Importaciones controladas de contenido público

Para evitar que los cambios ascendentes detengan las cargas de trabajo críticas, se pueden agregar exámenes de seguridad y pruebas funcionales.

En esta sección, creará una tarea de ACR para:

* Crear una imagen de prueba.
* Ejecutar un script de prueba funcional `./test.sh` en la imagen de prueba.
* Si la imagen se prueba correctamente, importe la imagen pública al registro **baseimages**.

### <a name="add-automation-testing"></a>Incorporación de pruebas de automatización

Para canalizar cualquier contenido ascendente, se implementan pruebas automatizadas. En este ejemplo, se proporciona un elemento `test.sh` que comprueba el elemento `$BACKGROUND_COLOR`. Si ocurre un error en la prueba, se devuelve un código `EXIT_CODE` de `1`, lo que genera un error en el paso de la tarea de ACR y finaliza la ejecución de la tarea. Las pruebas se pueden expandir en cualquier forma de herramientas, incluidos resultados de registro. El mecanismo de control se administra mediante una respuesta de tipo "pasa/no pasa" en el script que se reproduce aquí:

```bash
if [ ""$(echo $BACKGROUND_COLOR | tr '[:lower:]' '[:upper:]') = 'RED' ]; then
    echo -e "\e[31mERROR: Invalid Color:\e[0m" ${BACKGROUND_COLOR}
    EXIT_CODE=1
else
  echo -e "\e[32mValidation Complete - No Known Errors\e[0m"
fi
exit ${EXIT_CODE}
```
### <a name="task-yaml"></a>Código YAML para las tareas 

Revise el archivo `acr-task.yaml` del repositorio `import-baseimage-node`, que lleva a cabo los pasos siguientes:

1. Compile la imagen base de prueba con el Dockerfile siguiente:
    ```dockerfile
    ARG REGISTRY_FROM_URL=
    FROM ${REGISTRY_FROM_URL}node:15-alpine
    WORKDIR /test
    COPY ./test.sh .
    CMD ./test.sh
    ```
1. Al terminar, ejecute el contenedor, que ejecuta `./test.sh`, para validar la imagen.
1. Solo si el proceso se completa correctamente, ejecute los pasos de importación, que se controlan con `when: ['validate-base-image']`.

```yaml
version: v1.1.0
steps:
  - id: build-test-base-image
    # Build off the base image we'll track
    # Add a test script to do unit test validations
    # Note: the test validation image isn't saved to the registry
    # but the task logs captures log validation results
    build: >
      --build-arg REGISTRY_FROM_URL={{.Values.REGISTRY_FROM_URL}}
      -f ./Dockerfile
      -t {{.Run.Registry}}/node-import:test
      .
  - id: validate-base-image
    # only continues if node-import:test returns a non-zero code
    when: ['build-test-base-image']
    cmd: "{{.Run.Registry}}/node-import:test"
  - id: pull-base-image
    # import the public image to base-artifacts
    # Override the stable tag,
    # and create a unique tag to enable rollback
    # to a previously working image
    when: ['validate-base-image']
    cmd: >
        docker pull {{.Values.REGISTRY_FROM_URL}}node:15-alpine
  - id: retag-base-image
    when: ['pull-base-image']
    cmd: docker tag {{.Values.REGISTRY_FROM_URL}}node:15-alpine {{.Run.Registry}}/node:15-alpine
  - id: retag-base-image-unique-tag
    when: ['pull-base-image']
    cmd: docker tag {{.Values.REGISTRY_FROM_URL}}node:15-alpine {{.Run.Registry}}/node:15-alpine-{{.Run.ID}}
  - id: push-base-image
    when: ['retag-base-image', 'retag-base-image-unique-tag']
    push:
    - "{{.Run.Registry}}/node:15-alpine"
    - "{{.Run.Registry}}/node:15-alpine-{{.Run.ID}}"
```

### <a name="create-task-to-import-and-test-base-image"></a>Creación de una tarea para importar y probar una imagen base

```azurecli-interactive
  az acr task create \
  --name base-import-node \
  -f acr-task.yaml \
  -r $REGISTRY_BASE_ARTIFACTS \
  --context $GIT_NODE_IMPORT \
  --git-access-token $(az keyvault secret show \
                        --vault-name $AKV \
                        --name github-token \
                        --query value -o tsv) \
  --set REGISTRY_FROM_URL=${REGISTRY_PUBLIC_URL}/ \
  --assign-identity
```

Agregue las credenciales a la tarea para el registro público simulado:

```azurecli-interactive
az acr task credential add \
  -n base-import-node \
  -r $REGISTRY_BASE_ARTIFACTS \
  --login-server $REGISTRY_PUBLIC_URL \
  -u https://${AKV}.vault.azure.net/secrets/registry-${REGISTRY_PUBLIC}-user \
  -p https://${AKV}.vault.azure.net/secrets/registry-${REGISTRY_PUBLIC}-password \
  --use-identity [system]
```

Conceda a la tarea acceso para leer los valores del almacén de claves:

```azurecli-interactive
az keyvault set-policy \
  --name $AKV \
  --resource-group $AKV_RG \
  --object-id $(az acr task show \
                  --name base-import-node \
                  --registry $REGISTRY_BASE_ARTIFACTS \
                  --query identity.principalId --output tsv) \
  --secret-permissions get
```

Ejecute la tarea de importación:

```azurecli-interactive
az acr task run -n base-import-node -r $REGISTRY_BASE_ARTIFACTS
```

> [!NOTE]
> Si se produce un error en la tarea debido a `./test.sh: Permission denied`, asegúrese de que el script tenga permisos de ejecución y vuelva a confirmar con el repositorio de Git:
>```bash
>chmod +x ./test.sh
>```

## <a name="update-hello-world-image-to-build-from-gated-node-image"></a>Actualización de la imagen `hello-world` para crear a partir de la imagen `node` controlada

Cree un [token de acceso][acr-tokens] para acceder al registro de artefactos base, limitado a `read` desde el repositorio `node`. Luego, establézcalo en el almacén de claves:

```azurecli-interactive
az keyvault secret set \
  --vault-name $AKV \
  --name "registry-${REGISTRY_BASE_ARTIFACTS}-user" \
  --value "registry-${REGISTRY_BASE_ARTIFACTS}-user"

az keyvault secret set \
  --vault-name $AKV \
  --name "registry-${REGISTRY_BASE_ARTIFACTS}-password" \
  --value $(az acr token create \
              --name "registry-${REGISTRY_BASE_ARTIFACTS}-user" \
              --registry $REGISTRY_BASE_ARTIFACTS \
              --repository node content/read \
              -o tsv \
              --query credentials.passwords[0].value)
```

Agregue credenciales a la tarea **hello-world** correspondiente al registro de artefactos base:

```azurecli-interactive
az acr task credential add \
  -n hello-world \
  -r $REGISTRY \
  --login-server $REGISTRY_BASE_ARTIFACTS_URL \
  -u https://${AKV}.vault.azure.net/secrets/registry-${REGISTRY_BASE_ARTIFACTS}-user \
  -p https://${AKV}.vault.azure.net/secrets/registry-${REGISTRY_BASE_ARTIFACTS}-password \
  --use-identity [system]
```

Actualice la tarea para cambiar `REGISTRY_FROM_URL` para usar el registro `BASE_ARTIFACTS`.

```azurecli-interactive
az acr task update \
  -n hello-world \
  -r $REGISTRY \
  --set KEYVAULT=$AKV \
  --set REGISTRY_FROM_URL=${REGISTRY_BASE_ARTIFACTS_URL}/ \
  --set ACI=$ACI \
  --set ACI_RG=$ACI_RG
```

Ejecute la tarea **hello-world** para cambiar su dependencia de imagen base:

```azurecli-interactive
az acr task run -r $REGISTRY -n hello-world
```

## <a name="update-the-base-image-with-a-valid-change"></a>Actualización de la imagen base con un cambio "válido"

1. Abra `Dockerfile` en el repositorio `base-image-node`.
1. Cambie `BACKGROUND_COLOR` a `Green` para simular un cambio válido.

```Dockerfile
ARG REGISTRY_NAME=
FROM ${REGISTRY_NAME}node:15-alpine
ENV NODE_VERSION 15-alpine
ENV BACKGROUND_COLOR Green
```

Confirme el cambio y supervise la secuencia de actualizaciones:

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY_PUBLIC -o table
```

Una vez en ejecución, escriba **Ctrl+C** y supervise los registros:

```azurecli-interactive
az acr task logs -r $REGISTRY_PUBLIC
```

Al terminar, supervise la tarea **base-image-import** :

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY_BASE_ARTIFACTS -o table
```

Una vez en ejecución, escriba **Ctrl+C** y supervise los registros:

```azurecli-interactive
az acr task logs -r $REGISTRY_BASE_ARTIFACTS
```

Al terminar, supervise la tarea **hello-world** :

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY -o table
```

Una vez en ejecución, escriba **Ctrl+C** y supervise los registros:

```azurecli-interactive
az acr task logs -r $REGISTRY
```

Cuando termine, obtenga la dirección IP del sitio en el que se hospeda la imagen `hello-world` actualizada:

```azurecli-interactive
az container show \
  --resource-group $ACI_RG \
  --name ${ACI} \
  --query ipAddress.ip \
  --out tsv
```

En el explorador, vaya al sitio, que debe tener un fondo verde (válido).

### <a name="view-the-gated-workflow"></a>Visualización del flujo de trabajo controlado

Repita los pasos de la sección anterior con un color de fondo rojo.

1. Abra `Dockerfile` en el repositorio `base-image-node`.
1. Cambie `BACKGROUND_COLOR` a `Red` para simular un cambio no válido.

```Dockerfile
ARG REGISTRY_NAME=
FROM ${REGISTRY_NAME}node:15-alpine
ENV NODE_VERSION 15-alpine
ENV BACKGROUND_COLOR Red
```

Confirme el cambio y supervise la secuencia de actualizaciones:

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY_PUBLIC -o table
```

Una vez en ejecución, escriba **Ctrl+C** y supervise los registros:

```azurecli-interactive
az acr task logs -r $REGISTRY_PUBLIC
```

Al terminar, supervise la tarea **base-image-import** :

```azurecli-interactive
watch -n1 az acr task list-runs -r $REGISTRY_BASE_ARTIFACTS -o table
```

Una vez en ejecución, escriba **Ctrl+C** y supervise los registros:

```azurecli-interactive
az acr task logs -r $REGISTRY_BASE_ARTIFACTS
```

En este punto, debería ver que la tarea **base-import-node** no pasa la validación y detiene la secuencia para publicar una actualización de `hello-world`. La salida es parecida a esta:

```console
[...]
2020/10/30 03:57:39 Launching container with name: validate-base-image
Validating Image
NODE_VERSION: 15-alpine
BACKGROUND_COLOR: Red
ERROR: Invalid Color: Red
2020/10/30 03:57:40 Container failed during run: validate-base-image. No retries remaining.
failed to run step ID: validate-base-image: exit status 1
```

### <a name="publish-an-update-to-hello-world"></a>Publicación de una actualización de `hello-world`

Los cambios de la imagen `hello-world` seguirán usando la última imagen `node` validada.

Cualquier otro cambio en la imagen `node` base que pase las validaciones controladas desencadenará actualizaciones de imagen base de la imagen `hello-world`.

## <a name="cleaning-up"></a>Limpieza

Cuando ya no los necesite, elimine los recursos que se usan en este artículo.

```azurecli-interactive
az group delete -n $REGISTRY_RG --no-wait -y
az group delete -n $REGISTRY_PUBLIC_RG --no-wait -y
az group delete -n $REGISTRY_BASE_ARTIFACTS_RG --no-wait -y
az group delete -n $AKV_RG --no-wait -y
az group delete -n $ACI_RG --no-wait -y
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, usó tareas de ACR para crear un flujo de trabajo controlado automatizado para introducir en el entorno imágenes base actualizadas. Consulte la información relacionada para administrar las imágenes en Azure Container Registry.


* [Recomendaciones para el etiquetado y el control de versiones de las imágenes de contenedor](container-registry-image-tag-version.md)
* [Bloqueo de una imagen de contenedor en un registro de contenedor de Azure](container-registry-image-lock.md)

[install-cli]:                  /cli/azure/install-azure-cli
[acr]:                          https://aka.ms/acr
[acr-repo-permissions]:         https://aka.ms/acr/repo-permissions
[acr-task]:                     https://aka.ms/acr/tasks
[acr-task-triggers]:            container-registry-tasks-overview.md#task-scenarios
[acr-task-credentials]:       container-registry-tasks-authentication-managed-identity.md#4-optional-add-credentials-to-the-task
[acr-tokens]:                   https://aka.ms/acr/tokens
[aci]:                          https://aka.ms/aci
[alpine-public-image]:          https://hub.docker.com/_/alpine
[docker-hub]:                   https://hub.docker.com
[docker-hub-tokens]:            https://hub.docker.com/settings/security
[git-token]:                    https://github.com/settings/tokens
[gcr]:                          https://cloud.google.com/container-registry
[ghcr]:                         https://docs.github.com/en/free-pro-team@latest/packages/getting-started-with-github-container-registry/about-github-container-registry
[helm-charts]:                  https://helm.sh
[mcr]:                          https://aka.ms/mcr
[nginx-public-image]:           https://hub.docker.com/_/nginx
[oci-artifacts]:                https://aka.ms/acr/artifacts
[oci-consuming-public-content]: https://opencontainers.org/posts/blog/2020-10-30-consuming-public-content/
[opa]:                          https://www.openpolicyagent.org/
[quay]:                         https://quay.io




