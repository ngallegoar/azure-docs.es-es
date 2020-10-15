---
title: Implementación desde un repositorio GIT local
description: Aprenda a habilitar la implementación de Git local en Azure App Service. Una de las formas más sencillas de implementar código desde una máquina local.
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.topic: article
ms.date: 06/18/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: efe4c07a6231e0b2c95b049db056a4e5d055db98
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77152999"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Implementación de Git local en Azure App Service

En esta guía se muestra cómo implementar la aplicación en [Azure App Service](overview.md) desde un repositorio Git en la máquina local.

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos de esta guía:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Instale Git](https://www.git-scm.com/downloads).
  
- Obtenga un repositorio Git local con el código que quiere implementar. Para descargar un repositorio de ejemplo, ejecute el comando siguiente en la ventana del terminal local:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-build-server"></a>Implementación con el servidor de compilación Kudu

La manera más fácil de habilitar la implementación de Git local para la aplicación con el servidor de compilación Kudu App Service es usar Azure Cloud Shell. 

### <a name="configure-a-deployment-user"></a>Configuración de un usuario de implementación

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>Obtener la dirección URL de implementación

Para obtener la URL para habilitar la implementación de Git local de una aplicación existente, ejecute [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) en Cloud Shell. Reemplace \<app-name> y \<group-name> por los nombres de la aplicación y su grupo de recursos de Azure.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```
> [!NOTE]
> Si usa un plan de servicio de aplicaciones de Linux, debe agregar este parámetro:--runtime python|3.7


Igualmente, para crear una aplicación habilitada para Git, ejecute [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) en Cloud Shell con el parámetro `--deployment-local-git`. Reemplace \<app-name>, \<group-name> y \<plan-name> por los nombres de la nueva aplicación de Git, su grupo de recursos de Azure y su plan de Azure App Service.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

Cualquiera de los comandos devuelve una dirección URL como: `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Use esta dirección URL para implementar la aplicación en el siguiente paso.

En lugar de usar esta dirección URL de nivel de cuenta, también puede habilitar la instancia de Git local con las credenciales del nivel de aplicación. Azure App Service genera automáticamente estas credenciales para cada aplicación. 

Puede obtener las credenciales de la aplicación ejecutando el siguiente comando en Cloud Shell. Reemplace \<app-name> y \<group-name> por el nombre de la aplicación y el nombre del grupo de recursos de Azure.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Use la dirección URL que vuelve a implementar la aplicación en el siguiente paso.

### <a name="deploy-the-web-app"></a>Implementación de la aplicación web

1. Abra una ventana del terminal local en su repositorio de Git local y agregue una instancia remota de Azure. En el siguiente comando, reemplace \<url> por la dirección URL específica del usuario de la implementación o la dirección URL específica de la aplicación que ha obtenido en el paso anterior.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Acceda a la instancia remota de Azure con `git push azure master`. 
   
1. En la ventana del **administrador de credenciales de Git**, escriba su [contraseña de usuario de implementación](#configure-a-deployment-user), no la contraseña de inicio de sesión de Azure.
   
1. Revise el resultado. Es posible que vea la automatización específica para el entorno de tiempo de ejecución, como MSBuild para ASP.NET, `npm install` para Node.js y `pip install` para Python. 
   
1. Busque su aplicación en Azure Portal para comprobar que el contenido esté implementado.

## <a name="deploy-with-azure-pipelines-builds"></a>Implementación con compilaciones de Azure Pipelines

Si su cuenta tiene los permisos necesarios, puede configurar Azure Pipelines (versión preliminar) para habilitar la implementación de Git local para la aplicación. 

- Su cuenta de Azure debe tener permisos para escribir en Azure Active Directory y crear un servicio. 
  
- Su cuenta de Azure debe tener el rol **Propietario** en su suscripción de Azure.

- Debe ser administrador en el proyecto de Azure DevOps que quiera usar.

Para habilitar la implementación de Git local para la aplicación con Azure Pipelines (versión preliminar):

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **App Services**. 

1. Seleccione la aplicación de Azure App Service y elija **Centro de implementación** en el menú de la izquierda.
   
1. En la página del **centro de implementación** , seleccione **Local Git** (Git local) y **Continue** (Continuar). 
   
   ![Seleccione Local Git (Git local) y luego seleccione Continue (Continuar)](media/app-service-deploy-local-git/portal-enable.png)
   
1. En la página **Build provider** (Compilar proveedor), seleccione **Azure Pipelines (Preview)** (Azure Pipelines [versión preliminar]) y **Continue** (Continuar). 
   
   ![Seleccione Azure Pipelines (Preview) (Azure Pipelines [versión preliminar]) y luego Continue (Continuar).](media/app-service-deploy-local-git/pipeline-builds.png)

1. En la página **Configure** (Configurar), configure una nueva organización de Azure DevOps o especifique una organización existente y, a continuación, seleccione **Continue** (Continuar).
   
   > [!NOTE]
   > Si su organización Azure DevOps existente no figura en la lista, es posible que deba vincularla a su suscripción de Azure. Para obtener más información, consulte [Define your CD release pipeline](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd) (Definir la canalización de versión de CD).
   
1. Dependiendo de su [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/plans/) de App Service, puede ver una página **Deploy to staging** (Implementar en el almacenamiento provisional). Dcida si quiere [habilitar las ranuras de implementación](deploy-staging-slots.md) y seleccione **Continue** (Continuar).
   
1. En la página **Summary** (Resumen), revise la configuración y seleccione **Finish** (Finalizar).
   
1. Cuando Azure Pipeline esté listo, copie la dirección URL del repositorio de Git desde la página del **centro de implementación** para usarla en el siguiente paso. 
   
   ![Copie la dirección URL del repositorio de Git.](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. En la ventana del terminal local, agregue una instancia remota de Azure al repositorio de Git local. En el comando, reemplace \<url> por la dirección URL del repositorio de Git que ha obtenido en el paso anterior.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Acceda a la instancia remota de Azure con `git push azure master`. 
   
1. En la página del **administrador de credenciales de GIT**, inicie sesión con su nombre de usuario de visualstudio.com. Para ver otros métodos de autenticación, consulte la [información general sobre autenticación de Azure DevOps Services](/vsts/git/auth-overview?view=vsts).
   
1. Una vez finalizada la implementación, puede consultar el progreso de la compilación en `https://<azure_devops_account>.visualstudio.com/<project_name>/_build` y el progreso de la implementación en `https://<azure_devops_account>.visualstudio.com/<project_name>/_release`.
   
1. Busque su aplicación en Azure Portal para comprobar que el contenido esté implementado.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Solución de problemas de implementación

Puede ver los siguientes mensajes de error comunes cuando usa Git para publicar en una aplicación de App Service en Azure:

|Message|Causa|Resolución
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|La aplicación no está funcionando.|inicie la aplicación en Azure Portal. La implementación de GIT no está disponible cuando la aplicación web está detenida.|
|`Couldn't resolve host 'hostname'`|La información de dirección del repositorio remoto de "azure" es incorrecta.|use el comando `git remote -v` para obtener un listado de todos los remotos, junto con la dirección URL asociada. Compruebe que la URL para el repositorio correcto "azure" es correcta. Si lo necesita, suprima y vuelva a crear este repositorio remoto utilizando la URL correcta.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`|No especificó una rama durante `git push`, o no ha establecido el valor `push.default` en `.gitconfig`.|Vuelva a ejecutar `git push` y especifique la rama principal `git push azure master`.|
|`src refspec [branchname] does not match any.`|Intentó agregar una rama que no es maestra en el repositorio remoto "azure".|Vuelva a ejecutar `git push` y especifique la rama principal `git push azure master`.|
|`RPC failed; result=22, HTTP code = 5xx.`|este error puede producirse si se trata de insertar un repositorio Git de gran tamaño a través de HTTPS.|Cambie la configuración de Git en la máquina local para aumentar el tamaño de `postBuffer`. Por ejemplo: `git config --global http.postBuffer 524288000`.|
|`Error - Changes committed to remote repository but your web app not updated.`|Implementó una aplicación Node.js con un archivo _package.json_ que especifica los módulos adicionales requeridos.|Revise los mensajes de error `npm ERR!` anteriores a este error para obtener más contexto sobre el error. A continuación se indican las causas conocidas de este error y los mensajes `npm ERR!`:<br /><br />**El archivo package.json tiene una estructura incorrecta**: `npm ERR! Couldn't read dependencies.`<br /><br />**Módulo nativo que no tiene una distribución binaria para Windows**:<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />or <br />`npm ERR! [modulename@version] preinstall: \make || gmake\ `|

## <a name="additional-resources"></a>Recursos adicionales

- [Documentación de Project Kudu](https://github.com/projectkudu/kudu/wiki)
- [Implementación continua en Azure App Service](deploy-continuous-deployment.md)
- [Ejemplo: Creación de una aplicación web e implementación de código desde un repositorio local de Git (CLI de Azure)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Ejemplo: Creación de una aplicación web e implementación de código desde un repositorio local de Git (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
