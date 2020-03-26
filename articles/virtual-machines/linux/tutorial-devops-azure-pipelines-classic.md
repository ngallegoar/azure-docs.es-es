---
title: 'Tutorial: DevOps integrado para IaaS y PaaS en Azure'
description: En este tutorial aprenderá a configurar la integración continua y la implementación continua de una aplicación en máquinas virtuales de Azure mediante Azure Pipelines.
author: ushan
manager: jpconnock
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 1/16/2020
ms.author: ushan
ms.custom: devops
ms.openlocfilehash: 5707a99b329915b35131fe793b0dfabd02348677
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77912535"
---
# <a name="tutorial-integrated-devops-for-iaas-and-paas-on-azure"></a>Tutorial: DevOps integrado para IaaS y PaaS en Azure

Con las soluciones globales de Azure, los equipos pueden implementar prácticas de DevOps en cada una de las fases del ciclo de vida de la aplicación: planeamiento, desarrollo, entrega y funcionamiento. 

A continuación se muestran algunos de los servicios de Azure que simplifican las cargas de trabajo en la nube y que se pueden combinar para habilitar escenarios increíblemente eficaces.
La combinación de procesos con estas tecnologías permite a los equipos de profesionales ofrecer propuestas de valor a los clientes continuamente. 

- Azure: https://portal.azure.com Portal para crear cargas de trabajo en la nube. Administre y supervise todo, desde aplicaciones web sencillas hasta complejas aplicaciones en la nube 
- Azure DevOps: https://dev.azure.com Planee de manera más inteligente, colabore mejor y realice envíos más rápidos con un conjunto de servicios de desarrollo modernos 
- Azure Machine Learning Studio: https://ml.azure.com Prepare datos y entrene e implemente modelos de Machine Learning 
 

Azure DevOps es un servicio integrado de Azure que automatiza cada parte del proceso de DevOps con integración y entrega continuas para cualquier recurso de Azure.
Independientemente de que su aplicación use máquinas virtuales, aplicaciones web, Kubernetes o cualquier otro recurso, podrá utilizar Azure y Azure DevOps para la implementación de infraestructura como código, y la integración, pruebas, entrega y supervisión continuas.  
![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 
 
 
## <a name="iaas---configure-cicd"></a>IaaS: configuración de CI/CD 
Azure Pipelines ofrece un conjunto completo de herramientas de automatización de CI/CD para implementaciones en máquinas virtuales. Desde Azure Portal puede configurar directamente una canalización de entrega continua para una máquina virtual de Azure. Este documento contiene los pasos de configuración de una canalización de CI/CD para la implementación en varias máquinas desde Azure Portal. Configure CI/CD en las máquinas virtuales.

Las máquinas virtuales se pueden agregar como destino en un [grupo de implementación](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) para las actualizaciones graduales en varias máquinas. Las vistas del historial de implementación de los grupos de implementación permiten realizar un seguimiento desde la máquina virtual a la canalización y, posteriormente, a la confirmación. 
 
**Actualizaciones graduales**: una implementación gradual reemplaza las instancias de la versión anterior de una aplicación por instancias de la nueva versión en un conjunto fijo de máquinas (conjunto de implementación gradual) en cada iteración. Veamos cómo puede configurar una actualización gradual en las máquinas virtuales.  
Con la opción de entrega continua puede configurar las actualizaciones graduales en sus "**máquinas virtuales**" de Azure Portal. 

Este es el tutorial paso a paso. 
1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. En el panel de la izquierda de la máquina virtual, vaya al menú **Entrega continua** y haga clic en **Configurar**. 
   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azdevops-configure.png) 
3. En el panel de configuración, haga clic en "Organización de Azure DevOps" para seleccionar una cuenta existente o crear una. A continuación, seleccione el proyecto en el que desea configurar la canalización.  
   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azdevops-project.png) 
4. Un grupo de implementación es un conjunto lógico de máquinas de destino de implementación que representan los entornos físicos; por ejemplo, "Dev", "Test", "UAT" y "Production". Puede crear un grupo de implementación o seleccionar uno existente. Opcionalmente, puede etiquetar la máquina con el rol. Por ejemplo, "web", "db", etc.  
5. Haga clic en **Aceptar** en el cuadro de diálogo para configurar la canalización de entrega continua. 
6. Una vez hecho esto, tendrá una canalización de entrega continua configurada para implementarla en la máquina virtual.  
   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-pipeline.png)
7. Verá que la implementación en la máquina virtual está en curso. Puede hacer clic en el vínculo para ir a la canalización. Haga clic en **Release-1** para ver la implementación. También puede hacer clic en **Editar** para modificar la definición de la canalización de versión. 
8. Si va a configurar varias máquinas virtuales, repita los pasos 2-5 para agregarlas al grupo de implementación. 
9. Una vez hecho esto, haga clic en la definición de la canalización, vaya a Organización de Azure DevOps y haga clic en **Editar** para editar la canalización de versión. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azdevops-edit-pipeline.png)
10. Haga clic en el vínculo de **1 trabajo, 1 tarea** en la fase **dev**. Haga clic en la fase **Implementar**.  
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azdevops-deployment-group.png)
11. En el panel de configuración de la derecha, puede ver que, de forma predeterminada, la canalización está configurada para realizar una actualización gradual en todos los destinos en paralelo. Puede configurar las implementaciones para que se produzcan de una en una o en términos de porcentaje mediante el control deslizante.  
  
  
La **implementación controlada** reduce el riesgo al implementar lentamente el cambio en un pequeño subconjunto de usuarios. A medida que vaya ganando confianza con la nueva versión, puede empezar a usarla en más servidores de la infraestructura y enrutar más usuarios a ella. Puede configurar implementaciones controladas en sus "**máquinas virtuales**" con la opción de entrega continua de Azure Portal. Este es el tutorial paso a paso. 
1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. Siga los pasos 2-5 de la sección anterior para agregar varias máquinas virtuales al grupo de implementación. 
3. Agregue una etiqueta personalizada a las máquinas virtuales que van a formar parte de las implementaciones controladas; por ejemplo, "controlada".
4. Una vez configurada la canalización para las máquinas virtuales, haga clic en ella, inicie Organización de Azure DevOps, haga clic en **Editar** para editar la canalización y vaya a la fase de **desarrollo**. Agregue la etiqueta al filtro "controlada". 
5. Agregue otra fase al grupo de implementación y configúrela con las etiquetas para las máquinas virtuales de destino que quedan en el grupo de implementación.  
6. Opcionalmente, configure un paso de validación manual para promover o rechazar las implementaciones controladas. 
   ![AzDevOps_Canary](media/tutorial-devops-azure-pipelines-classic/azdevops-canary-deploy.png)

**Blue-Green** reduce el tiempo de inactividad de la implementación al disponer de un entorno de espera idéntico. Siempre hay uno de los entornos activo. Mientras se prepara para una nueva versión, la fase final de las pruebas se realiza en el entorno Green. Una vez que el software esté funcionando en el entorno Green, cambie el tráfico para que todas las solicitudes entrantes vayan a este (el entorno Blue estará inactivo).
Puede configurar implementaciones Blue-Green en sus "**máquinas virtuales**" con la opción de entrega continua de Azure Portal. 

Este es el tutorial paso a paso. 

1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. Siga los pasos 2-5 de la sección **Actualizaciones graduales** para agregar varias máquinas virtuales al grupo de implementación. Agregue una etiqueta personalizada a las máquinas virtuales que van a formar parte de las implementaciones Blue-Green; por ejemplo, "Blue" o "Green" para las máquinas virtuales que son para el rol de espera. 
3. Una vez configurada la canalización para las máquinas virtuales, haga clic en ella, inicie Organización de Azure DevOps, haga clic en **Editar** para editar la canalización, vaya a la fase de **desarrollo** y agregue la etiqueta al filtro "Green". 
4. Agregue una fase sin agente, configúrela con el paso de validación manual y un paso de la API Invoke-REST para intercambiar las etiquetas. 
   ![AzDevOps_BlueGreen](media/tutorial-devops-azure-pipelines-classic/azdevops-blue-green-deploy.png)
 
 
## <a name="azure-devops-project"></a>Proyecto de Azure DevOps 
Empezar a usar Azure nunca fue tan fácil.
 
Con DevOps Projects, empiece a ejecutar la aplicación en cualquier servicio de Azure en tan solo tres pasos: seleccione un idioma para la aplicación, un entorno de ejecución y un servicio de Azure.
 
[Más información](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Recursos adicionales 
- [Implementación en máquinas virtuales de Azure con un proyecto de DevOps](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementación continua de una aplicación en un conjunto de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
