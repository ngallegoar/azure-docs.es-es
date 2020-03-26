---
title: Implementación de aplicaciones en Azure Spring Cloud mediante Jenkins y la CLI de Azure
description: Aprenda a usar la CLI de Azure en una canalización de integración e implementación continuas para implementar microservicios en el servicio Azure Spring Cloud
ms.topic: tutorial
ms.date: 01/07/2020
ms.openlocfilehash: 67ad97bb762ed302ef52c404d47c5755ea4b245b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75732858"
---
# <a name="tutorial-deploy-apps-to-azure-spring-cloud-using-jenkins-and-the-azure-cli"></a>Tutorial: Implementación de aplicaciones en Azure Spring Cloud mediante Jenkins y la CLI de Azure

[Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-overview) es un microservicio de desarrollo totalmente administrado con administración de configuración y detección de servicios integrada. Facilita la implementación de aplicaciones de microservicio basadas en Spring Boot en Azure. En este tutorial se muestra cómo usar la CLI de Azure en Jenkins para automatizar la integración y entrega continuas (CI/CD) para Azure Spring Cloud.

En este tutorial va a completar estas tareas:

> [!div class="checklist"]
> * Aprovisionamiento de una instancia de servicio e inicio de una aplicación Java Spring
> * Preparación del servidor Jenkins
> * Uso de la CLI de Azure en una canalización de Jenkins para compilar e implementar las aplicaciones de microservicios 

En este tutorial se supone que tiene un conocimiento intermedio de los servicios básicos de Azure, Azure Spring Cloud, las [canalizaciones](https://jenkins.io/doc/book/pipeline/) y los complementos de Jenkins y GitHub.

## <a name="prerequisites"></a>Prerrequisitos

>[!Note]
> Azure Spring Cloud se ofrece actualmente como versión preliminar pública. Las ofertas de versión preliminar pública permiten a los clientes experimentar con nuevas características antes de su publicación oficial.  Los servicios y las características en versión preliminar pública no están diseñados para su uso en producción.  Para más información sobre el soporte técnico durante las versiones preliminares, revise las [preguntas frecuentes](https://azure.microsoft.com/support/faq/) o envíe una [solicitud de soporte técnico](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) para obtener más información.

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]

* Una cuenta de GitHub. Antes de comenzar, si no tiene una cuenta de GitHub, cree una [gratuita](https://github.com/).

* Un servidor maestro de Jenkins. Si aún no tiene un servidor maestro de Jenkins, implemente [Jenkins](https://aka.ms/jenkins-on-azure) en Azure mediante los pasos de este  [inicio rápido](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). En el nodo/agente de Jenkins se requiere lo siguiente (por ejemplo, servidor de compilación):

    * [Git](https://git-scm.com/)
    * [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3.0 o cualquier versión superior](https://maven.apache.org/download.cgi)
    * La [CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest), versión 2.0.67 o posterior

    >[!TIP]
    > Las herramientas como Git, JDK, Az CLI y los complementos de Azure se incluyen de forma predeterminada en la plantilla de la solución de [Microsoft Jenkins](https://aka.ms/jenkins-on-azure) en Azure Marketplace.
    
* [Registro para obtener una suscripción a Azure](https://azure.microsoft.com/free/)
 
## <a name="provision-a-service-instance-and-launch-a-java-spring-application"></a>Aprovisionamiento de una instancia de servicio e inicio de una aplicación Java Spring

Usamos [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics) como aplicación de servicio de Microsoft de ejemplo y seguimos los mismos pasos de [Inicio rápido: Inicio de una aplicación Java Spring mediante la CLI de Azure](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli) para aprovisionar la instancia de servicio y configurar las aplicaciones. Si ya ha pasado por este proceso, puede ir directamente a la sección siguiente. De lo contrario, a continuación se incluyen los comandos de la CLI de Azure. Consulte [Inicio rápido: Inicio de una aplicación Java Spring mediante la CLI de Azure](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli) para obtener información general adicional.

La máquina local debe cumplir los mismos requisitos previos que el servidor de compilación de Jenkins. Asegúrese de que se instala lo siguiente para compilar e implementar las aplicaciones de microservicios:
    * [Git](https://git-scm.com/)
    * [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3.0 o cualquier versión superior](https://maven.apache.org/download.cgi)
    * La [CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest), versión 2.0.67 o posterior

1. Instale la extensión de Azure Spring Cloud:

    ```Azure CLI
        az extension add --name spring-cloud
    ```

2. Cree un grupo de recursos que contenga el servicio Azure Spring Cloud:

    ```Azure CLI
        az group create --location eastus --name <resource group name>
    ```

3. Aprovisione una instancia de Azure Spring Cloud:

    ```Azure CLI
        az spring-cloud create -n <service name> -g <resource group name>
    ```

4. Bifurque el repositorio de [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics) a su cuenta de GitHub. En la máquina local, clone el repositorio en un directorio llamado `source-code`:

    ```bash
        mkdir source-code
        git clone https://github.com/<your GitHub id>/piggymetrics
    ```

5. Configure el servidor de configuración. Asegúrese de reemplazar &lt;el identificador de GitHub&gt; por el valor correcto.

    ```Azure CLI
        az spring-cloud config-server git set -n <your-service-name> --uri https://github.com/<your GitHub id>/piggymetrics --label config
    ```

6. Compile el proyecto:

    ```bash
        cd piggymetrics
        mvn clean package -D skipTests
    ```

7. Cree los tres microservicios: **gateway**, **auth-service** y **account-service**:

    ```Azure CLI
        az spring-cloud app create --n gateway -s <service name> -g <resource group name>
        az spring-cloud app create --n auth-service -s <service name> -g <resource group name>
        az spring-cloud app create --n account-service -s <service name> -g <resource group name>
    ```

8. Implemente las aplicaciones: 

    ```Azure CLI
        az spring-cloud app deploy -n gateway -s <service name> -g <resource group name> --jar-path ./gateway/target/gateway.jar
        az spring-cloud app deploy -n account-service -s <service name> -g <resource group name> --jar-path ./account-service/target/account-service.jar
        az spring-cloud app deploy -n auth-service -s <service name> -g <resource group name> --jar-path ./auth-service/target/auth-service.jar
    ```

9. Asigne un punto de conexión público a gateway:

    ```Azure CLI
        az spring-cloud app update -n gateway -s <service name> -g <resource group name> --is-public true
    ```

10. Consulte la aplicación gateway para obtener la URL y así poder comprobar que la aplicación se está ejecutando.

    ```Azure CLI
    az spring-cloud app show --name gateway | grep url
    ```
    
    Vaya hasta la dirección URL proporcionada por el comando anterior para ejecutar la aplicación PiggyMetrics. 

## <a name="prepare-jenkins-server"></a>Preparación del servidor Jenkins

En esta sección se prepara el servidor de Jenkins para ejecutar una compilación, adecuada para realizar pruebas. Sin embargo, por motivos de seguridad, debe usar un [agente de máquina virtual de Azure](https://plugins.jenkins.io/azure-vm-agents) o un [agente de Azure Container](https://plugins.jenkins.io/azure-container-agents) para establecer un agente de Azure para ejecutar las compilaciones. Para más información, consulte el artículo de Jenkins sobre las [implicaciones de seguridad de compilar en el servidor maestro](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master).

### <a name="install-plug-ins"></a>Instalación de los complementos

1. Inicie sesión en el servidor Jenkins. Seleccione **Manage Jenkins > Manage Plugins** (Administrar Jenkins > Administrar complementos).
2. En la pestaña **Available** (Disponible), seleccione los siguientes complementos:
    * [GitHub Integration](https://plugins.jenkins.io/github-pullrequest)
    * [Azure Credentials](https://plugins.jenkins.io/azure-credentials)

    Si estos complementos no aparecen en la lista, compruebe la pestaña **Installed** (Instalado) para ver si ya están instalados.

3. Para instalar los complementos, elija **Download now and install after restart** (Descargar ahora e instalar después de reiniciar).

4. Reinicie el servidor Jenkins para completar la instalación.

### <a name="add-your-azure-service-principal-credential-in-jenkins-credential-store"></a>Incorporación de la credencial de la entidad de servicio de Azure en el almacén de credenciales de Jenkins

1. Se necesita una entidad de servicio de Azure para la implementación en Azure. Para más información, consulte la sección  [Creación de una entidad de servicio](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) del tutorial Implementación en Azure App Service. La salida de `az ad sp create-for-rbac` es parecida a esta:

    ```
    {
        "appId": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "displayName": "xxxxxxxjenkinssp",
        "name": "http://xxxxxxxjenkinssp",
        "password": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "tenant": "xxxxxx--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. En el panel de Jenkins, seleccione **Credentials** (Credenciales) > **System**(Sistema). A continuación, haga clic en **Global credentials(unrestricted)** (Credenciales globales [sin restricción]).

3. Seleccione **Add Credentials** (Agregar credenciales). 

4. Seleccione el tipo **Microsoft Azure Service Principal** (Entidad de servicio de Microsoft Azure).

5. Proporcione valores para: * Subscription ID (Identificador de la suscripción): use su identificador de suscripción de Azure * Client ID (Identificador de cliente): use `appId` * Client Secret (Secreto de cliente): use `password` * Tenant ID (Identificador de inquilino): use `tenant` * Azure Environment (Entorno de Azure): seleccione un valor preestablecido. Por ejemplo, use **Azure** para Azure Global * ID (Identificador): establecido como **azure_service_principal**. Usamos este identificador en un paso posterior de este artículo * Description (Descripción): campo opcional. Es recomendable proporcionar aquí un valor significativo.

### <a name="install-maven-and-az-cli-spring-cloud-extension"></a>Instalación de las extensiones Maven y Az CLI Spring-Cloud

La canalización de ejemplo usa Maven para la compilación y Az CLI para la implementación en la instancia de servicio. Cuando se instala Jenkins, se crea una cuenta de administrador denominada *jenkins*. Asegúrese de que el usuario *jenkins* tiene permiso para ejecutar la extensión Spring-Cloud.

1. Conéctese a Jenkins Master mediante SSH. 

2. Instale Maven.

    ```bash
        sudo apt-get install maven 
    ```

3. Instale la CLI de Azure. Para más información, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). La CLI de Azure se instala de manera predeterminada si usa [Jenkins Master en Azure](https://aka.ms/jenkins-on-azure).

4. Cambie al usuario `jenkins`:

    ```bash
        sudo su jenkins
    ```

5. Agregue la extensión **Spring-Cloud**:

    ```bash
        az extension add --name spring-cloud
    ```

## <a name="create-a-jenkinsfile"></a>Creación de un archivo Jenkinsfile
1. En su propio repositorio (https://github.com/&lt ; su identificador de GitHub&gt; /piggymetrics), cree un archivo **Jenkinsfile** en la raíz.

2. Actualice el archivo de la forma siguiente. Asegúrese de reemplazar los valores de **\<nombre del grupo de recursos>** y **\<nombre del servicio>** . Reemplace **entidad_de_servicio_de_azure** por el identificador correcto si usa un valor diferente al agregar la credencial en Jenkins. 

```groovy
    node {
      stage('init') {
        checkout scm
      }
      stage('build') {
        sh 'mvn clean package'
      }
      stage('deploy') {
        withCredentials([azureServicePrincipal('azure_service_principal')]) {
          // login to Azure
          sh '''
            az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
            az account set -s $AZURE_SUBSCRIPTION_ID
          '''  
          // Set default resource group name and service name. Replace <resource group name> and <service name> with the right values
          sh 'az configure --defaults group=<resource group name>'
          sh 'az configure --defaults spring-cloud=<service name>'
          // Deploy applications
          sh 'az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar'
          sh 'az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar'
          sh 'az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar'
          sh 'az logout'
        }
      }
    }
```

3. Confirme el cambio y guárdelo.

## <a name="create-the-job"></a>Creación del trabajo

1. En el panel de Jenkins, haga clic en **New Item** (Nuevo elemento).

2. Proporcione un nombre, *Deploy-PiggyMetrics*, para el trabajo y seleccione **Pipeline** (Canalización). Haga clic en Aceptar.

3. Haga clic en la pestaña **Pipeline** (Canalización) a continuación.

4. En **Definition** (Definición), seleccione **Pipeline script from SCM** (Script de canalización del SCM).

5. En **SCM**, seleccione **Git**.

6. Escriba la dirección URL de GitHub para el repositorio bifurcado: **https://github.com/&lt ; su identificador de GitHub&gt; /piggymetrics.git**.

7. Asegúrese de que **Branch Specifier (blank for 'any')** (Especificador de rama [en blanco para "cualquiera"]) es * **/Azure**.

8. Mantenga **Script path** (Ruta de acceso del script) como **Jenkinsfile**.

7. Haga clic en **Guardar**

## <a name="validate-and-run-the-job"></a>Validación y ejecución del trabajo

Antes de ejecutar el trabajo, vamos a actualizar el texto del cuadro de entrada de inicio de sesión a **Escriba el id. de inicio de sesión**.

1. En su propio repositorio, abra `index.html` en **/gateway/src/main/resources/static/** .

2. Busque "enter your login" y actualice el texto a "Escriba el id. de inicio de sesión".

    ```HTML
        <input class="frontforms" id="frontloginform" name="username" placeholder="enter login ID" type="text" autocomplete="off"/>
    ```

3. Confirmación de los cambios

4. Ejecute el trabajo en Jenkins manualmente. En el panel de Jenkins, haga clic en el trabajo *Deploy-PiggyMetrics* y en **Build Now** (Compilar ahora).

Una vez completado el trabajo, vaya a la dirección IP pública de la aplicación **gateway** y compruebe que la aplicación se ha actualizado. 

![PiggyMetrics actualizado](media/tutorial-jenkins-deploy-cli-spring-cloud/piggymetrics.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine los recursos creados en este artículo:

```bash
az group delete -y --no-wait -n <resource group name>
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a usar la CLI de Azure en Jenkins para automatizar la integración y entrega continuas (CI/CD) para Azure Spring Cloud.

Para más información sobre el proveedor de Jenkins para Azure, consulte el sitio de Jenkins en Azure.

> [!div class="nextstepaction"]
> [Jenkins en Azure](/azure/jenkins/)
