---
title: Almacenamiento de gráficos de Helm
description: Información sobre cómo almacenar gráficos de Helm para las aplicaciones de Kubernetes mediante repositorios en Azure Container Registry
ms.topic: article
ms.date: 03/20/2020
ms.openlocfilehash: 3f1a68258b758380a66b63e3c3137f1d460d288c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399387"
---
# <a name="push-and-pull-helm-charts-to-an-azure-container-registry"></a>Inserción y extracción de gráficos de Helm en Azure Container Registry

Para administrar e implementar rápidamente aplicaciones de Kubernetes, puede usar el [administrador de paquetes de código abierto de Helm][helm]. Con Helm, los paquetes de aplicación se definen como [gráficos](https://helm.sh/docs/topics/charts/) que se recopilan y almacenan en un [repositorio de gráficos de Helm](https://helm.sh/docs/topics/chart_repository/).

En este artículo se muestra cómo hospedar repositorios gráficos de Helm en un registro de contenedor de Azure mediante una instalación de Helm 3 o Helm 2. En la mayoría de los escenarios, creará y cargará sus propios gráficos para las aplicaciones que desarrolla. Para obtener más información sobre cómo crear sus propios gráficos de Helm, consulte la [guía para desarrolladores de plantillas de gráficos][develop-helm-charts] (en inglés). También puede almacenar un gráfico de Helm existente desde otro repositorio de Helm.

> [!IMPORTANT]
> La compatibilidad con gráficos de Helm en Azure Container Registry está actualmente en versión preliminar. Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="helm-3-or-helm-2"></a>¿Helm 3 o Helm 2?

Para almacenar, administrar e instalar gráficos de Helm, se usa un cliente de Helm y la CLI de Helm. Las versiones principales del cliente de Helm incluyen Helm 3 y Helm 2. Helm 3 admite un nuevo formato de gráfico y ya no instala el componente del lado servidor de Tiller. Para obtener más información sobre las diferencias de versión, vea las [preguntas más frecuentes sobre la versión](https://helm.sh/docs/faq/). Si ya ha implementado gráficos de Helm 2 anteriormente, consulte [Migración de Helm v2 a v3](https://helm.sh/docs/topics/v2_v3_migration/).

Puede usar Helm 3 o Helm 2 para hospedar gráficos de Helm en Azure Container Registry, con flujos de trabajo específicos de cada versión:

* [Cliente de Helm 3](#use-the-helm-3-client): use `helm chart` comandos de la CLI de Helm para administrar gráficos en el registro como [artefactos OCI](container-registry-image-formats.md#oci-artifacts)
* [Cliente de Helm 2](#use-the-helm-2-client): use comandos [az acr helm][az-acr-helm] en la CLI de Azure para agregar y administrar su instancia de Container Registry como un repositorio de gráficos de Helm.

### <a name="additional-information"></a>Información adicional

* En la mayoría de los escenarios, se recomienda usar el flujo de trabajo de Helm 3 con comandos nativos `helm chart` para administrar gráficos como artefactos OCI.
* Puede usar el flujo de trabajo y los comandos [az acr helm][az-acr-helm] heredados de la CLI de Azure con los gráficos y el cliente de Helm 3. Sin embargo, algunos comandos como `az acr helm list` no son compatibles con los gráficos de Helm 3.
* A partir de Helm 3, los comandos [az acr helm][az-acr-helm] se admiten principalmente para la compatibilidad con el formato de los gráficos y el cliente de Helm 2. Actualmente no se planea el desarrollo futuro de estos comandos.

## <a name="use-the-helm-3-client"></a>Uso del cliente de Helm 3

### <a name="prerequisites"></a>Prerrequisitos

- **Una instancia de Azure Container Registry** en la suscripción a Azure. Si es necesario, cree un registro mediante [Azure Portal](container-registry-get-started-portal.md) o la [CLI de Azure](container-registry-get-started-azure-cli.md).
- **Versión de cliente de Helm 3.1.0 o posterior**: ejecute `helm version` para buscar la versión actual. Para más información sobre cómo instalar y usar Helm, consulte [Instalación de Helm][helm-install].
- **Un clúster de Kubernetes** donde instalar un gráfico de Helm. Si es necesario, cree [un clúster de Azure Kubernetes Service][aks-quickstart]. 
- **CLI de Azure versión 2.0.71 o posterior**: ejecute `az --version` para buscar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

### <a name="high-level-workflow"></a>Flujo de trabajo de alto nivel

Con **Helm 3**:

* Puede crear uno o varios repositorios de Helm en una instancia de Azure Container Registry.
* Almacene gráficos de Helm 3 en un registro como [artefactos OCI](container-registry-image-formats.md#oci-artifacts). Actualmente, la compatibilidad con Helm 3 para OCI es *experimental*.
* Autentíquese con el registro mediante el comando `helm registry login`.
* Use `helm chart` comandos de la CLI de Helm para insertar, extraer y administrar gráficos de Helm en un registro
* Use `helm install` para instalar gráficos en un clúster de Kubernetes desde una caché del repositorio local.

Consulte las secciones siguientes para ver ejemplos.

### <a name="enable-oci-support"></a>Habilitar la compatibilidad de OCI

Establezca la siguiente variable de entorno para habilitar la compatibilidad de OCI en el cliente de Helm 3. Actualmente, esta compatibilidad es experimental. 

```console
export HELM_EXPERIMENTAL_OCI=1
```

### <a name="create-a-sample-chart"></a>Crear un gráfico de ejemplo

Cree un gráfico de prueba con los siguientes comandos:

```console
mkdir helmtest

cd helmtest
helm create hello-world
```

Como ejemplo básico, cambie el directorio a la carpeta `templates` y, en primer lugar, elimine el contenido aquí:

```console
cd hello-world/templates
rm -rf *
```

En la carpeta `templates`, cree un archivo denominado `configmap.yaml` con el siguiente contenido:

```console
cat <<EOF > configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: hello-world-configmap
data:
  myvalue: "Hello World"
EOF
```

Para más información sobre cómo crear y ejecutar este ejemplo, consulte [Introducción](https://helm.sh/docs/chart_template_guide/getting_started/) en los documentos de Helm.

### <a name="save-chart-to-local-registry-cache"></a>Guardar el gráfico en la caché del registro local

Cambie el directorio al subdirectorio `hello-world`. A continuación, ejecute `helm chart save` para guardar una copia del gráfico localmente y crear también un alias con el nombre completo del registro (todo en minúsculas) y el repositorio y la etiqueta de destino. 

En el ejemplo siguiente, el nombre del registro es *mycontainerregistry*, el repositorio de destino es *hello-world* y la etiqueta del gráfico de destino es *v1*, pero sustituye los valores de su entorno:

```console
cd ..
helm chart save . hello-world:v1
helm chart save . mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Ejecute `helm chart list` para confirmar que ha guardado los gráficos en la caché del registro local. La salida es parecida a esta:

```console
REF                                                      NAME            VERSION DIGEST  SIZE            CREATED
hello-world:v1                                           hello-world       0.1.0   5899db0 3.2 KiB        2 minutes 
mycontainerregistry.azurecr.io/helm/hello-world:v1       hello-world       0.1.0   5899db0 3.2 KiB        2 minutes
```

### <a name="authenticate-with-the-registry"></a>Autenticación con el registro

Ejecute el comando `helm registry login` de la CLI de Helm 3 para [autenticarse con el del registro](container-registry-authentication.md) mediante las credenciales apropiadas para su escenario.

Por ejemplo, cree una entidad de servicio de Azure Active Directory [con permisos de extracción e inserción](container-registry-auth-service-principal.md#create-a-service-principal) (rol AcrPush) para el registro. A continuación, proporcione las credenciales de la entidad de servicio para `helm registry login`. En el ejemplo siguiente se proporciona la contraseña mediante una variable de entorno:

```console
echo $spPassword | helm registry login mycontainerregistry.azurecr.io \
  --username <service-principal-id> \
  --password-stdin
```

### <a name="push-chart-to-azure-container-registry"></a>Insertar un gráfico en Azure Container Registry

Ejecute el comando `helm chart push` de la CLI de Helm 3 para insertar el gráfico al repositorio de destino completo:

```console
helm chart push mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Después de una inserción correcta, la salida es similar a:

```output
The push refers to repository [mycontainerregistry.azurecr.io/helm/hello-world]
ref:     mycontainerregistry.azurecr.io/helm/hello-world:v1
digest:  5899db028dcf96aeaabdadfa5899db025899db025899db025899db025899db02
size:    3.2 KiB
name:    hello-world
version: 0.1.0
```

### <a name="list-charts-in-the-repository"></a>Lista de gráficos en el repositorio

Al igual que con las imágenes almacenadas en una instancia de Azure Container Registry, puede usar comandos [az acr repository][az-acr-repository] para mostrar los repositorios que hospedan sus gráficos, así como los manifiestos y las etiquetas de estos. 

Por ejemplo, ejecute [az acr repository show][az-acr-repository-show] para ver las propiedades del repositorio que creó en el paso anterior:

```azurecli
az acr repository show \
  --name mycontainerregistry \
  --repository helm/hello-world
```

La salida es parecida a esta:

```output
{
  "changeableAttributes": {
    "deleteEnabled": true,
    "listEnabled": true,
    "readEnabled": true,
    "writeEnabled": true
  },
  "createdTime": "2020-03-20T18:11:37.6701689Z",
  "imageName": "helm/hello-world",
  "lastUpdateTime": "2020-03-20T18:11:37.7637082Z",
  "manifestCount": 1,
  "registry": "mycontainerregistry.azurecr.io",
  "tagCount": 1
}
```

Ejecute el comando [az acr repository show-manifests][az-acr-repository-show-manifests] para ver los detalles del gráfico almacenado en el repositorio. Por ejemplo:

```azurecli
az acr repository show-manifests \
  --name mycontainerregistry \
  --repository helm/hello-world --detail
```

La salida, abreviada en este ejemplo, muestra un valor de `configMediaType` de `application/vnd.cncf.helm.config.v1+json`:

```output
[
  {
    [...]
    "configMediaType": "application/vnd.cncf.helm.config.v1+json",
    "createdTime": "2020-03-20T18:11:37.7167893Z",
    "digest": "sha256:0c03b71c225c3ddff53660258ea16ca7412b53b1f6811bf769d8c85a1f0663ee",
    "imageSize": 3301,
    "lastUpdateTime": "2020-03-20T18:11:37.7167893Z",
    "mediaType": "application/vnd.oci.image.manifest.v1+json",
    "tags": [
      "v1"
    ]
```

### <a name="pull-chart-to-local-cache"></a>Extraer el gráfico en la caché local

Para instalar un gráfico de Helm en Kubernetes, el gráfico debe estar en la memoria caché local. En este ejemplo, ejecute primero `helm chart remove` para quitar el gráfico local existente denominado `mycontainerregistry.azurecr.io/helm/hello-world:v1`:

```console
helm chart remove mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Ejecute `helm chart pull` para descargar el gráfico desde Azure Container Registry en la memoria caché local:

```console
helm chart pull mycontainerregistry.azurecr.io/helm/hello-world:v1
```

### <a name="export-helm-chart"></a>Exportar el gráfico de Helm

Para seguir trabajando con el gráfico, expórtelo a un directorio local mediante `helm chart export`. Por ejemplo, exporte el gráfico que extrajo al directorio `install`:

```console
helm chart export mycontainerregistry.azurecr.io/helm/hello-world:v1 \
  --destination ./install
```

Para ver información del gráfico exportado en el repositorio, ejecute el comando `helm show chart` en el directorio donde lo exportó.

```console
cd install
helm show chart hello-world
```

Helm devuelve información detallada sobre la versión más reciente del gráfico, tal como se muestra en la siguiente salida de ejemplo:

```output
apiVersion: v2
appVersion: 1.16.0
description: A Helm chart for Kubernetes
name: hello-world
type: application
version: 0.1.0    
```

### <a name="install-helm-chart"></a>Instalar el gráfico de Helm

Ejecute `helm install` para instalar el gráfico de Helm que extrajo en la caché local y exportó. Especifique un nombre de versión, como *myhelmtest* o bien pase el parámetro `--generate-name`. Por ejemplo:

```console
helm install myhelmtest ./hello-world
```

La salida después de la instalación correcta del gráfico es similar a:

```console
NAME: myhelmtest
LAST DEPLOYED: Fri Mar 20 14:14:42 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

Para comprobar la instalación, ejecute el comando `helm get manifest`. El comando devuelve los datos de YAML en el archivo de plantilla `configmap.yaml`.

Ejecute `helm uninstall` para desinstalar la versión del gráfico en el clúster:

```console
helm uninstall myhelmtest
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Eliminar un gráfico de Helm desde el repositorio

Para eliminar un gráfico del repositorio, use el comando [az acr repository delete][az-acr-repository-delete]. Ejecute el siguiente comando y confirme la operación cuando se le solicite:

```azurecli
az acr repository delete --name mycontainerregistry --image helm/hello-world:v1
```

## <a name="use-the-helm-2-client"></a>Uso del cliente de Helm 2

### <a name="prerequisites"></a>Prerrequisitos

- **Una instancia de Azure Container Registry** en la suscripción a Azure. Si es necesario, cree un registro mediante [Azure Portal](container-registry-get-started-portal.md) o la [CLI de Azure](container-registry-get-started-azure-cli.md).
- **Versión de cliente Helm 2.11.0 (no una versión RC) o versión posterior**: ejecute `helm version` para buscar la versión actual. También necesitará un servidor de Helm (Tiller) inicializado dentro de un clúster de Kubernetes. Si es necesario, cree [un clúster de Azure Kubernetes Service][aks-quickstart]. Para más información sobre cómo instalar y usar Helm, consulte [Instalación de Helm][helm-install-v2].
- **CLI de Azure versión 2.0.46 o posterior**: ejecute `az --version` para buscar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

### <a name="high-level-workflow"></a>Flujo de trabajo de alto nivel

Con **Helm 2**:

* Configure su instancia de Azure Container Registry como un repositorio de gráficos de Helm *único*. Azure Container Registry administra la definición del índice a medida que agrega y quita gráficos en el repositorio.
* Realice la autenticación con su instancia de Azure Container Registry mediante la CLI de Azure, que luego actualizará el cliente de Helm automáticamente con el URI y las credenciales del registro. No es necesario especificar manualmente esta información del registro, por lo que las credenciales no se exponen en el historial de comandos.
* Use los comandos [az acr helm][az-acr-helm] en la CLI de Azure para agregar su instancia de Azure Container Registry como un repositorio de gráficos de Helm, así como para insertar y administrar gráficos. Estos comandos de la CLI de Azure encapsulan los comandos del cliente de Helm 2.
* Agregue el repositorio de gráficos de su instancia de Azure Container Registry al índice del repositorio local de Helm, que admite la búsqueda de gráficos.
* Use `helm install` para instalar gráficos en un clúster de Kubernetes desde una caché del repositorio local.

Consulte las secciones siguientes para ver ejemplos.

### <a name="add-repository-to-helm-client"></a>Agregar un repositorio al cliente de Helm

Agregue el repositorio de gráficos de Helm de Azure Container Registry al cliente de Helm mediante el comando [az acr helm repo add][az-acr-helm-repo-add]. Este comando obtiene un token de autenticación del Azure Container Registry que usa el cliente de Helm. El token de autenticación tiene una validez de 3 horas. De forma similar a `docker login`, puede ejecutar este comando en sesiones futuras de la CLI para autenticar el cliente de Helm con el repositorio de gráficos de Helm de Azure Container Registry:

```azurecli
az acr helm repo add --name mycontainerregistry
```

### <a name="add-a-sample-chart-to-the-repository"></a>Agregar un gráfico de ejemplo al repositorio

En primer lugar, cree un directorio local en *~/acr-helm* y después descargue el gráfico *stable/wordpress* existente:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm repo update
helm fetch stable/wordpress
```

Escriba `ls` para mostrar el gráfico descargado y anote la versión de Wordpress que se incluye en el nombre de archivo. El comando `helm fetch stable/wordpress` no ha especificado una versión determinada, por lo que se capturó la versión *más reciente*. En la siguiente salida de ejemplo, el gráfico de Wordpress es de la versión *8.1.0*:

```output
wordpress-8.1.0.tgz
```

Inserte el gráfico en el repositorio de gráficos de Helm en Azure Container Registry mediante el comando [az acr helm push][az-acr-helm-push] de la CLI de Azure. Especifique el nombre de su gráfico de Helm descargado en el paso anterior, como *wordpress-8.1.0.tgz*:

```azurecli
az acr helm push --name mycontainerregistry wordpress-8.1.0.tgz
```

Transcurridos unos instantes, la CLI de Azure notifica que se ha guardado el gráfico, como se muestra en la siguiente salida de ejemplo:

```output
{
  "saved": true
}
```

### <a name="list-charts-in-the-repository"></a>Lista de gráficos en el repositorio

Para usar el gráfico cargado en el paso anterior, se debe actualizar el índice de repositorios de Helm local. Puede volver a indexar los repositorios en el cliente Helm o usar la CLI de Azure para actualizar el índice de repositorios. Este paso debe completarse cada vez que agregue un gráfico al repositorio:

```azurecli
az acr helm repo add --name mycontainerregistry
```

Con un gráfico almacenado en el repositorio y el índice actualizado disponible localmente, puede usar los comandos de cliente de Helm regulares para buscar o instalar. Para ver todos los gráficos del repositorio, use el comando `helm search` y proporcione su propio nombre de Azure Container Registry:

```console
helm search mycontainerregistry
```

Se muestra el gráfico de Wordpress que se insertó en el paso anterior, como se muestra en la siguiente salida de ejemplo:

```output
NAME                  CHART VERSION    APP VERSION    DESCRIPTION
helmdocs/wordpress    8.1.0           5.3.2          Web publishing platform for building blogs and websites.
```

También puede enumerar los gráficos con la CLI de Azure mediante [az acr helm list][az-acr-helm-list]:

```azurecli
az acr helm list --name mycontainerregistry
```

### <a name="show-information-for-a-helm-chart"></a>Mostrar la información de un gráfico de Helm

Para ver información de un gráfico específico del repositorio, puede usar el comando `helm inspect`.

```console
helm inspect mycontainerregistry/wordpress
```

Cuando no se proporciona ningún número de versión, se usa la versión *más reciente*. Helm devuelve información detallada sobre el gráfico, tal como se muestra en la siguiente salida de ejemplo condensada:

```output
apiVersion: v1
appVersion: 5.3.2
description: Web publishing platform for building blogs and websites.
engine: gotpl
home: http://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: Bitnami
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 8.1.0
[...]
```

También puede mostrar la información de un gráfico con el comando [az acr helm show][az-acr-helm-show] de la CLI de Azure. Nuevamente, de forma predeterminada se devuelve la versión *más reciente* de un gráfico. Puede anexar `--version` para mostrar una versión específica de un gráfico, como *8.1.0*:

```azurecli
az acr helm show --name mycontainerregistry wordpress
```

### <a name="install-a-helm-chart-from-the-repository"></a>Instalar un gráfico de Helm desde el repositorio

El gráfico de Helm del repositorio se instala mediante la especificación del nombre del repositorio y el nombre del gráfico. Use el cliente de Helm para instalar el gráfico de Wordpress:

```console
helm install mycontainerregistry/wordpress
```

> [!TIP]
> Si inserta en el repositorio de gráfico de Helm de Azure Container Registry y regresa posteriormente en una nueva sesión CLI, el cliente de Helm local necesita un token de autenticación actualizado. Para obtener un nuevo token de autenticación, use el comando [az acr helm repo add][az-acr-helm-repo-add].

Durante el proceso de instalación deben completarse los siguientes pasos:

- El cliente de Helm busca el índice del repositorio local.
- El gráfico correspondiente se descarga desde el repositorio de Azure Container Registry.
- El gráfico se implementa mediante el Tiller en el clúster de Kubernetes.

A medida que continúe la instalación, siga las instrucciones de la salida del comando para ver las direcciones URL y las credenciales de Wordpress. También puede ejecutar el comando `kubectl get pods` para ver los recursos de Kubernetes implementados mediante el gráfico de Helm:

```output
NAME                                    READY   STATUS    RESTARTS   AGE
wordpress-1598530621-67c77b6d86-7ldv4   1/1     Running   0          2m48s
wordpress-1598530621-mariadb-0          1/1     Running   0          2m48s
[...]
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Eliminar un gráfico de Helm desde el repositorio

Para eliminar un gráfico desde el repositorio, use el comando [az acr helm delete][az-acr-helm-delete]. Especifique el nombre del gráfico, como *wordpress*, y la versión que desea eliminar, como *8.1.0*.

```azurecli
az acr helm delete --name mycontainerregistry wordpress --version 8.1.0
```

Si desea eliminar todas las versiones del gráfico con nombre, omita el parámetro `--version`.

El gráfico se sigue devolviendo al ejecutar `helm search`. Nuevamente, el cliente de Helm no actualiza automáticamente la lista de gráficos disponibles en un repositorio. Para actualizar el índice de repositorio del cliente de Helm, use de nuevo el comando [az acr helm repo add][az-acr-helm-repo-add]:

```azurecli
az acr helm repo add --name mycontainerregistry
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre cómo crear e implementar sus propios gráficos de Helm, consulte cómo [desarrollar gráficos de Helm][develop-helm-charts].
* Más información sobre la instalación de aplicaciones con Helm en [Azure Kubernetes Service (AKS)](../aks/kubernetes-helm.md).
* Los gráficos de helm pueden usarse como parte del proceso de compilación de contenedor. Para más información, consulte [Uso de Azure Container Registry Tasks][acr-tasks].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-v2]: https://v2.helm.sh/docs/using_helm/#installing-helm
[develop-helm-charts]: https://helm.sh/docs/chart_template_guide/
[semver2]: https://semver.org/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az-configure
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-helm]: /cli/azure/acr/helm
[az-acr-repository]: /cli/azure/acr/repository
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-helm-repo-add]: /cli/azure/acr/helm/repo#az-acr-helm-repo-add
[az-acr-helm-push]: /cli/azure/acr/helm#az-acr-helm-push
[az-acr-helm-list]: /cli/azure/acr/helm#az-acr-helm-list
[az-acr-helm-show]: /cli/azure/acr/helm#az-acr-helm-show
[az-acr-helm-delete]: /cli/azure/acr/helm#az-acr-helm-delete
[acr-tasks]: container-registry-tasks-overview.md
