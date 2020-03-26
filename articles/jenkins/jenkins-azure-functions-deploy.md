---
title: Implementación en Azure Functions con el complemento para Azure Functions de Jenkins
description: Aprenda a implementar en Azure Functions con el complemento para Azure Functions de Jenkins
keywords: jenkins, azure, devops, java, azure functions
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: 731bac13a596bbeaf970b3f6ce976a582d1f11ae
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78250921"
---
# <a name="deploy-to-azure-functions-using-the-jenkins-azure-functions-plug-in"></a>Implementación en Azure Functions con el complemento para Azure Functions de Jenkins

[Azure Functions](/azure/azure-functions/) es un servicio de proceso sin servidor. Con Azure Functions, puede ejecutar código a petición sin aprovisionar ni administrar infraestructura. En este tutorial se muestra cómo implementar una función de Java en Azure Functions mediante el complemento para Azure Functions.

## <a name="prerequisites"></a>Prerrequisitos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- **Servidor Jenkins**: Si no tiene instalado un servidor Jenkins, consulte el artículo sobre la [Creación de un servidor Jenkins en Azure](./install-jenkins-solution-template.md).

  > [!TIP]
  > El código fuente utilizado para este tutorial se encuentra en el [repositorio de GitHub de Visual Studio China](https://github.com/VSChina/odd-or-even-function/blob/master/src/main/java/com/microsoft/azure/Function.java).

## <a name="create-a-java-function"></a>Creación de una función de Java

Para crear una función de Java con la pila del entorno de tiempo de ejecución de Java, utilice [Azure Portal](https://portal.azure.com) o la [CLI de Azure](/cli/azure/?view=azure-cli-latest).

Los siguientes pasos muestran cómo crear una función de Java mediante la CLI de Azure:

1. Cree un grupo de recursos y sustituya el marcador de posición **&lt;resource_group>** por el nombre de este.

    ```azurecli
    az group create --name <resource_group> --location eastus
    ```

1. Cree una cuenta de almacenamiento de Azure y sustituya los marcadores de posición por los valores adecuados.
 
    ```azurecli
    az storage account create --name <storage_account> --location eastus --resource-group <resource_group> --sku Standard_LRS    
    ```

1. Cree la aplicación de función de prueba y sustituya los marcadores de posición por los valores adecuados.

    ```azurecli
    az functionapp create --resource-group <resource_group> --consumption-plan-location eastus --name <function_app> --storage-account <storage_account>
    ```

## <a name="prepare-jenkins-server"></a>Preparación del servidor Jenkins

En los siguientes pasos se explica cómo preparar el servidor Jenkins:

1. Implemente un [servidor Jenkins](https://aka.ms/jenkins-on-azure) en Azure. Si aún no tiene una instancia del servidor Jenkins instalado, el artículo sobre la [Creación de un servidor Jenkins en Azure](./install-jenkins-solution-template.md) le guiará por el proceso.

1. Inicie sesión en la instancia de Jenkins con SSH.

1. En la instancia de Jenkins, instale maven mediante el siguiente comando:

    ```terminal
    sudo apt install -y maven
    ```

1. En la instancia de Jenkins, instale [Azure Functions Core Tools](/azure/azure-functions/functions-run-local); para ello, ejecute los siguientes comandos en un símbolo del sistema del terminal:

    ```terminal
    wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    sudo apt-get update
    sudo apt-get install azure-functions-core-tools
    ```

1. En el panel de Jenkins, instale los siguientes complementos:

    - Complemento de Azure Functions
    - Complemento EnvInject

1. Jenkins necesita una entidad de servicio de Azure para la autenticación y el acceso a los recursos de Azure. Consulte el artículo de [Implementación en Azure App Service](./tutorial-jenkins-deploy-web-app-azure-app-service.md) para instrucciones paso a paso.

1. Con la entidad de servicio de Azure, agregue el tipo de credenciales "Entidad de servicio de Microsoft Azure" en Jenkins. Consulte el tutorial de [Implementación en Azure App Service](./tutorial-jenkins-deploy-web-app-azure-app-service.md#add-service-principal-to-jenkins).

## <a name="fork-the-sample-github-repo"></a>Bifurcación del repositorio de GitHub de ejemplo

1. [Inicie sesión en el repositorio de GitHub para la aplicación de ejemplo par o impar](https://github.com/VSChina/odd-or-even-function.git).

1. En la esquina superior derecha de GitHub, elija **Fork** (Bifurcar).

1. Siga las indicaciones para seleccionar la cuenta de GitHub y finalizar la bifurcación.

## <a name="create-a-jenkins-pipeline"></a>Creación de una canalización de Jenkins

En esta sección, creará la [canalización de Jenkins](https://jenkins.io/doc/book/pipeline/).

1. En el panel de Jenkins, cree una canalización.

1. Habilite **Prepare an environment for the run** (Preparar un entorno para la ejecución).

1. Agregue las siguientes variables de entorno en **Properties Content** (Contenido de las propiedades) y sustituya los marcadores de posición por los valores adecuados para su entorno:

    ```
    AZURE_CRED_ID=<service_principal_credential_id>
    RES_GROUP=<resource_group>
    FUNCTION_NAME=<function_name>
    ```
    
1. En la sección **Pipeline->Definition** (Canalización->Definición), seleccione **Pipeline script from SCM** (Canalización de SCM).

1. Especifique la dirección URL y la ruta de acceso del script de la bifurcación de GitHub ("doc/recursos/jenkins/JenkinsFile") para utilizar en el [ejemplo de JenkinsFile](https://github.com/VSChina/odd-or-even-function/blob/master/doc/resources/jenkins/JenkinsFile).

   ```
   node {
    stage('Init') {
        checkout scm
        }

    stage('Build') {
        sh 'mvn clean package'
        }

    stage('Publish') {
        azureFunctionAppPublish appName: env.FUNCTION_NAME, 
                                azureCredentialsId: env.AZURE_CRED_ID, 
                                filePath: '**/*.json,**/*.jar,bin/*,HttpTrigger-Java/*', 
                                resourceGroup: env.RES_GROUP, 
                                sourceDirectory: 'target/azure-functions/odd-or-even-function-sample'
        }
    }
    ```

## <a name="build-and-deploy"></a>Compilación e implementación

Ahora es el momento de ejecutar el trabajo de Jenkins.

1. En primer lugar, siga las instrucciones del artículo [Enlaces y desencadenadores HTTP de Azure Functions](/azure/azure-functions/functions-bindings-http-webhook-trigger#authorization-keys) para obtener la clave de autorización.

1. En el explorador, escriba la dirección URL de la aplicación. Reemplace los marcadores de posición por los valores adecuados y especifique un valor numérico para **&lt;input_number>** como entrada para la función de Java.

    ```
    https://<function_app>.azurewebsites.net/api/HttpTrigger-Java?code=<authorization_key>&number=<input_number>
    ```
1. Verá resultados similares a la salida del ejemplo siguiente (donde se utilizó un número impar [365] como prueba):

    ```output
    The number 365 is Odd.
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine los recursos que ha creado con el siguiente paso:

```azurecli
az group delete -y --no-wait -n <resource_group>
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Functions, consulte el siguiente recurso:
> [!div class="nextstepaction"]
> [Documentación de Azure Functions](/azure/azure-functions/)
