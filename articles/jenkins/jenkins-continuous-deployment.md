---
title: 'Tutorial: Implementación desde GitHub en Azure Kubernetes Service (AKS) con Jenkins'
description: Configuración de Jenkins para la integración continua (CI) desde GitHub y la implementación continua (CD) en Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
ms.author: zarhoads
ms.topic: article
ms.date: 01/09/2019
ms.openlocfilehash: eb48a8558aab6c7a832efe45650686d9df0d7426
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624507"
---
# <a name="tutorial-deploy-from-github-to-azure-kubernetes-service-aks-with-jenkins-continuous-integration-and-deployment"></a>Tutorial: Implementación desde GitHub en Azure Kubernetes Service (AKS) con la integración e implementación continuas de Jenkins

En este tutorial se implementa una aplicación de ejemplo de GitHub en un clúster de [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes) mediante la integración (CI) e implementación continuas (CD) en Jenkins. De este modo, cuando actualice la aplicación mediante la inserción de confirmaciones a GitHub, Jenkins automáticamente ejecuta una nueva compilación de contenedor, inserta imágenes de contenedor en Azure Container Registry (ACR) y, después, ejecuta la aplicación en AKS. 

En este tutorial va a completar estas tareas:

> [!div class="checklist"]
> * Implementar una aplicación de votación de Azure de ejemplo en un clúster de AKS.
> * Crear un proyecto de Jenkins básico.
> * Configurar credenciales para que Jenkins interactúe con ACR.
> * Crear un trabajo de compilación de Jenkins y webhook de GitHub para las compilaciones automatizadas.
> * Probar la canalización de CI/CD para actualizar una aplicación en AKS en función de las confirmaciones de código de GitHub.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita estos elementos:

- Conocimientos básicos de Kubernetes, Git, CI/CD e imágenes de contenedor

- Un [clúster de AKS](../aks/kubernetes-walkthrough.md) y `kubectl` configurado con las [credenciales del clúster de AKS](/cli/azure/aks#az-aks-get-credentials).

- Un [registro de Azure Container Registry (ACR)](../container-registry/container-registry-get-started-azure-cli.md), el nombre del servidor de inicio de sesión de ACR y el clúster de AKS configurados para [autenticarse con el registro de ACR](../aks/cluster-container-registry-integration.md).

- CLI de Azure versión 2.0.46 o posterior instalada y configurada. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte  [Install Azure CLI](/cli/azure/install-azure-cli) (Instalación de la CLI de Azure).

- [Docker instalado](https://docs.docker.com/install/) en el sistema de desarrollo

- Una cuenta de GitHub, un [token de acceso personal a GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) y el cliente Git instalado en el sistema de desarrollo

- Si al implementar Jenkins proporciona su propia instancia de Jenkins en lugar de utilizar un script como el de este ejemplo, su instancia de Jenkins necesitará [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) y que [Docker esté instalado y configurado](https://docs.docker.com/install/).

## <a name="prepare-your-app"></a>Preparación de la aplicación

En este artículo, se usa una aplicación de votación de Azure de ejemplo que contiene una interfaz web hospedada en uno o varios pods y un segundo pod que hospeda Redis para el almacenamiento de datos temporales. Antes de integrar Jenkins y AKS para las implementaciones automatizadas, primero prepare e implemente manualmente la aplicación de votación de Azure en el clúster de AKS. Esta implementación manual es la primera versión de la aplicación y permite ver la aplicación en acción.

> [!NOTE]
> La aplicación de ejemplo de voto de Azure usa un pod de Linux que está programado para ejecutarse en un nodo de Linux. El flujo que se describe en este artículo también funciona para un pod de Windows Server que se ha programado en un nodo de Windows Server.

Bifurque el siguiente repositorio de GitHub para la aplicación de ejemplo - [https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis). Para bifurcar el repositorio en su propia cuenta de GitHub, seleccione el botón **Fork** (Bifurcar) de la esquina superior derecha.

Clone la bifurcación para el sistema de desarrollo. Asegúrese de que usa la URL de la bifurcación al clonar este repositorio:

```console
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Cambie al directorio de la bifurcación clonada:

```console
cd azure-voting-app-redis
```

Para crear las imágenes de contenedor necesarias para la aplicación de ejemplo, use el archivo *docker-compose.yaml* con `docker-compose`:

```console
docker-compose up -d
```

Se extraen las imágenes bases necesarias y los contenedores de aplicación creados. Luego puede usar el comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) para ver la imagen creada. Se han descargado o creado tres imágenes. La imagen `azure-vote-front` contiene la aplicación y usa la imagen `nginx-flask` como base. La imagen `redis` se usa para iniciar una instancia de Redis:

```
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Antes de poder insertar la imagen de contenedor *azure-vote-front* en ACR, debe obtener el servidor de inicio de sesión de ACR con el comando [az acr list](/cli/azure/acr#az-acr-list). En el ejemplo siguiente se obtiene la dirección del servidor de inicio de sesión de ACR de un registro en el grupo de recursos denominado *myResourceGroup*:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Use el comando [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) para etiquetar la imagen con el nombre del servidor de inicio de sesión de ACR y un número de versión `v1`. Proporcione su propio nombre `<acrLoginServer>` obtenido en el paso anterior:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Por último, inserte la imagen *azure-vote-front* en el registro de Azure Container Registry. De nuevo, reemplace `<acrLoginServer>` con el nombre del servidor de inicio de sesión de su propio registro de ACR, como `myacrregistry.azurecr.io`:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-the-sample-application-to-aks"></a>Implementar la aplicación de ejemplo en AKS

Para implementar la aplicación de ejemplo en el clúster de AKS, puede usar el archivo de manifiesto de Kubernetes en la raíz del repositorio de votación de Azure. Abra el archivo de manifiesto *azure-vote-all-in-one-redis.yaml* con un editor como `vi`. Reemplace `microsoft` por el nombre del servidor de inicio de sesión de ACR. Este valor se encuentra en la línea **47** del archivo de manifiesto:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Después, use el comando [kubectl apply](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply) para ejecutar la aplicación en el clúster de AKS:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Se crea un servicio de equilibrador de carga de Kubernetes que expone la aplicación en Internet. Este proceso puede tardar unos minutos. Para supervisar el progreso de la implementación del equilibrador de carga, use el comando [kubectl get service](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) con el argumento `--watch`. Una vez que la dirección *EXTERNAL-IP* haya cambiado de *pendiente* a una *dirección IP*, use `Control + C` para detener el proceso de inspección de kubectl.

```console
$ kubectl get service azure-vote-front --watch

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.215.27   <pending>     80:30747/TCP   22s
azure-vote-front   LoadBalancer   10.0.215.27   40.117.57.239   80:30747/TCP   2m
```

Para ver la aplicación en acción, abra un explorador web en la dirección IP externa del servicio. Se muestra la aplicación de votación de Azure, como en este ejemplo:

![Aplicación de votación de ejemplo de Azure en AKS](media/jenkins-continuous-deployment/azure-vote.png)

## <a name="deploy-jenkins-to-an-azure-vm"></a>Implementar Jenkins en una máquina virtual de Azure

Para implementar rápidamente Jenkins para usarlo en este artículo, puede usar este script para implementar una máquina virtual de Azure, configurar el acceso de red y completar una instalación básica de Jenkins. Para la autenticación entre Jenkins y el clúster de AKS, el script copia el archivo de configuración de Kubernetes del sistema de desarrollo en el sistema Jenkins.

> [!WARNING]
> Este script de ejemplo tiene fines de demostración para aprovisionar rápidamente un entorno de Jenkins que se ejecuta en una máquina virtual de Azure. Usa la extensión de script personalizado de Azure para configurar una máquina virtual y, a continuación, mostrar las credenciales necesarias. Su *~/.kube/config* se copia en la máquina virtual de Jenkins.

Ejecute los siguientes comandos para descargar y ejecutar el script. Debe revisar el contenido de cualquier script antes de ejecutarlo - [https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh](https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh).

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Se tarda unos pocos minutos en crear la máquina virtual e implementar los componentes necesarios para Docker y Jenkins. Cuando finaliza el script, se genera una dirección para el servidor de Jenkins y una clave para desbloquear el panel, como se muestra en la salida del ejemplo siguiente:

```
Open a browser to http://40.115.43.83:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

Abra un explorador web en la URL mostrada y escriba la clave de desbloqueo. Siga las indicaciones en pantalla para completar la configuración de Jenkins:

- Elija **Install suggested plugins** (Instalar complementos sugeridos).
- Cree el primer usuario administrador. Escriba un nombre de usuario, como por ejemplo, *azureuser* y proporcione su propia contraseña segura. Por último, escriba un nombre completo y la dirección de correo electrónico.
- Seleccione **Save and Finish** (Guardar y finalizar).
- Cuando esté listo Jenkins, seleccione **Start using Jenkins** (Comenzar a usar Jenkins).
    - Si el explorador web muestra una página en blanco cuando empiece a usar Jenkins, reinicie el servicio Jenkins. Para reiniciar el servicio, use un SSH para la dirección IP pública de la instancia de Jenkins y escriba `sudo service jenkins restart`. Una vez reiniciado el servicio, actualice el explorador web.
- Inicie sesión en Jenkins con el nombre de usuario y la contraseña que creó en el proceso de instalación.

## <a name="create-a-jenkins-environment-variable"></a>Crear una variable de entorno de Jenkins

Una variable de entorno de Jenkins se usa para contener el nombre del servidor de inicio de sesión de ACR. Se hace referencia a esta variable durante el trabajo de compilación de Jenkins. Para crear esta variable de entorno, siga estos pasos:

- En el lado izquierdo del portal de Jenkins, seleccione **Manage Jenkins** (Administrar Jenkins) > **Configure System** (Configurar sistema).
- En **Propiedades globales**, seleccione **Variables de entorno**. Agregue una variable con el nombre `ACR_LOGINSERVER` y el valor del servidor de inicio de sesión de ACR.

    ![Variables de entorno de Jenkins](media/jenkins-continuous-deployment/env-variables.png)

- Cuando haya finalizado, haga clic en **Guardar** en la página de configuración de Jenkins.

## <a name="create-a-jenkins-credential-for-acr"></a>Crear una credencial de Jenkins para ACR

Para permitir que Jenkins compile y luego inserte imágenes de contenedor actualizadas en ACR, debe especificar las credenciales de ACR. Esta autenticación se puede usar para las entidades de servicio de Azure Active Directory. En los requisitos previos, se ha configurado la entidad de servicio para los clústeres de AKS con permisos de *lector* para el registro de ACR. Estos permisos permiten que el clúster de AKS *inserte* imágenes desde el registro de ACR. Durante el proceso de CI/CD, Jenkins genera nuevas imágenes de contenedor en función de las actualizaciones de aplicaciones y luego debe *insertar* esas imágenes en el registro de Azure Container Registry. Para la separación de roles y permisos, ahora puede configurar una entidad de servicio de Jenkins con permisos de *colaborador* en el registro de ACR.

### <a name="create-a-service-principal-for-jenkins-to-use-acr"></a>Crear una entidad de servicio para Jenkins para que use ACR

Primero, cree una entidad de servicio mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac):

```azurecli
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "626dd8ea-042d-4043-a8df-4ef56273670f",
  "displayName": "azure-cli-2018-09-28-22-19-34",
  "name": "http://azure-cli-2018-09-28-22-19-34",
  "password": "1ceb4df3-c567-4fb6-955e-f95ac9460297",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Anote el valor de *appId* y *password* que se muestra en la salida. Estos valores se usan en los siguientes pasos para configurar el recurso de credencial de Jenkins.

Obtenga el identificador de recurso del registro de Azure Container Registry mediante el comando [az acr show](/cli/azure/acr#az-acr-show) y almacénelo como una variable. Indique su nombre del grupo de recursos y el nombre de ACR:

```azurecli
ACR_ID=$(az acr show --resource-group myResourceGroup --name <acrLoginServer> --query "id" --output tsv)
```

Ahora cree una asignación de roles para asignar los derechos de *colaborador* de la entidad de servicio al registro de Azure Container Registry. En el ejemplo siguiente, proporcione su propio *appId* que se muestra en la salida de un comando anterior para crear la entidad de servicio:

```azurecli
az role assignment create --assignee 626dd8ea-042d-4043-a8df-4ef56273670f --role Contributor --scope $ACR_ID
```

### <a name="create-a-credential-resource-in-jenkins-for-the-acr-service-principal"></a>Crear un recurso de credencial en Jenkins para la entidad de servicio de ACR

Con la asignación de roles que se creó en Azure, almacene ahora las credenciales de ACR en un objeto de credenciales de Jenkins. A estas credenciales se hace referencia durante el trabajo de compilación de Jenkins.

De nuevo en el lado izquierdo del portal de Jenkins, haga clic en **Credentials** (Credenciales) > **Jenkins** > **Global credentials (unrestricted)** (Credenciales globales [sin restricción]) > **Add Credentials** (Agregar credenciales).

Asegúrese de que el tipo de credencial es **Username with password** (Nombre de usuario con contraseña) y especifique lo siguiente:

- **Username** (Nombre de usuario): *appId* de la entidad de servicio creada para la autenticación con el registro de Azure Container Registry.
- **Password** (Contraseña): *password* de la entidad de servicio creada para la autenticación con el registro de Azure Container Registry.
- **ID** (Id.): identificador de credencial como, por ejemplo, *acr-credentials*.

Cuando termine, el formulario de credenciales tendrá este aspecto:

![Crear un objeto de credencial de Jenkins con información de la entidad de servicio](media/jenkins-continuous-deployment/acr-credentials.png)

Haga clic en **Aceptar** y vuelva al portal de Jenkins.

## <a name="create-a-jenkins-project"></a>Crear un proyecto de Jenkins

En la página principal del portal de Jenkins, seleccione **Nuevo elemento** en el lado izquierdo:

1. Escriba *azure-vote* como nombre de trabajo. Seleccione **Freestyle project** (Proyecto de estilo libre) y elija **OK** (Aceptar).
1. En la sección **General**, seleccione el **proyecto de GitHub** y escriba la dirección URL del repositorio bifurcado; por ejemplo, *https:\//github.com/\<su-cuenta-de-github\>/azure-voting-app-redis*.
1. En la sección **Administración de código fuente**, seleccione **Git** y escriba la dirección URL del repositorio bifurcado *.git*; por ejemplo, *https:\//github.com/\<su-cuenta-de-github\>/azure-voting-app-redis.git*.

1. En la sección **Build Triggers** (Compilar desencadenadores), seleccione **GitHub hook trigger for GITScm polling** (Desencadenador de enlace de GitHub para sondeo de GITScm).
1. En **Build Environment** (Entorno de compilación), seleccione **Use secret texts or files** (Usar textos secretos o archivos).
1. En **Bindings** (Enlaces), seleccione **Add** (Agregar) > **Username and password (separated)** (Nombre de usuario y contraseña [separados]).
   - Escriba `ACR_ID` como **Username Variable** (Variable de nombre de usuario) y `ACR_PASSWORD` como **Password Variable** (Variable de contraseña).

     ![Enlaces de Jenkins](media/jenkins-continuous-deployment/bindings.png)

1. Elija agregar un **Build Step** (Paso de compilación) de tipo **Execute shell** (Ejecutar shell) y use este texto. Este script genera una nueva imagen del contenedor y la inserta en el registro de ACR.

    ```bash
    # Build new image and push to ACR.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME ./azure-vote
    docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
    docker push $WEB_IMAGE_NAME
    ```

1. Agregue otro **Build Step** (Paso de compilación) de tipo **Execute shell** (Ejecutar shell) y use el texto siguiente. Este script actualiza la implementación de aplicación en AKS con la nueva imagen de contenedor de ACR.

    ```bash
    # Update kubernetes deployment with new image.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

1. Cuando termine, haga clic en **Save** (Guardar).

## <a name="test-the-jenkins-build"></a>Prueba de la compilación de Jenkins

Antes de automatizar el trabajo en función de las confirmaciones de GitHub, primero pruebe manualmente la compilación de Jenkins. Esta compilación manual confirma que el trabajo se ha configurado correctamente, que se ha implementado el archivo de autenticación Kubernetes adecuado y que la autenticación con ACR funciona.

En el menú de la izquierda del proyecto, seleccione **Build Now** (Compilar ahora).

![Compilación de prueba de Jenkins](media/jenkins-continuous-deployment/test-build.png)

La primera compilación tarda un minuto o dos mientras se extraen las capas de imagen de Docker hacia el servidor de Jenkins. Las compilaciones posteriores pueden usar las capas de imagen almacenadas en caché para mejorar los tiempos de compilación.

Durante el proceso de compilación, se clona el repositorio de GitHub en el servidor de compilación de Jenkins. Se genera una nueva imagen del contenedor y se inserta en el registro de ACR. Por último, se actualiza la aplicación Azure Vote en ejecución en el clúster de AKS para usar la nueva imagen. Dado que no se ha efectuado ningún cambio en el código de la aplicación, la aplicación no se modifica si visualiza la aplicación de muestra en un explorador web.

Una vez completado el trabajo de compilación, haga clic en **compilación #1** en el historial de compilaciones. Seleccione **Salida de la consola** y vea la salida del proceso de compilación. La última línea debe indicar una compilación correcta.

## <a name="create-a-github-webhook"></a>Crear un webhook de GitHub

Con una correcta compilación manual completa, integre GitHub en la compilación de Jenkins. Se puede usar un webhook para ejecutar el trabajo de compilación de Jenkins cada vez que se realiza una confirmación de código en GitHub. Para crear el webhook de GitHub, siga estos pasos:

1. Vaya al repositorio de GitHub bifurcado en un explorador web.
1. Seleccione **Configuración** y **Webhooks** en el lado izquierdo.
1. Seleccione **Agregar Webhook**. Para *Dirección URL de carga*, escriba `http://<publicIp:8080>/github-webhook/`, donde `<publicIp>` corresponde a la dirección IP del servidor de Jenkins. Asegúrese de incluir la barra diagonal (/) final. Deje los demás valores predeterminados para el tipo de contenido y para el desencadenador en eventos de *inserción*.
1. Seleccione **Agregar Webhook**.

    ![Crear un webhook de GitHub para Jenkins](media/jenkins-continuous-deployment/webhook.png)

## <a name="test-the-complete-cicd-pipeline"></a>Probar la canalización de CI/CD completa

Ya puede probar la canalización de CI/CD completa. Cuando se inserte una confirmación de código en GitHub, se producen estos pasos:

1. El webhook de GitHub llega a Jenkins.
1. Jenkins inicia el trabajo de compilación y extrae la última confirmación de código de GitHub.
1. Se inicia una compilación de Docker con el código actualizado y se etiqueta la nueva imagen de contenedor con el número de compilación más reciente.
1. Esta nueva imagen de contenedor se inserta en Azure Container Registry.
1. Se implementa la aplicación en las actualizaciones de Azure Kubernetes Service con la imagen de contenedor más reciente procedente del registro de Azure Container Registry.

En la máquina de desarrollo, abra la aplicación clonada con un editor de código. En el directorio */azure-vote/azure-vote*, abra el archivo con el nombre **config_file.cfg**. Actualice los valores de votación en este archivo a algo distinto de gatos y perros, como se muestra en el ejemplo:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Cuando lo haya actualizado, guarde el archivo, confirme los cambios e insértelos en la bifurcación del repositorio de GitHub. El webhook de GitHub desencadena un nuevo trabajo de compilación en Jenkins. Supervise el proceso de compilación en el panel web de Jenkins. Tarda unos segundos en extraer el código más reciente, crear e insertar la imagen actualizada e implementar la aplicación actualizada en AKS.

Una vez completada la compilación, actualice el explorador web de la aplicación de votación de Azure de ejemplo. Los cambios que ha realizado se pueden apreciar, como en este ejemplo:

![Aplicación de votación de Azure de ejemplo en AKS actualizada por el trabajo de compilación de Jenkins](media/jenkins-continuous-deployment/azure-vote-updated.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Jenkins en Azure](/azure/jenkins)