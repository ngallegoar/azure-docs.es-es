---
title: Uso del complemento de Azure Dev Spaces para Jenkins con Azure Kubernetes Service
description: Aprenda a usar el complemento de Azure Dev Spaces en una canalización de integración continua.
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: 281565cec5ee947781ab8ee9f62a00e01f9ababb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79037038"
---
# <a name="tutorial-using-the-azure-dev-spaces-plug-in-for-jenkins-with-azure-kubernetes-service"></a>Tutorial: Uso del complemento de Azure Dev Spaces para Jenkins con Azure Kubernetes Service 

Azure Dev Spaces le permite probar y desarrollar iterativamente su aplicación de microservicios ejecutándola en Azure Kubernetes Service (AKS) sin necesidad de replicar o simular dependencias. El complemento de Azure Dev Spaces para Jenkins le permitirá usar Dev Spaces en la canalización de integración y entrega continuas (CI/CD).

En este tutorial, también se utiliza Azure Container Registry (ACR). ACR almacena imágenes y una instancia de ACR Tasks compila artefactos de Docker y Helm. El uso de ACR y ACR Tasks para la generación de artefactos elimina la necesidad de instalar software adicional, como Docker, en el servidor Jenkins. 

En este tutorial va a completar estas tareas:

> [!div class="checklist"]
> * Creación de un clúster de AKS con Azure Dev Spaces habilitado
> * Implementación de una aplicación multiservicio en AKS
> * Preparación del servidor Jenkins
> * Uso del complemento de Azure Dev Spaces en una canalización de Jenkins para previsualizar cambios de código antes de fusionarlos en el proyecto

En este tutorial se supone que tiene un conocimiento intermedio de los servicios básicos de Azure, AKS, ACR, Azure Dev Spaces, las [canalizaciones](https://jenkins.io/doc/book/pipeline/) y los complementos de Jenkins y GitHub. También resulta útil tener un conocimiento básico de herramientas de apoyo, como kubectl y Helm.

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Una cuenta de GitHub. Antes de comenzar, si no tiene una cuenta de GitHub, cree una [gratuita](https://github.com/).

* [Visual Studio Code](https://code.visualstudio.com/download) con la extensión [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) instalada.

* [La CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest), versión 2.0.43 o posterior.

* Un servidor maestro de Jenkins. Si aún no tiene un servidor maestro de Jenkins, implemente [Jenkins](https://aka.ms/jenkins-on-azure) en Azure mediante los pasos de este  [inicio rápido](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* El servidor Jenkins debe tener tanto Helm como kubectl instalados y disponibles para la cuenta de Jenkins, como se explica más adelante en este tutorial.

* VS Code, el terminal de VS Code o WSL, y Bash. 


## <a name="create-azure-resources"></a>Creación de recursos de Azure

En esta sección, creará recursos de Azure:

* Un grupo de recursos que contendrá todos los recursos de Azure para este tutorial.
* Un clúster de [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) (AKS).
* Una instancia de [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) (ACR) para compilar (con ACR Tasks) y almacenar imágenes de Docker.

1. Cree un grupo de recursos.

    ```azurecli
    az group create --name MyResourceGroup --location westus2
    ```

2. Cree un clúster de AKS. Cree el clúster de AKS en una [región que admita Dev Spaces](../dev-spaces/about.md#supported-regions-and-configurations).

    ```azurecli
    az aks create --resource-group MyResourceGroup --name MyAKS --location westus2 --kubernetes-version 1.11.9 --enable-addons http_application_routing --generate-ssh-keys --node-count 1 --node-vm-size Standard_D1_v2
    ```

3. Configure AKS para usar Dev Spaces.

    ```azurecli
    az aks use-dev-spaces --resource-group MyResourceGroup --name MyAKS
    ```
    Este paso instala la extensión de la CLI `azds`.

4. Cree un registro de contenedor.

    ```azurecli
    az acr create -n MyACR -g MyResourceGroup --sku Basic --admin-enabled true
    ```

## <a name="deploy-sample-apps-to-the-aks-cluster"></a>Implementación de aplicaciones de ejemplo en el clúster de AKS

En esta sección, configurará un espacio de desarrollo e implementará una aplicación de ejemplo para el clúster AKS que ha creado en la sección anterior. La aplicación consta de dos partes: *webfrontend* y *mywebapi*. Ambos componentes se implementan en un espacio de desarrollo. Más adelante en este tutorial, enviará una solicitud de incorporación de cambios en mywebapi para desencadenar la canalización de integración continua en Jenkins.

Para más información sobre el uso de Azure Dev Spaces y el desarrollo de varios servicios con Azure Dev Spaces, consulte [Introducción a Azure Dev Spaces con Java](https://docs.microsoft.com/azure/dev-spaces/get-started-java) y [Desarrollo de varios servicios con Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/multi-service-java). Estos tutoriales proporcionan información de contexto adicional que no se incluye aquí.

1. Descargue el repositorio https://github.com/Azure/dev-spaces desde GitHub.

2. Abra la carpeta `samples/java/getting-started/webfrontend` en VS Code. (Puede omitir cualquier mensaje predeterminado para agregar recursos de depuración o restaurar el proyecto).

3. Actualice `/src/main/java/com/ms/sample/webfrontend/Application.java` para que tenga este aspecto:

    ```java
    package com.ms.sample.webfrontend;

    import java.io.*;
    import java.net.*;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.web.bind.annotation.*;

    @SpringBootApplication
    @RestController
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }

        @RequestMapping(value = "/greeting", produces = "text/plain")
        public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
            URLConnection conn = new URL("http://mywebapi/").openConnection();
            conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
            try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
            {
                return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
            }
        }
    }
    ```

4. Haga clic en **Ver** > **Terminal** para abrir el terminal integrado en VS Code.

5. Ejecute el comando `azds prep` para preparar la aplicación para que se ejecute en un espacio de desarrollo. Este comando se debe ejecutar desde `dev-spaces/samples/java/getting-started/webfrontend` para preparar correctamente la aplicación:

    ```bash
    azds prep --public
    ```

    El comando `azds prep` de la CLI de Dev Spaces genera recursos de Docker y Kubernetes con la configuración predeterminada. Estos archivos se conservan durante todo el proyecto y se pueden personalizar:

    * `./Dockerfile` y `./Dockerfile.develop` describen la imagen de contenedor de la aplicación y cómo el código fuente se compila y se ejecuta dentro del contenedor.
    * A [gráfico de Helm](https://helm.sh/docs/topics/charts/) bajo `./charts/webfrontend` describe cómo implementar el contenedor en Kubernetes.
    * `./azds.yaml` es el archivo de configuración de Azure Dev Spaces.

    Para más información, consulte [Funcionamiento y configuración de Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works).

6. Compile y ejecute la aplicación en AKS mediante el comando `azds up`:

    ```bash
    azds up
    ```
    <a name="test_endpoint"></a>Examine la salida de la consola para obtener información acerca de la dirección URL que creó el comando `up`. Tendrá el formato siguiente:

    ```bash
    (pending registration) Service 'webfrontend' port 'http' will be available at '<url>'
    Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
    ```
    Abra esta dirección URL en una ventana del explorador y verá la aplicación web. Mientras se ejecuta el contenedor, las salidas `stdout` y `stderr` se transmiten a la ventana de terminal.

8. A continuación, configure e implemente *mywebapi*:

    1. Cambie el directorio a `dev-spaces/samples/java/getting-started/mywebapi`.

    2. Ejecute

        ```bash
        azds prep
        ```

    3. Ejecute

        ```bash
        azds up -d
        ```

## <a name="prepare-jenkins-server"></a>Preparación del servidor Jenkins

En esta sección, preparará el servidor Jenkins para ejecutar la canalización de integración continua de ejemplo.

* Instalación de los complementos
* Instalación de la CLI de Kubernetes y Helm
* Adición de credenciales

### <a name="install-plug-ins"></a>Instalación de los complementos

1. Inicie sesión en el servidor Jenkins. Seleccione **Manage Jenkins > Manage Plugins** (Administrar Jenkins > Administrar complementos).
2. En la pestaña **Available** (Disponible), seleccione los siguientes complementos:
    * [Azure Dev Spaces](https://plugins.jenkins.io/azure-dev-spaces)
    * [Azure Container Registry Tasks](https://plugins.jenkins.io/azure-container-registry-tasks)
    * [Environment Injector](https://plugins.jenkins.io/envinject)
    * [GitHub Integration](https://plugins.jenkins.io/github-pullrequest)

    Si estos complementos no aparecen en la lista, compruebe la pestaña **Installed** (Instalado) para ver si ya están instalados.

3. Para instalar los complementos, elija **Download now and install after restart** (Descargar ahora e instalar después de reiniciar).

4. Reinicie el servidor Jenkins para completar la instalación.

### <a name="install-helm-and-kubectl"></a>Instalación de Helm y kubectl

La canalización de ejemplo usa kubectl y Helm para la implementación en el espacio de desarrollo. Cuando se instala Jenkins, se crea una cuenta de administrador denominada *jenkins*. El usuario jenkins debe poder acceder tanto a Helm como a kubectl.

1. Realice una conexión SSH al servidor maestro de Jenkins. 

2. Cambie al usuario `jenkins`:
    ```bash
    sudo su jenkins
    ```

3. Instale la CLI de Helm. Para más información, consulte [Instalación de Helm](https://helm.sh/docs/using_helm/#installing-helm).

4. Instale kubectl. Para más información, consulte [**az acs kubernetes install-cli**](https://helm.sh/docs/using_helm/#installing-helm).

### <a name="add-credentials-to-jenkins"></a>Adición de credenciales a Jenkins

1. Jenkins necesita una entidad de servicio de Azure para la autenticación y el acceso a los recursos de Azure. Para crear la entidad de servicio, consulte la sección  [Creación de una entidad de servicio](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) del tutorial Implementación en Azure App Service. Asegúrese de guardar una copia de la salida de `create-for-rbac`, ya que esa información es necesaria para completar el paso siguiente. El resultado tendrá un aspecto similar al siguiente:

    ```json
    {
      "appId": "f4150da1-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "displayName": "xxxxxxxjenkinssp",
      "name": "http://xxxxxxxjenkinssp",
      "password": "f6e4d839-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "tenant": "72f988bf--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. Con la información de la entidad de servicio del paso anterior, agregue el tipo de credencial *Microsoft Azure Service Principal* (Entidad de servicio de Microsoft Azure) en Jenkins. Los nombres de la captura de pantalla siguiente corresponden a la salida de `create-for-rbac`.

    El campo **ID** es el nombre de credencial de Jenkins para la entidad de servicio. En el ejemplo se usa el valor de `displayName` (en este caso, `xxxxxxxjenkinssp`), pero puede usar cualquier texto que desee. Este nombre de credencial es el valor de la variable de entorno AZURE_CRED_ID en la sección siguiente.

    ![Adición de credenciales de entidad de servicio a Jenkins](media/tutorial-jenkins-dev-spaces/add-service-principal-credentials.png)

    El campo **Description** (Descripción) es opcional. Para obtener instrucciones detalladas, consulte la sección [Incorporación de la entidad de servicio a Jenkins](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#add-service-principal-to-jenkins) del tutorial Implementación en Azure App Service. 



3. Para mostrar sus credenciales de ACR, ejecute este comando:

    ```azurecli
    az acr credential show -n <yourRegistryName>
    ```

    Realice una copia de la salida de JSON, que debe tener un aspecto similar a este:

    ```json
    {
      "passwords": [
        {
          "name": "password",
          "value": "vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz"
        },
        {
          "name": "password2",
          "value": "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
        }
      ],
      "username": "acr01"
    }
    ```

4. Agregue un tipo de credencial *Username with password* (Nombre de usuario con contraseña) en Jenkins. **username** (nombre de usuario) es el nombre de usuario del último paso, en este ejemplo `acr01`. **password** (contraseña) es el valor de la primera contraseña, en este ejemplo `vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz`. El **ID** de esta credencial es el valor de ACR_CRED_ID.

5. Configure una credencial de AKS. Agregue un tipo de credencial *Kubernetes configuration (kubeconfig)* [Configuración de Kubernetes (kubeconfig)] en Jenkins (use la opción "Enter directly" [Escribir directamente]). Para obtener las credenciales de acceso para su clúster de AKS, ejecute el siguiente comando:

    ```azurecli
    az aks get-credentials -g MyResourceGroup -n <yourAKSName> -f -
    ```

   El **ID** de esta credencial es el valor de KUBE_CONFIG_ID de la sección siguiente.

## <a name="create-a-pipeline"></a>Crear una canalización

El escenario seleccionado para la canalización de ejemplo se basa en un patrón real: Una solicitud de incorporación de cambios desencadena una canalización de integración continua que compila e implementa los cambios propuestos en una instancia de Azure Dev Spaces para revisión y pruebas. Dependiendo del resultado de la revisión, los cambios se combinan e implementan en AKS o se descartan. Por último, se elimina el espacio de desarrollo.

La configuración de la canalización de Jenkins y Jenkinsfile definen las fases de la canalización de integración continua. Este diagrama de flujo muestra las fases de la canalización y los puntos de decisión definidos por Jenkinsfile:

![Flujo de canalización de Jenkins](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-flow.png)

1. Descargue una versión modificada del proyecto *mywebapi* de [https://github.com/azure-devops/mywebapi](https://github.com/azure-devops/mywebapi). Este proyecto contiene varios archivos necesarios para crear una canalización, como *Jenkinsfile*, *Dockerfiles* y el gráfico de Helm.

2. Inicie sesión en Jenkins. En el menú de la izquierda, seleccione **Add Item** (Agregar elemento).

3. Seleccione **Pipeline** (Canalización) y, a continuación, escriba un nombre en el cuadro **Enter an item name** (Escribir un nombre para el elemento). Seleccione **Aceptar** y, a continuación, se abrirá automáticamente la pantalla de configuración de la canalización.

4. En la pestaña **General**, seleccione **Prepare an environment for the run** (Preparar un entorno para la ejecución). 

5. Seleccione **Keep Jenkins Environment Variables** (Mantener variables de entorno de Jenkins) y **Keep Jenkins Build Variables** (Mantener variables de compilación de Jenkins).

6. En el cuadro **Properties Content** (Contenido de las propiedades), escriba las siguientes variables de entorno:

    ```
    AZURE_CRED_ID=[your credential ID of service principal]
    RES_GROUP=[your resource group of the function app]
    ACR_RES_GROUP=[your ACR resource group]
    ACR_NAME=[your ACR name]
    ACR_REGISTRY=[your ACR registry url, without http schema]
    ACR_CRED_ID=[your credential id of your ACR account]
    AKS_RES_GROUP=[your AKS resource group]
    AKS_NAME=[your AKS name]
    IMAGE_NAME=[name of Docker image you will push to ACR, without registry prefix]
    KUBE_CONFIG_ID=[your kubeconfig id]
    PARENT_DEV_SPACE=[shared dev space name]
    TEST_ENDPOINT=[your web frontend end point for testing. Should be webfrontend.XXXXXXXXXXXXXXXXXXX.xxxxxx.aksapp.io]
    ```

    Con los valores de ejemplo proporcionados en las secciones anteriores, la lista de variables de entorno debe tener un aspecto similar al siguiente:

    ![Variables de entorno de canalización de Jenkins](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-environment.png)

7. Seleccione **Pipeline script from SCM** (Script de canalización del SCM) en **Pipeline > Definition** (Canalización > Definición).
8. En **SCM**, elija **Git** y, a continuación, escriba la dirección URL del repositorio.
9. En **Branch Specifier** (Especificador de rama), escriba `refs/remotes/origin/${GITHUB_PR_SOURCE_BRANCH}`.
10. Rellene la dirección URL del repositorio de SCM y la ruta del script "Jenkinsfile".
11. La opción **Lightweight checkout** (Restauración ligera) debe estar seleccionada.

## <a name="create-a-pull-request-to-trigger-the-pipeline"></a>Creación de una solicitud de incorporación de cambios para desencadenar la canalización

Para completar el paso 3 de esta sección, deberá comentar parte de Jenkinsfile. En caso contrario, obtendrá un error 404 al intentar ver las versiones nuevas y antiguas en paralelo. De forma predeterminada, cuando decide combinar la solicitud de incorporación de cambios, se quita la anterior versión compartida de mywebapi y se reemplaza por la nueva versión. Realice el siguiente cambio en Jenkinsfile antes de completar el paso 1:

```Groovy
    if (userInput == true) {
        stage('deploy') {
            // Apply the deployment to shared namespace in AKS using acsDeploy, Helm or kubectl   
        }
        
        stage('Verify') {
            // verify the staging environment is working properly
        }
        
        /* Comment the cleanup stage to allow side by side comparison with the new child dev space

        stage('cleanup') {
            devSpacesCleanup aksName: env.AKS_NAME, 
                azureCredentialsId: env.AZURE_CRED_ID, 
                devSpaceName: devSpaceNamespace, 
                kubeConfigId: env.KUBE_CONFIG_ID, 
                resourceGroupName: env.AKS_RES_GROUP,
                helmReleaseName: releaseName
        }
        */

    } else {
        // Send a notification
    }
```

1. Realiza un cambio en `mywebapi/src/main/java/com/ms/sample/mywebapi/Application.java` y, a continuación, cree una solicitud de incorporación de cambios. Por ejemplo:

    ```java
    public String index() {
        return "Hello from mywebapi in my private dev space";
    }
    ```

2. Inicie sesión en Jenkins, seleccione el nombre de la canalización y, a continuación, elija **Build Now** (Compilar ahora). 

    También puede configurar un *webhook* para desencadenar automáticamente la canalización de Jenkins. Al especificar una solicitud de incorporación de cambios, GitHub emite una solicitud POST dirigida a Jenkins, desencadenando la canalización. Para obtener más información acerca de cómo configurar un webhook, consulte [Conexión de Jenkins a GitHub](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service.md#connect-jenkins-to-github).

3. Compare los cambios con la versión compartida actual:

    1. Abra el explorador y vaya a la versión compartida `https://webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. TEST_ENDPOINT contiene la dirección URL.

    2. Abra otra pestaña y, a continuación, escriba la dirección URL del espacio de desarrollo de la solicitud de incorporación de cambios. Será algo similar a `https://<yourdevspacename>.s.webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. Encontrará el vínculo en **Build History > <build#> > Console Output** (Historial de compilación > <n.º de compilación> > Salida de consola) para el trabajo de Jenkins. Busque `aksapp` en la página o, para ver solo el prefijo, busque `azdsprefix`.

 

### <a name="constructing-the-url-to-the-child-dev-space"></a>Creación de la dirección URL al espacio de desarrollo secundario

Al realizar una solicitud de incorporación de cambios, Jenkins crea un espacio de desarrollo secundario basado en el espacio de desarrollo compartido del equipo y ejecuta el código de la solicitud de incorporación de cambios en ese espacio de desarrollo secundario. La dirección URL al espacio de desarrollo secundario adopta la forma `http://$env.azdsprefix.<test_endpoint>`. 

**$env.azdsprefix** se establece durante la ejecución de la canalización por parte del complemento de Azure Dev Spaces mediante **devSpacesCreate**:

```Groovy
stage('create dev space') {
    devSpacesCreate aksName: env.AKS_NAME,
        azureCredentialsId: env.AZURE_CRED_ID,
        kubeconfigId: env.KUBE_CONFIG_ID,
        resourceGroupName: env.AKS_RES_GROUP,
        sharedSpaceName: env.PARENT_DEV_SPACE,
        spaceName: devSpaceNamespace
}
```

`test_endpoint` es la dirección URL a la aplicación webfrontend que implementó anteriormente mediante `azds up` en [Implementación de aplicaciones de ejemplo en el clúster de AKS, paso 7](#test_endpoint). El valor de `$env.TEST_ENDPOINT` se establece en la configuración de la canalización. 

El siguiente fragmento de código muestra cómo se usa la dirección URL del espacio de desarrollo secundario en la fase `smoketest`. El código comprueba si el espacio de desarrollo secundario TEST_ENDPOINT está disponible y, si es así, descarga el texto de saludo en stdout:

```Groovy
stage('smoketest') {
    // CI testing against http://$env.azdsprefix.$env.TEST_ENDPOINT" 
    SLEEP_TIME = 30
    SITE_UP = false
    for (int i = 0; i < 10; i++) {
        sh "sleep ${SLEEP_TIME}"
        code = "0"
        try {
            code = sh returnStdout: true, script: "curl -sL -w '%{http_code}' 'http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting' -o /dev/null"
        } catch (Exception e){
            // ignore
        }
        if (code == "200") {
            sh "curl http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting"
            SITE_UP = true
            break
        }
    }
    if(!SITE_UP) {
        echo "The site has not been up after five minutes"
    }
}
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar la aplicación de ejemplo, limpie los recursos de Azure mediante la eliminación del grupo de recursos:

```azurecli
az group delete -y --no-wait -n MyResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [CI/CD con Jenkins en Azure](jenkins-continuous-deployment.md)
