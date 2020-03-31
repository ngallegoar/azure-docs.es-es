---
title: Configuración de la implementación continua
description: Aprenda a habilitar CI/CD para Azure App Service desde GitHub, BitBucket, Azure Repos u otro repositorios. Seleccione la canalización de compilación que se ajuste a sus necesidades.
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.topic: article
ms.date: 08/23/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: d58eb333c930d2ffac4eb57340ea776338325181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231644"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Implementación continua en Azure App Service

[Azure App Service](overview.md) le permite realizar implementaciones continuas desde los repositorios de GitHub, BitBucket y [Azure Repos](https://azure.microsoft.com/services/devops/repos/) al incorporar las últimas actualizaciones. En este artículo le mostramos cómo usar Azure Portal para implementar continuamente la aplicación a través del servicio de compilación Kudu o [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/). 

Para obtener más información sobre los servicios de control de código fuente, consulte [Crear un repositorio (GitHub)], [Crear un repositorio (BitBucket)] o [Crear un nuevo repositorio de Git (Azure Repos)].

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Autorizar Azure App Service 

Para usar Azure Repos, asegúrese de que su organización de Azure DevOps esté vinculada a su suscripción de Azure. Para obtener más información, consulte [Configurar una cuenta de Azure DevOps Services para que se pueda implementar en una aplicación web](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops).

En cuanto a Bitbucket o GitHub, autorice a Azure App Service a conectarse a su repositorio. Solo necesita realizar la autorización con un servicio de control de código fuente una vez. 

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **App Services**. 

   ![Busque App Services.](media/app-service-continuous-deployment/search-for-app-services.png)

1. Seleccione la aplicación web que quiere implementar.

   ![Seleccione la aplicación.](media/app-service-continuous-deployment/select-your-app.png)
   
1. En la página de la aplicación, seleccione **Deployment Center** (Centro de implementación) en el menú de la izquierda.
   
1. En la página del **centro de implementación**, seleccione **GitHub** o **Bitbucket** y, a continuación, seleccione **Authorize** (Autorizar). 
   
   ![Seleccione el servicio de control de código fuente y, a continuación, Autorizar.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Inicie sesión en el servicio si es necesario y siga las indicaciones de autorización. 

## <a name="enable-continuous-deployment"></a>Habilitación de la implementación continua 

Después de autorizar un servicio de control de código fuente, configure su aplicación para la implementación continua a través del [servidor de compilación de App Service para Kudu ](#option-1-use-the-app-service-build-service) integrado o mediante [Azure Pipelines](#option-2-use-azure-pipelines). 

### <a name="option-1-use-the-app-service-build-service"></a>Opción 1: use el servicio de compilación de App Service

Puede usar el servidor de compilación de App Service para Kudu integrado para realizar implementaciones continuas desde GitHub, Bitbucket o Azure Repos. 

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **App Services** y luego elija la aplicación web que quiere implementar. 
   
1. En la página de la aplicación, seleccione **Deployment Center** (Centro de implementación) en el menú de la izquierda.
   
1. Seleccione su proveedor de control de código fuente autorizado en la página del **centro de implementación** y seleccione **Continue** (Continuar). En cuanto a GitHub o Bitbucket, también puede seleccionar **Change account** (Cambiar cuenta) para cambiar la cuenta autorizada. 
   
   > [!NOTE]
   > Para usar Azure Repos, asegúrese de que su organización de Azure DevOps Services esté vinculada a su suscripción de Azure. Para obtener más información, consulte [Configurar una cuenta de Azure DevOps Services para que se pueda implementar en una aplicación web](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops).
   
1. En cuanto a GitHub o Azure Repos, en la página para **compilar el proveedor**, seleccione **App Service build service** (Servicio de compilación de App Service) y, a continuación, **Continue** (Continuar). Bitbucket siempre usa el servicio de compilación de App Service.
   
   ![Seleccione el servicio de compilación de App Service y, a continuación, seleccione Continuar.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. En la página de **configuración**:
   
   - Para GitHub, despliegue el menú y seleccione la **organización**, el **repositorio** y la **rama** que quiere implementar continuamente.
     
     > [!NOTE]
     > Si no ve ningún repositorio, es posible que deba autorizar Azure App Service en GitHub. Vaya a su repositorio de GitHub y a **Settings (Configuración)**  > **Applications (Aplicaciones)**  > **Authorized OAuth Apps** (Aplicaciones OAuth autorizadas). Seleccione **Azure App Service** y **Grant** (Conceder). En el caso de los repositorios de la organización, debe ser propietario de la organización para conceder los permisos.
     
   - Para Bitbucket, seleccione el **equipo** , el **repositorio**  y la **rama**  de Bitbucket que quiere implementar continuamente.
     
   - Para Azure Repos, seleccione la **organización de Azure DevOps** , el **proyecto**, el **repositorio** y la **rama** que quiere implementar continuamente.
     
     > [!NOTE]
     > Si su organización de Azure DevOps no figura en la lista, asegúrese de que esté vinculada a su suscripción de Azure. Para obtener más información, consulte [Configurar una cuenta de Azure DevOps Services para que se pueda implementar en una aplicación web](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps?view=azure-devops).
     
1. Seleccione **Continuar**.
   
   ![Complete la información del repositorio y seleccione Continuar.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Después de configurar el proveedor de compilación, revise la configuración en la página de **resumen** y seleccione **Finish** (Finalizar).
   
   Las nuevas confirmaciones del repositorio y la rama seleccionados ahora se implementan continuamente en su aplicación de App Service. Puede realizar el seguimiento de las confirmaciones y las implementaciones en la página del **centro de implementación**.
   
   ![Seguimiento de confirmaciones e implementaciones en el centro de implementación](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-use-azure-pipelines"></a>Opción 2: Use Azure Pipelines 

Si su cuenta tiene los permisos necesarios, puede configurar Azure Pipelines para que se implemente continuamente desde los repositorios de GitHub o Azure Repos. Para obtener más información sobre la implementación a través de Azure Pipelines, consulte [Implementación de una aplicación web en Azure App Services](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

Para que Azure App Service cree la entrega continua de Azure Pipelines en su organización de Azure DevOps: 

- Su cuenta de Azure debe tener permisos para escribir en Azure Active Directory y crear un servicio. 
  
- Su cuenta de Azure debe tener el rol **Propietario** en su suscripción de Azure.

- Debe ser administrador en el proyecto de Azure DevOps que quiera usar.

Para configurar Azure Pipelines (versión preliminar):

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **App Services** y luego elija la aplicación web que quiere implementar. 
   
1. En la página de la aplicación, seleccione **Deployment Center** (Centro de implementación) en el menú de la izquierda.
   
1. En la página **Build provider** (Compilar proveedor), seleccione **Azure Pipelines (Preview)** (Azure Pipelines [versión preliminar]) y **Continue** (Continuar). 
   
1. En la página de **configuración** , en la sección **Code** (Código):
   
   - Para GitHub, despliegue el menú y seleccione la **organización**, el **repositorio** y la **rama** que quiere implementar continuamente.
     
     > [!NOTE]
     > Si no ve ningún repositorio, es posible que deba autorizar Azure App Service en GitHub. Vaya a su repositorio de GitHub y a **Settings (Configuración)**  > **Applications (Aplicaciones)**  > **Authorized OAuth Apps** (Aplicaciones OAuth autorizadas). Seleccione **Azure App Service** y **Grant** (Conceder). En el caso de los repositorios de la organización, debe ser propietario de la organización para conceder los permisos.
     
   - Para Azure Repos, seleccione la **organización de Azure DevOps** , el **proyecto**, el **repositorio** y la **rama** que quiere implementar continuamente o configure una organización de Azure DevOps nueva.
     
     > [!NOTE]
     > Si su organización Azure DevOps existente no figura en la lista, es posible que deba vincularla a su suscripción de Azure. Para obtener más información, consulte [Define your CD release pipeline](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd) (Definir la canalización de versión de CD).
     
1. Seleccione **Continuar**.
   
1. Para Azure Repos, en la sección de **compilación**, especifique el marco de lenguaje que Azure Pipelines debe usar para ejecutar tareas de compilación y luego seleccione **Continuar**.
   
1. En la página **Test** (Probar), decida si quiere habilitar las pruebas de carga y luego seleccione **Continue** (Continuar).
   
1. Dependiendo de su [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/plans/) de App Service, puede ver una página **Deploy to staging** (Implementar en el almacenamiento provisional). Dcida si quiere [habilitar las ranuras de implementación](deploy-staging-slots.md) y seleccione **Continue** (Continuar).
   
   > [!NOTE]
   > Azure Pipelines no permite la entrega continua al espacio de producción. Esta restricción evita implementaciones accidentales en la producción. Configure la entrega continua en un espacio de ensayo, compruebe los cambios del mismo e intercambie los espacios cuando esté listo.
   
1. Después de configurar el proveedor de compilación, revise la configuración en la página de **resumen** y seleccione **Finish** (Finalizar).
   
   Las nuevas confirmaciones del repositorio y la rama seleccionados ahora se implementan continuamente en su aplicación de App Service. Puede realizar el seguimiento de las confirmaciones y las implementaciones en la página del **centro de implementación**.
   
   ![Seguimiento de confirmaciones e implementaciones en el centro de implementación](media/app-service-continuous-deployment/github-finished.png)

## <a name="disable-continuous-deployment"></a>Deshabilitación de la implementación continua

Para deshabilitar la implementación continua, seleccione **Disconnect** (Desconectar) en la parte superior de la página **Deployment Center** (Centro de implementación) de la aplicación.

![Deshabilitación de la implementación continua](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="use-unsupported-repos"></a>Uso de repositorios no admitidos

En el caso de las aplicaciones de Windows, puede configurar manualmente la implementación continua desde un repositorio GIT o de Mercurial en la nube que el portal no admite directamente, como [GitLab](https://gitlab.com/). Para ello, elija el cuadro Externo en la página **Centro de implementación**. Para obtener más información, consulte [Configuración de la implementación continua mediante pasos manuales](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="additional-resources"></a>Recursos adicionales

* [Investigar los problemas habituales con la implementación continua](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Uso de Azure PowerShell](/powershell/azureps-cmdlets-docs)
* [Documentación de Git](https://git-scm.com/documentation)
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)

[Crear un repositorio (GitHub)]: https://help.github.com/articles/create-a-repo
[Crear un repositorio (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Crear un nuevo repositorio de Git (Azure Repos)]: /azure/devops/repos/git/creatingrepo
