---
title: Implementación en Azure Kubernetes Service con Jenkins y el patrón de implementación blue/green
description: Aprenda a implementar en Azure Kubernetes Service (AKS) con Jenkins y el patrón de implementación azul/verde.
keywords: jenkins, azure, devops, kubernetes, k8s, aks, blue green deployment, entrega continua, cd
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: 9d6551f910bd99322f844b44130ebb03732df83c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78251472"
---
# <a name="deploy-to-azure-kubernetes-service-aks-by-using-jenkins-and-the-bluegreen-deployment-pattern"></a>Implementación en Azure Kubernetes Service (AKS) con Jenkins y el patrón de implementación azul/verde

Azure Kubernetes Service (AKS) administra el entorno hospedado de Kubernetes, lo que hace que sea fácil y rápido implementar y administrar aplicaciones en contenedores. No es necesario tener experiencia en la orquestación de contenedores. AKS también elimina la carga de las operaciones y el mantenimiento continuos gracias al aprovisionamiento, la actualización y el escalado de recursos a petición. No es necesario desconectar las aplicaciones. Para más información sobre AKS, consulte la [documentación de AKS](/azure/aks/).

La implementación azul/verde es un patrón de entrega continua de Azure DevOps que se basa en mantener una versión existente (azul) activa mientras se implementa una nueva (verde). Normalmente, este patrón emplea el equilibrio de carga para aumentar directamente las cantidades de tráfico para la implementación verde. Si la supervisión detecta un incidente, se puede redirigir el tráfico a la implementación azul, que sigue en ejecución. Para más información sobre la entrega continua, consulte [What is Continuous Delivery](/azure/devops/what-is-continuous-delivery) (Qué es la entrega continua).

En este tutorial, aprenderá a realizar las siguientes tareas:

> [!div class="checklist"]
> * Obtener información sobre el patrón de implementación azul/verde
> * Crear un clúster de Kubernetes administrado
> * Ejecutar un script de ejemplo para configurar un clúster de Kubernetes
> * Configurar manualmente un clúster de Kubernetes
> * Crear y ejecutar un trabajo de Jenkins

## <a name="prerequisites"></a>Prerequisites
- [Cuenta de GitHub](https://github.com): se necesita una cuenta de GitHub para clonar el repositorio de ejemplo.
- [CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest): usará la CLI de Azure 2.0 para crear el clúster de Kubernetes.
- [Chocolatey](https://chocolatey.org): un administrador de paquetes que se usa para instalar kubectl.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): una interfaz de la línea de comandos que se usa para ejecutar comandos en clústeres de Kubernetes.
- [jq](https://stedolan.github.io/jq/download/): un procesador JSON ligero de línea de comandos.

## <a name="clone-the-sample-app-from-github"></a>Clonación de la aplicación de ejemplo desde GitHub

En el repositorio de Microsoft en GitHub, puede encontrar una aplicación de ejemplo en la que se ilustra cómo implementar en AKS con Jenkins y el patrón azul/verde. En esta sección, se crea una bifurcación del repositorio en su GitHub y se clona la aplicación en el sistema local.

1. Vaya al repositorio de GitHub para la aplicación de ejemplo [todo-app-java-on-azure](https://github.com/microsoft/todo-app-java-on-azure.git).

    ![Captura de pantalla de aplicación de ejemplo en el repositorio de GitHub de Microsoft](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Bifurque el repositorio seleccionando **Bifurcación** en la esquina superior derecha de la página y siga las instrucciones para bifurcar el repositorio en su cuenta de GitHub.

    ![Captura de pantalla de la opción de GitHub para bifurcar](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Una vez que bifurque el repositorio, verá que el nombre de la cuenta cambia al de su cuenta, y una nota indica desde dónde se bifurcó el repositorio (Microsoft).

    ![Captura de pantalla de la cuenta de GitHub y nota](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Seleccione **Clone or download**(Clonar o descargar).

    ![Captura de pantalla de la opción de GitHub para clonar o descargar un repositorio](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. En la ventana **Clone with HTTPS** (Clonar con HTTPS), seleccione el icono de **copia**.

    ![Captura de pantalla de la opción de GitHub para copiar la dirección URL de clonación en el Portapapeles](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Abra una ventana de Git Bash o de terminal.

1. Cambie los directorios a la ubicación deseada en la que desea almacenar la copia local (clon) del repositorio.

1. Mediante el comando `git clone`, clone la dirección URL copiada anteriormente.

    ![Captura de pantalla del comando git clone de Git Bash](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Presione la tecla Entrar para iniciar el proceso de clonación.

    ![Captura de pantalla de resultados del comando git clone de Git Bash](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Cambie los directorios al directorio recién creado que contiene el clon del origen de la aplicación.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Creación y configuración de un clúster de Kubernetes administrado

En esta sección se realizarán los pasos siguientes:

- Use la CLI de Azure 2.0 para crear un clúster de Kubernetes administrado.
- Más información sobre cómo configurar un clúster mediante el script de configuración o manualmente.
- Cree una instancia del servicio Azure Container Registry.

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Uso de la CLI de Azure 2.0 para crear un clúster de Kubernetes administrado
Para crear un clúster de Kubernetes administrado con la [CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), asegúrese de que está usando la CLI de Azure 2.0.25 o posterior.

1. Inicie sesión en la cuenta de Azure. Después de escribir el siguiente comando, se proporcionan instrucciones que explican cómo realizar el inicio de sesión. 
    
    ```azurecli
    az login
    ```

1. Al ejecutar el comando `az login` en el paso anterior, se muestra una lista de todas las suscripciones de Azure (junto con sus identificadores de suscripción). En este paso, se establece la suscripción de Azure predeterminada. Reemplace el marcador de posición &lt;your-subscription-id> por el identificador de suscripción de Azure deseado. 

    ```azurecli
    az account set -s <your-subscription-id>
    ```

1. Cree un grupo de recursos. Reemplace el marcador de posición &lt;your-resource-group-name> por el nombre del nuevo grupo de recursos y reemplace el marcador de posición &lt;your-location> con la ubicación. El comando `az account list-locations` muestra todas las ubicaciones de Azure. Durante la versión preliminar de AKS, no todas las ubicaciones están disponibles. Si escribe una ubicación que no es válida en este momento, el mensaje de error muestra las ubicaciones disponibles.

    ```azurecli
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Cree un clúster de Kubernetes. Reemplace &lt;your-resource-group-name> por el nombre del grupo de recursos creado en el paso anterior y &lt;you-kubernetes-cluster-name> por el nombre del nuevo clúster. (Este proceso puede tardar varios minutos en completarse).

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Configuración del clúster de Kubernetes

Puede configurar una implementación azul/verde en AKS de forma manual o con un script de configuración proporcionado en el ejemplo clonado anteriormente. En esta sección, verá cómo aplicar ambas opciones.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Configuración del clúster de Kubernetes con el script de configuración de ejemplo
1. Edite el archivo **deploy/aks/setup/setup.sh** y reemplace los siguientes marcadores de posición con los valores apropiados del entorno: 

   - **&lt;your-resource-group-name>**
   - **&lt;your-kubernetes-cluster-name>**
   - **&lt;your-location>**
   - **&lt;your-dns-name-suffix>**

     ![Captura de pantalla del script setup.sh en Bash, con varios marcadores de posición resaltados](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Ejecute el script de configuración.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>Configuración manual de un clúster de Kubernetes 
1. Descargue la configuración de Kubernetes en la carpeta de perfil.

    ```azurecli
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Cambie el directorio por el directorio **deploy/aks/setup**. 

1. Ejecute los siguientes comandos **kubectl** para configurar los servicios para el punto de conexión público y los dos puntos de conexión de prueba.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Actualice el nombre DNS para los puntos de conexión público y de prueba. Al crear un clúster de Kubernetes, también se crea un [grupo de recursos adicional](https://github.com/Azure/AKS/issues/3) con el patrón de nomenclatura de **MC_&lt;your-resource-group-name> _&lt;your-kubernetes-cluster-name>_ &lt;your-location>** .

    Busque las direcciones IP públicas del grupo de recursos.

    ![Captura de pantalla de las direcciones IP públicas del grupo de recursos](./media/jenkins-aks-blue-green-deployment/publicip.png)

    Para cada uno de los servicios, busque la dirección IP externa mediante la ejecución del comando siguiente:
    
    ```bash
    kubectl get service todoapp-service
    ```
    
    Actualice el nombre DNS para la dirección IP correspondiente con el siguiente comando:

    ```azurecli
    az network public-ip update --dns-name aks-todoapp --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Repita la llamada para `todoapp-test-blue` y `todoapp-test-green`:

    ```azurecli
    az network public-ip update --dns-name todoapp-blue --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>

    az network public-ip update --dns-name todoapp-green --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    El nombre DNS debe ser único en la suscripción. Para garantizar la unicidad, puede usar `<your-dns-name-suffix>`.

### <a name="create-an-instance-of-container-registry"></a>Creación de una instancia de Container Registry

1. Ejecute el comando `az acr create` para crear una instancia de Container Registry. En la sección siguiente, puede usar `login server` como la dirección URL del registro de Docker.

    ```azurecli
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Ejecute el comando `az acr credential` para mostrar las credenciales de Container Registry. Anote el nombre de usuario y la contraseña del registro de Docker, los necesitará en la siguiente sección.

    ```azurecli
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Preparación del servidor Jenkins

En esta sección, verá cómo preparar el servidor de Jenkins para ejecutar una compilación, que sea adecuada para realizar pruebas. Sin embargo, debe usar un [agente de máquina virtual de Azure](https://plugins.jenkins.io/azure-vm-agents) o un [agente de Azure Container](https://plugins.jenkins.io/azure-container-agents) para establecer un agente de Azure para ejecutar las compilaciones. Para más información, consulte el artículo de Jenkins sobre las [implicaciones de seguridad de compilar en el servidor maestro](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master).

1. Implemente un [servidor maestro Jenkins en Azure](https://aka.ms/jenkins-on-azure).

1. Conéctese al servidor a través de SSH e instale las herramientas de compilación en el servidor donde se ejecuta la compilación.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Instale Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Asegúrese de que el usuario `jenkins` tenga permiso para ejecutar los comandos `docker`.

1. [Instale kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Descargue jq](https://stedolan.github.io/jq/download/).

1. Instale jq con el comando siguiente:

   ```bash
   sudo apt-get install jq
   ```
   
1. Instale los complementos en Jenkins mediante la aplicación de los pasos siguientes en el panel de Jenkins:

    1. Seleccione **Manage Jenkins > Manage Plugins > Available** (Administrar Jenkins > Administrar complementos > Disponibles).
    1. Busque e instale el complemento Azure Container Service.

1. Agregue las credenciales para administrar los recursos de Azure. Si aún no tiene el complemento, instale el complemento **Azure Credentials**.

1. Agregue la credencial de la entidad de servicio de Azure como el tipo **Microsoft Azure Service Principal** (Entidad de servicio de Microsoft Azure).

1. Agregue el nombre de usuario y la contraseña del registro de Docker para Azure (que obtuvo en la sección "Creación de una instancia de Azure Container Registry") como el tipo **Username with password** (Nombre de usuario con contraseña).

## <a name="edit-the-jenkinsfile"></a>Edición del archivo de Jenkins

1. En su propio repositorio, vaya a `/deploy/aks/` y abra `Jenkinsfile`.

2. Actualice el archivo de la forma siguiente:

    ```groovy
    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = '<your-resource-group-name>'
    def aks = '<your-kubernetes-cluster-name>'

    def cosmosResourceGroup = '<your-cosmodb-resource-group>'
    def cosmosDbName = '<your-cosmodb-name>'
    def dbName = '<your-dbname>'

    def dockerRegistry = '<your-acr-name>.azurecr.io'
    ```
    
    Actualice el identificador de credencial de Container Registry:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>Creación del trabajo
1. Agregue un nuevo trabajo de tipo **Canalización**.

1. Seleccione **Pipeline** > **Definition** > **Pipeline script from SCM** (Canalización > Definición > Script de canalización de SCM).

1. Escriba la dirección URL del repositorio de SCM con &lt;your-forked-repo>.

1. Escriba la ruta de acceso del script como `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>Ejecutar el trabajo

1. Verifique que puede ejecutar el proyecto correctamente en el entorno local. Aquí se indica cómo: [Ejecutar el proyecto en la máquina local](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it).

1. Ejecute el trabajo de Jenkins. La primera vez que se ejecuta el trabajo, Jenkins implementa la aplicación de tareas pendientes en el entorno azul, que es el entorno inactivo predeterminado. 

1. Para comprobar que se ejecutó el trabajo, vaya a estas direcciones URL:
    - Punto de conexión público: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Punto de conexión azul: `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Punto de conexión verde: `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

Los puntos de conexión de prueba público y azul tienen la misma actualización, mientras que el punto de conexión verde muestra la imagen de Tomcat predeterminada. 

Si ejecuta la compilación más de una vez, recorre las implementaciones azul y verde. En otras palabras, si el entorno actual es azul, el trabajo se implementa y se prueba en el entorno verde. Luego, si las pruebas son correctas, el trabajo actualiza el punto de conexión público de aplicación para enrutar el tráfico al entorno verde.

## <a name="additional-information"></a>Información adicional

Para más información sobre la implementación sin tiempo de inactividad, consulte esta [plantilla de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos creados en este tutorial, puede eliminarlos.

```azurecli
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Solución de problemas

Si detecta algún error con los complementos de Jenkins, envíe un problema en [Jenkins JIRA](https://issues.jenkins-ci.org/) para el componente específico.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió cómo realizar implementaciones en AKS con Jenkins y el patrón de implementación azul/verde. Para obtener más información sobre el proveedor de Jenkins para Azure, consulte el sitio de Jenkins en Azure.

> [!div class="nextstepaction"]
> [Jenkins en Azure](/azure/jenkins/)