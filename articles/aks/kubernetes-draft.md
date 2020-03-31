---
title: Desarrollo en Azure Kubernetes Service (AKS) con Draft
description: Uso de Draft con AKS y Azure Container Registry
services: container-service
author: zr-msft
ms.topic: article
ms.date: 06/20/2019
ms.author: zarhoads
ms.openlocfilehash: b03256ee65a3c40d8a64d70b877c49e44e68f822
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595228"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-draft"></a>Inicio rápido: Desarrollo en Azure Kubernetes Service (AKS) con Draft

Draft es una herramienta de código abierto que ayuda a empaquetar y ejecutar contenedores de aplicación en un clúster de Kubernetes. Con Draft, puede reimplementar rápidamente una aplicación en Kubernetes a medida que se produzcan cambios en el código, sin tener que confirmar los cambios al control de versiones. Para más información sobre Draft, consulte la [documentación de Draft en GitHub][draft-documentation].

En este artículo se muestra cómo usar Draft para empaquetar y ejecutar una aplicación en AKS.


## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
* [La CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Tener Docker instalado y configurado. Docker proporciona paquetes que lo configuran en cualquier sistema [Mac][docker-for-mac], [Windows][docker-for-windows] o [Linux][docker-for-linux].
* [Helm v2 instalado][helm-install].
* Tener [Draft instalado][draft-documentation].

## <a name="create-an-azure-kubernetes-service-cluster"></a>Creación de un clúster de Azure Kubernetes Service

Cree un clúster de AKS. Los siguientes comandos permiten crear un grupo de recursos llamado MyResourceGroup y un clúster de AKS denominado MyAKS.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --generate-ssh-keys
```

## <a name="create-an-azure-container-registry"></a>Creación de una instancia de Azure Container Registry
Para usar Draft para ejecutar la aplicación en el clúster de AKS, necesita Azure Container Registry para almacenar las imágenes de contenedor. En el ejemplo siguiente se usa [az acr create][az-acr-create] para crear un ACR llamado *MyDraftACR* en el grupo de recursos *MyResourceGroup* con la SKU *Básica*. Debería proporcionar su propio nombre único de registro. El nombre del registro debe ser único dentro de Azure y contener entre 5 y 50 caracteres alfanuméricos. La SKU *básica* es un punto de entrada optimizado para costo con fines de desarrollo que proporciona un equilibrio entre almacenamiento y rendimiento.

```azurecli
az acr create --resource-group MyResourceGroup --name MyDraftACR --sku Basic
```

La salida será similar al del ejemplo siguiente: Tome nota del valor de *loginServer* el ACR, puesto que se usará en un paso posterior. En el ejemplo siguiente, *mydraftacr.azurecr.io* es el valor de *loginServer* para *MyDraftACR*.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyDraftACR",
  "location": "eastus",
  "loginServer": "mydraftacr.azurecr.io",
  "name": "MyDraftACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```


Para que Draft usar la instancia de ACR, primero debe iniciar sesión. Use el comando [az acr login][az-acr-login] para iniciar sesión. En el ejemplo siguiente, va a iniciar sesión un ACR llamado *MyDraftACR*.

```azurecli
az acr login --name MyDraftACR
```

Al finalizar, el comando devuelve un mensaje que indica que el *inicio de sesión se ha realizado correctamente*.

## <a name="create-trust-between-aks-cluster-and-acr"></a>Creación de relación de confianza entre el clúster de AKS y ACR

El clúster de AKS también necesita acceder a su ACR para extraer las imágenes de contenedor y ejecutarlas. Para permitir el acceso al ACR desde AKS establezca una relación de confianza. Para establecer la confianza entre un clúster de AKS y un registro de ACR, conceda permisos para la entidad de servicio de Azure Active Directory que utiliza el clúster AKS para acceder al registro ACR. Los siguientes comandos conceden permisos a la entidad de servicio del clúster *MyAKS* en *MyResourceGroup* al ACR *MyDraftACR* en  *MyResourceGroup*.

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group MyResourceGroup --name MyAKS --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group MyResourceGroup --name MyDraftACR --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

## <a name="connect-to-your-aks-cluster"></a>Conectarse al clúster AKS

Para conectarse al clúster de Kubernetes desde su equipo local, use [kubectl][kubectl], el cliente de la línea de comandos de Kubernetes.

Si usa Azure Cloud Shell, `kubectl` ya está instalado. También lo puede instalar localmente. Para ello debe usar el comando [az aks install-cli][]:

```azurecli
az aks install-cli
```

Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][]. En el ejemplo siguiente se obtienen las credenciales del clúster de AKS llamado *MyAKS* en *MyResourceGroup*:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="create-a-service-account-for-helm"></a>Creación de una cuenta de servicio de Helm

Antes de implementar Helm en un clúster de AKS habilitado para RBAC, necesita una cuenta de servicio y el enlace de rol para el servicio Tiller. Para más información sobre cómo proteger Helm o Tiller en un clúster habilitado para RBAC, consulte [Tiller, Namespaces, and RBAC][tiller-rbac] (Tiller, espacios de nombres y RBAC). Si el clúster de AKS no está habilitado para RBAC, omita este paso.

Cree un archivo denominado `helm-rbac.yaml` y cópielo en el siguiente código YAML:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Cree la cuenta de servicio y el enlace de rol con el comando `kubectl apply`:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="configure-helm"></a>Configuración de Helm
Para implementar un servicio básico de Tiller en un clúster de AKS, use el comando [helm init][helm-init]. Si el clúster no está habilitado para RBAC, quite el argumento `--service-account` y el valor.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="configure-draft"></a>Configuración de Draft

Si no ha configurado Draft en el equipo local, ejecute `draft init`:

```console
$ draft init
Installing default plugins...
Installation of default plugins complete
Installing default pack repositories...
...
Happy Sailing!
```

También debe configurar Draft para usar el valor de *loginServer* de su ACR. El siguiente comando usa `draft config set` para usar `mydraftacr.azurecr.io` como un registro.

```console
draft config set registry mydraftacr.azurecr.io
```

Ha configurado Draft para usar el ACR y Draft puede insertar imágenes de contenedor en el ACR. Cuando Draft ejecuta la aplicación en el clúster de AKS, no se necesitan contraseñas ni secretos para insertar o extraer desde el registro de ACR. Dado que se creó una relación de confianza entre el clúster de AKS y el ACR, la autenticación se produce en el nivel de Azure Resource Manager, mediante Azure Active Directory.

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo

En este inicio rápido se usa [una aplicación de Java de ejemplo del repositorio de GitHub de Draft][example-java]. Clone la aplicación desde GitHub y vaya al directorio `draft/examples/example-java/`.

```console
git clone https://github.com/Azure/draft
cd draft/examples/example-java/
```

## <a name="run-the-sample-application-with-draft"></a>Ejecución de la aplicación de ejemplo con Draft

Use el comando `draft create` para preparar la aplicación.

```console
draft create
```

Este comando crea los artefactos que se usan para ejecutar la aplicación en un clúster de Kubernetes. Estos elementos incluyen un Dockerfile, un gráfico de Helm y un archivo *draft.toml*, que es el archivo de configuración de Draft.

```console
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

Para ejecutar la aplicación de ejemplo en el clúster de AKS, use el comando `draft up`.

```console
draft up
```

Este comando compila Dockerfile para crear una imagen de contenedor, inserta la imagen en ACR e instala el gráfico de Helm para iniciar la aplicación en AKS. La primera vez que ejecute este comando, la inserción y extracción de la imagen de contenedor pueden tardar algo de tiempo. Una vez que los niveles de base se almacenan en caché, el tiempo necesario para implementar la aplicación se reduce drásticamente.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

## <a name="connect-to-the-running-sample-application-from-your-local-machine"></a>Conexión a la aplicación de ejemplo en ejecución desde el equipo local

Para probar la aplicación, use el comando `draft connect`.

```console
draft connect
```

Este comando redirige una conexión segura mediante proxy al pod de Kubernetes. Al terminar, se puede acceder a la aplicación en la dirección URL proporcionada.

```console
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Vaya a la aplicación en un explorador con la dirección URL `localhost` para ver la aplicación de ejemplo. En el ejemplo anterior, la dirección URL es `http://localhost:49804`. Detenga la conexión con `Ctrl+c`.

## <a name="access-the-application-on-the-internet"></a>Acceso a la aplicación en Internet

El paso anterior creó una conexión de proxy al pod de la aplicación en el clúster AKS. A medida que desarrolle y pruebe la aplicación, es posible que quiera que esté disponible en Internet. Para exponer una aplicación en Internet, puede crear un servicio de Kubernetes con un tipo de servicio [LoadBalancer][kubernetes-service-loadbalancer].

Actualice `charts/example-java/values.yaml` para crear un servicio *LoadBalancer*. Cambie el valor de *service.type* de *ClusterIP* a *LoadBalancer*.

```yaml
...
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
...
```

Guarde los cambios, cierre el archivo y ejecute `draft up` para volver a ejecutar la aplicación.

```console
draft up
```

El servicio puede tardar unos minutos en devolver una dirección IP pública. Para supervisar el progreso, utilice el comando `kubectl get service` con el parámetro *watch*:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Navegue hasta el equilibrador de carga de la aplicación en un explorador mediante el valor de *EXTERNAL-IP* para ver la aplicación de ejemplo. En el ejemplo anterior, la dirección IP es `52.175.224.118`.

## <a name="iterate-on-the-application"></a>Iteración en la aplicación

Puede iterar la aplicación al realizar cambios localmente y volver a ejecutar `draft up`.

Actualice el mensaje devuelto en la línea 7 [ de src/main/java/helloworld/Hello.java][example-java-hello-l7].

```java
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
```

Ejecute el comando `draft up` para volver a implementar la aplicación:

```console
$ draft up

Draft Up Started: 'example-java': 01CMZC9RF0TZT7XPWGFCJE15X4
example-java: Building Docker Image: SUCCESS ⚓  (25.0202s)
example-java: Pushing Docker Image: SUCCESS ⚓  (7.1457s)
example-java: Releasing Application: SUCCESS ⚓  (3.5773s)
Inspect the logs with `draft logs 01CMZC9RF0TZT7XPWGFCJE15X4`
```

Para ver la aplicación actualizada, vaya a la dirección IP del equilibrador de carga de nuevo y compruebe que los cambios aparecen.

## <a name="delete-the-cluster"></a>Eliminación del clúster

Cuando ya no se necesite el clúster, use el comando [az group delete][az-group-delete] para quitar el grupo de recursos, el clúster de AKS, el registro de contenedores, las imágenes de contenedor almacenadas allí y todos los recursos relacionados.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Cuando elimina el clúster, la entidad de servicio Azure Active Directory que utiliza el clúster de AKS no se quita. Para conocer los pasos que hay que realizar para quitar la entidad de servicio, consulte [Consideraciones principales y eliminación de AKS][sp-delete].

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el uso de Draft, consulte la documentación de Draft en GitHub.

> [!div class="nextstepaction"]
> [Documentación de Draft][draft-documentation]


[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-create]: /cli/azure/acr#az-acr-login
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[kubernetes-ingress]: ./ingress-basic.md

[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[example-java]: https://github.com/Azure/draft/tree/master/examples/example-java
[example-java-hello-l7]: https://github.com/Azure/draft/blob/master/examples/example-java/src/main/java/helloworld/Hello.java#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[helm-init]: https://v2.helm.sh/docs/helm/#helm-init
[helm-install]: https://v2.helm.sh/docs/using_helm/#installing-helm
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[tiller-rbac]: https://v2.helm.sh/docs/using_helm/#tiller-namespaces-and-rbac
