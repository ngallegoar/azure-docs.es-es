---
title: 'Tutorial: Implementación desde GitHub en Azure App Service con Jenkins'
description: Configuración de Jenkins para la integración continua (CI) desde GitHub y la implementación continua (CD) en Azure App Service para aplicaciones web de Java
ms.topic: tutorial
ms.date: 10/23/2019
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 9fcf178b71ac1f07bfb58cd2502701ae5392b472
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74158392"
---
# <a name="tutorial-deploy-from-github-to-azure-app-service-with-jenkins-continuous-integration-and-deployment"></a>Tutorial: Implementación desde GitHub en Azure App Service con la integración e implementación continuas de Jenkins

En este tutorial se implementa una aplicación web de Java de ejemplo de GitHub en [Azure App Service en Linux](/azure/app-service/containers/app-service-linux-intro) mediante la integración (CI) e implementación continuas (CD) en Jenkins. Cuando actualiza la aplicación insertando confirmaciones en GitHub, Jenkins compila automáticamente la aplicación y vuelve a publicarla en Azure App Service. La aplicación de ejemplo de este tutorial se desarrolló mediante la plataforma [Spring Boot](https://projects.spring.io/spring-boot/). 

![Introducción a la implementación de GitHub en Azure App Service](media/tutorial-jenkins-deploy-web-app-azure-app-service/azure-continuous-integration-deployment-overview.png)

En este tutorial va a completar estas tareas:

> [!div class="checklist"]
> * Instalación de los complementos de Jenkins para poder realizar la compilación desde GitHub, implementar en Azure App Service y realizar otras tareas relacionadas.
> * Creación de una bifurcación del repositorio de GitHub de ejemplo para disponer de una copia de trabajo.
> * Conexión de Jenkins a GitHub.
> * Creación de una entidad de servicio de Azure para que Jenkins pueda acceder a Azure sin usar las credenciales.
> * Incorporación de la entidad de servicio a Jenkins.
> * Creación de la canalización de Jenkins que compila e implementa la aplicación de ejemplo cada vez que actualiza la aplicación en GitHub.
> * Creación de archivos de compilación e implementación para la canalización de Jenkins.
> * Apuntar hacia la canalización de Jenkins en el script de compilación e implementación.
> * Implementación de la aplicación de ejemplo en Azure mediante la ejecución de una compilación manual.
> * Inserción de una actualización de la aplicación en GitHub que desencadene Jenkins para compilar y volver a realizar la implementación en Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita estos elementos:

* Un servidor [Jenkins](https://jenkins.io/) con las herramientas Java Development Kit (JDK) y Maven en una máquina virtual Linux de Azure

  Si no tiene un servidor Jenkins, complete estos pasos en Azure Portal: [Creación de un servidor Jenkins en una máquina virtual Linux de Azure](/azure/jenkins/install-jenkins-solution-template)

* Una cuenta de [GitHub](https://github.com) para poder obtener una copia de trabajo (bifurcación) de la aplicación web de Java de ejemplo. 

* [CLI de Azure](/cli/azure/install-azure-cli), que puede ejecutar desde la línea de comandos local o desde [Azure Cloud Shell](/azure/cloud-shell/overview)

## <a name="install-jenkins-plug-ins"></a>Instalación de los complementos de Jenkins

1. Inicie sesión en la consola web de Jenkins en esta ubicación:

   `https://<Jenkins-server-name>.<Azure-region>.cloudapp.azure.com`

1. En la página principal de Jenkins, seleccione **Manage Jenkins (Administrar Jenkins)**  > **Manage Plugins** (Administrar complementos).

   ![Administrar complementos de Jenkins](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-jenkins-plugins.png)

1. En la pestaña **Available** (Disponible), seleccione estos complementos:

   - [Azure App Service](https://plugins.jenkins.io/azure-app-service)
   - [GitHub Branch Source](https://plugins.jenkins.io/github-branch-source) (Origen de bifurcación de GitHub)
   - [Environment Injector Plugin](https://plugins.jenkins.io/envinject) (Complemento inyector de entorno) de Jenkins
   - [Credenciales de Azure](https://plugins.jenkins.io/azure-credentials)

   Si no aparecen estos complementos, asegúrese de que no estén ya instalados consultando la pestaña **Installed** (Instalados).

1. Para instalar los complementos seleccionados, seleccione **Download now and install after restart** (Descargar ahora e instalar después de reiniciar).

1. Cuando haya terminado, en el menú de Jenkins, seleccione **Manage Jenkins** (Administrar Jenkins) para volver a la página de administración de Jenkins para pasos posteriores.

## <a name="fork-sample-github-repo"></a>Realizar bifurcación del repositorio de GitHub de ejemplo

1. [Inicie sesión en el repositorio de GitHub para la aplicación de ejemplo de Spring Boot](https://github.com/spring-guides/gs-spring-boot). 

1. En la esquina superior derecha de GitHub, seleccione **Fork** (Bifurcar).

   ![Realizar bifurcación del repositorio desde GitHub](media/tutorial-jenkins-deploy-web-app-azure-app-service/fork-github-repo.png)

1. Siga las indicaciones para seleccionar la cuenta de GitHub y finalizar la bifurcación.

A continuación, configure Jenkins con sus credenciales de GitHub.

## <a name="connect-jenkins-to-github"></a>Conexión de Jenkins a GitHub

Para que Jenkins supervise a GitHub y responda cuando se inserten nuevas confirmaciones en la aplicación web de la bifurcación de GitHub, habilite los [Webhooks de GitHub](https://developer.github.com/webhooks/) en Jenkins.

> [!NOTE]
> 
> Estos pasos permiten crear credenciales de token de acceso personal para que Jenkins trabaje con GitHub mediante su nombre de usuario y contraseña. 
> No obstante, si la cuenta de GitHub usa la autenticación en dos fases, cree el token en GitHub y configure Jenkins para que use ese token en su lugar. 
> Para más información, consulte la documentación relativa al [complemento de GitHub para Jenkins](https://wiki.jenkins.io/display/JENKINS/GitHub+Plugin).

1. En la página **Manage Jenkins** (Administrar Jenkins), seleccione **Configure System** (Configurar sistema). 

   ![Configurar el sistema en Jenkins](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-jenkins-configure-system.png)

1. En la sección **GitHub** proporcione los detalles del servidor de GitHub. En la lista **Add GitHub Server** (Agregar servidor de GitHub), seleccione **GitHub Server** (Servidor de GitHub). 

   ![Agregar servidor de GitHub en Jenkins](media/tutorial-jenkins-deploy-web-app-azure-app-service/add-GitHub-server.png)

1. Si la propiedad **Manage hooks** (Administrar enlaces) no está seleccionada, selecciónela. Seleccione **Avanzadas** para especificar otros valores de configuración. 

   ![Especificar configuración avanzada de Jenkins para el servidor de GitHub](media/tutorial-jenkins-deploy-web-app-azure-app-service/advanced-GitHub-settings.png)

1. En la lista **Manage additional GitHub actions** (Administrar acciones de GitHub adicionales), seleccione **Convert login and password to token** (Convertir contraseña e inicio de sesión en token).

   ![Convertir el inicio de sesión y la contraseña en token para GitHub](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-additional-actions.png)

1. Seleccione **From login and password** (A partir del inicio de sesión y la contraseña) para poder escribir el nombre de usuario y la contraseña de GitHub. Cuando haya terminado, seleccione **Create token credentials** (Crear credenciales de token), que permite crear un [token de acceso personal (PAT) de GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).   

   ![Creación de un PAT de GitHub a partir del inicio de sesión y la contraseña](media/tutorial-jenkins-deploy-web-app-azure-app-service/create-github-token-credentials.png)

1. En la sección **GitHub Server** (Servidor de GitHub), en la lista **Credentials** (Credenciales), seleccione el nuevo token. Compruebe que la autenticación funciona eligiendo **Prueba de conexión**.

   ![Comprobar conexión al servidor de GitHub con el nuevo PAT](media/tutorial-jenkins-deploy-web-app-azure-app-service/check-github-connection.png)

A continuación, cree la entidad de servicio que usa Jenkins para autenticar y acceder a los recursos de Azure.

## <a name="create-service-principal"></a>Creación de una entidad de servicio

En una sección posterior, cree un trabajo de canalización de Jenkins que compile la aplicación desde GitHub y la implemente en Azure App Service. Para hacer que Jenkins acceda a Azure sin tener que escribir las credenciales, cree una [entidad de servicio](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) en Azure Active Directory para Jenkins. Una entidad de servicio es una identidad independiente que Jenkins puede usar para autenticar el acceso a los recursos de Azure. Para crear esta entidad de servicio, ejecute el comando [ **`az ad sp create-for-rbac`** ](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) de la CLI de Azure, desde la línea de comandos local o desde Azure Cloud Shell, por ejemplo: 

```azurecli-interactive
az ad sp create-for-rbac --name "yourAzureServicePrincipalName" --password yourSecurePassword
```

Asegúrese de que utiliza comillas alrededor del nombre de entidad de seguridad de servicio. Igualmente, cree una contraseña segura que se base en las [reglas y restricciones de contraseñas de Azure Active Directory](/azure/active-directory/active-directory-passwords-policy). Si no proporciona una contraseña, la CLI de Azure creará una automáticamente. 

Esta es la salida que genera el comando **`create-for-rbac`** : 

```json
{
   "appId": "yourAzureServicePrincipal-ID", // A GUID such as AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA
   "displayName": "yourAzureServicePrincipalName", // A user-friendly name for your Azure service principal
   "name": "http://yourAzureServicePrincipalName",
   "password": "yourSecurePassword",
   "tenant": "yourAzureActiveDirectoryTenant-ID" // A GUID such as BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB
}
```

> [!TIP]
> 
> Si ya tiene una entidad de servicio, puede usarla de nuevo aquí.
> Al proporcionar los valores de la entidad de servicio para la autenticación, utilice los valores de propiedad `appId`, `password` y `tenant`. 
> Cuando busque una entidad de servicio ya existente, use el valor de propiedad `displayName`.

## <a name="add-service-principal-to-jenkins"></a>Incorporación de la entidad de servicio a Jenkins

1. En la página principal de Jenkins, seleccione **Credentials** (Credenciales) > **System**(Sistema). 

1. En la página **System** (Sistema), en **Domain** (Dominio), seleccione **Global credentials (unrestricted)** (Credenciales globales (sin restricciones)).

1. En el menú izquierdo, seleccione **Add Credentials** (Agregar credenciales).

1. En la lista **Kind** (Tipo), seleccione **Azure Service Principal** (Entidad de servicio de Azure).

1. Proporcione la información de la suscripción de Azure y la entidad de servicio en las propiedades que se describen en la tabla de este paso:

   ![Agregar credenciales de la entidad de servicio de Azure](media/tutorial-jenkins-deploy-web-app-azure-app-service/add-service-principal-credentials.png)

   | Propiedad | Value | Descripción | 
   |----------|-------|-------------| 
   | **Subscription ID** | <*yourAzureSubscription-ID*> | El valor GUID de la suscripción de Azure <p>**Sugerencia**: si no conoce el identificador de suscripción de Azure, ejecute este comando de la CLI de Azure en la línea de comandos o en Cloud Shell y, a continuación, utilice el valor GUID `id`: <p>`az account list` | 
   | **Id. de cliente** | <*yourAzureServicePrincipal-ID*> | El valor GUID `appId` que se generó anteriormente para la entidad de servicio de Azure | 
   | **Secreto de cliente** | <*yourSecurePassword*> | El valor `password` o "secreto" que proporcionó para la entidad de servicio de Azure | 
   | **Id. de inquilino** | <*yourAzureActiveDirectoryTenant-ID*> | El valor GUID `tenant` para el inquilino de Azure Active Directory | 
   | **Id** | <*yourAzureServicePrincipalName*> | El valor `displayName` de la entidad de servicio de Azure. | 

1. Para confirmar que la entidad de servicio funciona, seleccione **Verify Service Principal** (Comprobar entidad de servicio). Cuando finalice, seleccione **Aceptar**.

A continuación, cree la canalización de Jenkins que compila e implementa la aplicación.

## <a name="create-jenkins-pipeline"></a>Creación de una canalización de Jenkins

En Jenkins, cree el trabajo de canalización para compilar e implementar la aplicación.

1. Vuelva a la página principal de Jenkins y seleccione **New Item** (Nuevo elemento). 

   ![Creación de una canalización de Jenkins](media/tutorial-jenkins-deploy-web-app-azure-app-service/jenkins-select-new-item.png)

1. Proporcione un nombre para el trabajo de canalización, por ejemplo, "My-Java-Web-App" y seleccione **Pipeline** (Canalización). En la parte inferior, seleccione **OK** (Aceptar).  

   ![Asignar un nombre al trabajo de canalización de Jenkins](media/tutorial-jenkins-deploy-web-app-azure-app-service/jenkins-select-pipeline.png)

1. Configure Jenkins con la entidad de servicio para que este se pueda implementar en Azure sin usar sus propias credenciales.

   1. En la pestaña **General**, seleccione **Prepare an environment for the run** (Preparar un entorno para la ejecución). 

   1. En el cuadro **Properties Content** (Contenido de propiedades) que aparece, agregue estas variables de entorno y sus valores. 

      ```ini
      AZURE_CRED_ID=yourAzureServicePrincipalName
      RES_GROUP=yourWebAppAzureResourceGroupName
      WEB_APP=yourWebAppName
      ```

      ![Preparar un entorno para la ejecución y establecer las variables de entorno](media/tutorial-jenkins-deploy-web-app-azure-app-service/prepare-environment-for-jenkins-run.png)

1. Cuando finalice, seleccione **Guardar**.

A continuación, cree los scripts de compilación e implementación para Jenkins.

## <a name="create-build-and-deployment-files"></a>Creación de los archivos de compilación e implementación

Ahora, cree los archivos que usará Jenkins para compilar e implementar la aplicación.

1. En la carpeta `src/main/resources/` de la bifurcación de GitHub, cree este archivo de configuración llamado `web.config`, que contiene este XML pero reemplace `$(JAR_FILE_NAME)` por `gs-spring-boot-0.1.0.jar`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
      <system.webServer>
         <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
         </handlers>
         <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\${JAR_FILE_NAME}&quot;"></httpPlatform>
      </system.webServer>
   </configuration>
   ```

1. En la carpeta raíz de la bifurcación de GitHub, cree este script de compilación e implementación denominado `Jenkinsfile`, que contiene este texto ([aquí puede encontrar el código fuente en GitHub](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/doc/resources/jenkins/Jenkinsfile-webapp-se)):

   ```groovy
   node {
      stage('init') {
         checkout scm
      }
      stage('build') {
         sh '''
            mvn clean package
            cd target
            cp ../src/main/resources/web.config web.config
            cp todo-app-java-on-azure-1.0-SNAPSHOT.jar app.jar 
            zip todo.zip app.jar web.config
         '''
      }
      stage('deploy') {
         azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
         resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "**/todo.zip"
      }
   }
   ```

1. Confirme los archivos `web.config` y `Jenkinsfile` en la bifurcación de GitHub e inserte los cambios.

## <a name="point-pipeline-at-script"></a>Apuntar a la canalización en el script

Ahora, especifique el script de compilación e implementación que desea que use Jenkins.

1. En Jenkins, seleccione el trabajo de canalización que creó anteriormente. 

   ![Seleccionar trabajo de canalización de Jenkins para una aplicación web](media/tutorial-jenkins-deploy-web-app-azure-app-service/select-pipeline-job.png)

1. En el menú de la izquierda, seleccione **Configure** (Configurar).

1. En la pestaña **Pipeline**, en la lista **Definition** (Definición), seleccione **Pipeline script from SCM** (Script de canalización del SCM).

   1. En el cuadro **SCM** que aparece, seleccione **Git** como control de código fuente. 

   1. En la sección **Repositories**, en **Repository URL** (Dirección URL del repositorio), escriba la dirección URL de la bifurcación de GitHub, por ejemplo: 

      `https://github.com/<your-GitHub-username>/gs-spring-boot`

   1. En **Credentials**, seleccione el token de acceso personal de GitHub que creó anteriormente.

   1. En el cuadro **Script Path** (Ruta de acceso del script), agregue la ruta al script "Jenkinsfile".

   Una vez que haya terminado, la definición de la canalización debe ser parecida a la de este ejemplo: 

   ![Señalar una canalización de Jenkins en el script](media/tutorial-jenkins-deploy-web-app-azure-app-service/set-up-jenkins-github.png)

1. Cuando finalice, seleccione **Guardar**.

Después, compile e implemente la aplicación en Azure App Service. 

## <a name="build-and-deploy-to-azure"></a>Compilación e implementación en Azure

1. Con la CLI de Azure, ya sea desde la línea de comandos o desde Azure Cloud Shell, cree una [aplicación web de Azure App Service en Linux](/azure/app-service/containers/app-service-linux-intro) en la que Jenkins implemente la aplicación web después de finalizar una compilación. Asegúrese de que la aplicación web tiene un nombre único.

   ```azurecli-interactive
   az group create --name yourWebAppAzureResourceGroupName --location yourAzureRegion
   az appservice plan create --name yourLinuxAppServicePlanName --resource-group yourWebAppAzureResourceGroupName --is-linux
   az webapp create --name yourWebAppName --resource-group yourWebAppAzureResourceGroupName --plan yourLinuxAppServicePlanName --runtime "java|1.8|Tomcat|8.5"
   ```

   Para más información acerca de estos comandos de la CLI de Azure, consulte estas páginas:

   * [**`az group create`**](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)

   * [**`az appservice plan create`**](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create)

   * [**`az webapp create`**](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)

1. En Jenkins, seleccione el trabajo de canalización y seleccione **Build Now** (Compilar ahora).

   Una vez que termina la compilación, Jenkins implementa la aplicación, que ahora está activa en Azure en la dirección URL de publicación. Por ejemplo: 

   `http://<your-Java-web-app>.azurewebsites.net`

   ![Visualización de la aplicación implementada en Azure](media/tutorial-jenkins-deploy-web-app-azure-app-service/greetings-unedited.png)

## <a name="push-changes-and-redeploy"></a>Confirmación de cambios y reimplementación

1. En el explorador web, vaya a esta ubicación en la bifurcación de GitHub de la aplicación web:

   `complete/src/main/java/Hello/Application.java`
   
1. En la esquina superior derecha de GitHub, seleccione **Edit this file** (Editar este archivo).

1. Realice este cambio en el método `commandLineRunner()` y confírmelo en la bifurcación `master` del repositorio. Esta confirmación en la bifurcación `master` inicia una compilación en Jenkins. 
   
   ```java
   System.out.println("Let's inspect the beans provided by Spring Boot on Azure");
   ```

1. Una vez que finaliza la compilación, y después de que Jenkins se vuelva a implementar en Azure, actualice la aplicación y aparecerá ya la actualización.

   ![Visualización de la aplicación implementada en Azure](media/tutorial-jenkins-deploy-web-app-azure-app-service/greetings-edited.png)

## <a name="troubleshooting-the-jenkins-plug-in"></a>Solución de problemas del complemento de Jenkins

Si detecta algún error con los complementos de Jenkins, registre un problema en [Jenkins JIRA](https://issues.jenkins-ci.org/) para el componente específico.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Usar máquinas virtuales de Azure como agentes de compilación](/azure/jenkins/jenkins-azure-vm-agents)
