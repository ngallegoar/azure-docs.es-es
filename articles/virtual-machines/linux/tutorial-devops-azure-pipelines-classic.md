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
ms.openlocfilehash: 4159bf27c39087926d982552c49c606f3484de77
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885912"
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
Azure Pipelines ofrece un conjunto completo de herramientas de automatización de CI/CD para implementaciones en máquinas virtuales. Desde Azure Portal puede configurar directamente una canalización de entrega continua para una máquina virtual de Azure. Este documento contiene los pasos de configuración de una canalización de CI/CD para la implementación en varias máquinas desde Azure Portal. 


**Configuración de CI/CD en máquinas virtuales**

Se pueden agregar máquinas virtuales se pueden agregar como destino en un [grupo de implementación](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) y pueden ser el destino de una actualización en varias máquinas. En función de sus requisitos, puede elegir cualquiera de las estrategias de implementación integradas, _Rolling_ (Gradual), _Canary_ (Valor controlado) o _Blue-Green_ (Azul-verde), o bien puede personalizarlas. Tras la implementación, las vistas del historial de implementación de los grupos de implementación permiten realizar un seguimiento desde la máquina virtual a la canalización y, posteriormente, a la confirmación. 
 
**Implementaciones graduales**: una implementación gradual reemplaza las instancias de la versión anterior de una aplicación por instancias de la nueva versión en un conjunto fijo de máquinas (conjunto de implementación gradual) en cada iteración. Veamos cómo puede configurar una actualización gradual en las máquinas virtuales.  
Con la opción de entrega continua puede configurar las actualizaciones graduales en sus "**máquinas virtuales**" de Azure Portal. 

Este es el tutorial paso a paso. 
1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. En el panel de la izquierda de la máquina virtual, vaya al menú **Entrega continua** y haga clic en **Configurar**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. En el panel de configuración, haga clic en "Organización de Azure DevOps" para seleccionar una cuenta existente o crear una. A continuación, seleccione el proyecto en el que desea configurar la canalización.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Un grupo de implementación es un conjunto lógico de máquinas de destino de implementación que representan los entornos físicos; por ejemplo, "Dev", "Test", "UAT" y "Production". Puede crear un grupo de implementación o seleccionar uno existente. 
5. Seleccione la canalización de compilación que publica el paquete que se va a implementar en la máquina virtual. Tenga en cuenta que el paquete publicado debe tener un script de implementación _deploy.ps1_ o _deploy.sh_ en la carpeta _deployscripts_ de la raíz del paquete. La canalización de Azure DevOps ejecutará este script en tiempo de ejecución.
6. Seleccione la estrategia de implementación que prefiera. En este caso, seleccione "Rolling" (Gradual).
7. Opcionalmente, puede etiquetar la máquina con el rol. Por ejemplo, "web", "db", etc. Esto le ayuda a dirigirse solo a las máquinas virtuales que tienen un rol concreto.
8. Haga clic en **Aceptar** en el cuadro de diálogo para configurar la canalización de entrega continua. 
9. Una vez hecho esto, tendrá una canalización de entrega continua configurada para implementarla en la máquina virtual.  


   ![AzDevOps_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-deployment-history.png)
10. Verá que la implementación en la máquina virtual está en curso. Puede hacer clic en el vínculo para ir a la canalización. Haga clic en **Release-1** para ver la implementación. También puede hacer clic en **Editar** para modificar la definición de la canalización de versión. 
11. Si tiene varias máquinas virtuales para configurar, repita los pasos 2 a 4 para las máquinas que va a agregar al grupo de implementación. Tenga en cuenta que si selecciona un grupo de implementación para el que ya existe una ejecución de canalización, la máquina virtual se agregará al grupo sin crear canalizaciones. 
12. Una vez hecho esto, haga clic en la definición de la canalización, vaya a Organización de Azure DevOps y haga clic en **Editar** para editar la canalización de versión. 
   ![AzDevOps_edit_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline.png)
13. Haga clic en el vínculo de **1 trabajo, 1 tarea** en la fase **dev**. Haga clic en la fase **Implementar**.
   ![AzDevOps_deploymentGroup](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling-pipeline-tasks.png)
14. En el panel de configuración de la derecha, puede especificar el número de máquinas que desea implementar en paralelo en cada iteración. Si quiere realizar la implementación en varias máquinas a la vez, puede especificarlo de forma porcentual mediante el control deslizante.  

15. La tarea Execute Deploy Script (Ejecutar script de implementación) ejecutará de forma predeterminada los scripts de implementación _deploy.ps1_ o _deploy.sh_ de la carpeta _deployscripts_ del directorio raíz del paquete publicado.
  
**Implementaciones de valor controlado**: las implementaciones de valor controlado reducen el riesgo, ya que implementan el cambio gradualmente en un pequeño subconjunto de usuarios. A medida que vaya ganando confianza con la nueva versión, puede empezar a usarla en más servidores de la infraestructura y enrutar más usuarios a ella. Puede configurar implementaciones controladas en sus "**máquinas virtuales**" con la opción de entrega continua de Azure Portal. Este es el tutorial paso a paso. 
1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. Siga los pasos 2-7 de la sección **Implementaciones graduales** para agregar varias máquinas virtuales al grupo de implementación. En el menú desplegable de la estrategia de implementación, seleccione "Canary" (Valor controlado).
![AzDevOps_configure_canary](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure-canary.png)

3. Agregue la etiqueta "canary" a las máquinas virtuales que forman parte de las implementaciones de valor controlado y la etiqueta "prod" a las que lo hacen después de que la implementación de valor controlado se haya realizado correctamente.
4. Una vez hecho esto, tendrá una canalización de entrega continua configurada para implementarla en la máquina virtual.
![AzDevOps_canary_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-pipeline.png)


5. De igual forma que en la sección **Implementaciones graduales**, puede hacer clic en **Editar** canalización de versión en Azure DevOps para ver la configuración de la canalización. La canalización consta de tres fases: la primera es una fase de DG y se implementa en aquellas máquinas virtuales que tengan la etiqueta _canary_. En la segunda fase, la canalización se detiene y espera la intervención manual para reanudar la ejecución. Una vez que un usuario se ha asegurado de que la implementación de valor controlado es estable, puede reanudar la ejecución de la canalización y, seguidamente, se ejecutará la tercera fase, en la que se implementa en las máquinas virtuales que tengan la etiqueta _prod_. ![AzDevOps_canary_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-task.png)



**Implementaciones azul-verde**: Las implementaciones azul-verde reducen el tiempo de inactividad, ya que tienen un entorno en espera idéntico. Siempre hay uno de los entornos activo. Mientras se prepara para una nueva versión, la fase final de las pruebas se realiza en el entorno Green. Una vez que el software esté funcionando en el entorno Green, cambie el tráfico para que todas las solicitudes entrantes vayan a este (el entorno Blue estará inactivo).
Puede configurar implementaciones Blue-Green en sus "**máquinas virtuales**" con la opción de entrega continua de Azure Portal. 

Este es el tutorial paso a paso.

1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. Siga los pasos 2-7 de la sección **Implementaciones graduales** para agregar varias máquinas virtuales al grupo de implementación. Agregue una etiqueta "blue" (azul) o "green" (verde) a las máquinas virtuales que van a formar parte de las implementaciones azul-verde. Si la máquina virtual es para un rol en espera, se debe etiquetar como "green" (verde).
![AzDevOps_bluegreen_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-configure.png)

4. Una vez hecho esto, tendrá una canalización de entrega continua configurada para implementarla en la máquina virtual.
![AzDevOps_bluegreen_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-pipeline.png)

5. De igual forma que con las **implementaciones graduales**, puede hacer clic en **Edit** (Editar) la canalización de versión en Azure DevOps para ver la configuración de la canalización. La canalización consta de tres fases: la primera es una fase de DG y se implementa en aquellas máquinas virtuales que tengan la etiqueta _green_ (máquinas virtuales en espera). En la segunda fase, la canalización se detiene y espera la intervención manual para reanudar la ejecución. Una vez que un usuario se ha asegurado de que la implementación es estable, puede redirigir el tráfico a las máquinas virtuales con la etiqueta _green_ y reanudar la ejecución de la canalización, que, a partir de ese momento, cambiará las etiquetas _blue_ (azul) y _green_ (verde) en las máquinas virtuales. De esta forma se asegura de que las máquinas virtuales que tengan una versión anterior de la aplicación se etiqueten como _green_ (verde) y se implementen en la siguiente ejecución de la canalización.
![AzDevOps_bluegreen_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-tasks.png)

6. Tenga en cuenta que esta estrategia de implementación requiere que haya al menos una máquina virtual etiquetada como azul y otra como verde. Asegúrese de que antes de reanudar la ejecución de la canalización en el paso de intervención manual, tiene al menos una máquina virtual con la etiqueta _blue_ (azul).





 
## <a name="azure-devops-project"></a>Proyecto de Azure DevOps 
Empezar a usar Azure nunca fue tan fácil.
 
Con DevOps Projects, empiece a ejecutar la aplicación en cualquier servicio de Azure en tan solo tres pasos: seleccione un idioma para la aplicación, un entorno de ejecución y un servicio de Azure.
 
[Más información](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Recursos adicionales 
- [Implementación en máquinas virtuales de Azure con un proyecto de DevOps](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementación continua de una aplicación en un conjunto de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
