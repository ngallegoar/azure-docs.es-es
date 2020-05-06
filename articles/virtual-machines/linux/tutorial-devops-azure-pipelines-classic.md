---
title: 'Tutorial: Configuración de implementaciones graduales para Azure Linux Virtual Machines'
description: En este tutorial, aprenderá a configurar una canalización de implementación continua (CD) que actualiza incrementalmente un grupo de instancias de Azure Linux Virtual Machines mediante la estrategia de implementación gradual.
author: moala
manager: jpconnock
tags: azure-devops-pipelines
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: azure-pipelines
ms.workload: infrastructure
ms.date: 4/10/2020
ms.author: moala
ms.custom: devops
ms.openlocfilehash: 75888b1ebbda33891296fe0b54c5d204955e32a3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82113497"
---
# <a name="tutorial---configure-rolling-deployment-strategy-for-azure-linux-virtual-machines"></a>Tutorial: Configuración de una estrategia de implementación gradual para Azure Linux Virtual Machines

Azure DevOps es un servicio integrado de Azure que automatiza cada parte del proceso de DevOps con integración y entrega continuas para cualquier recurso de Azure.
Con independencia de que su aplicación use máquinas virtuales, aplicaciones web, Kubernetes o cualquier otro recurso, podrá implementar infraestructura como código e integración, pruebas, entrega y supervisión continuas con Azure y Azure DevOps.  

![AzDevOps_portalView](media/tutorial-devops-azure-pipelines-classic/azdevops-view.png) 


## <a name="iaas---configure-cicd"></a>IaaS: configuración de CI/CD 
Azure Pipelines ofrece un conjunto completo de herramientas de automatización de CI/CD para implementaciones en máquinas virtuales. Desde Azure Portal puede configurar directamente una canalización de entrega continua para una máquina virtual de Azure. Este documento contiene los pasos asociados a la configuración de una canalización de CI/CD para implementaciones graduales en varias máquinas desde Azure Portal. También puede echar un vistazo a otras estrategias, como [Canary](https://aka.ms/AA7jdrz) (Valor controlado) y [Blue-Green](https://aka.ms/AA83fwu) (Azul-verde), que se admiten de forma predeterminada en Azure Portal. 


**Configuración de CI/CD en máquinas virtuales**

Se pueden agregar máquinas virtuales se pueden agregar como destino en un [grupo de implementación](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) y pueden ser el destino de una actualización en varias máquinas. Tras la implementación, el **historial de implementación** del grupo de implementación permite realizar un seguimiento desde la máquina virtual hasta la canalización y, posteriormente, hasta la confirmación. 
 

**Implementaciones graduales**: una implementación gradual reemplaza las instancias de la versión anterior de una aplicación por instancias de la nueva versión en un conjunto fijo de máquinas (conjunto de implementación gradual) en cada iteración. Veamos cómo puede configurar una actualización gradual en las máquinas virtuales.  
Con la opción de entrega continua puede configurar las actualizaciones graduales en sus "**máquinas virtuales**" de Azure Portal. 

Este es el tutorial paso a paso. 
1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. En el panel de la izquierda de la máquina virtual, vaya al menú **Entrega continua** y haga clic en **Configurar**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. En el panel de configuración, haga clic en "Organización de Azure DevOps" para seleccionar una cuenta existente o crear una. A continuación, seleccione el proyecto en el que desea configurar la canalización.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Un grupo de implementación es un conjunto lógico de máquinas de destino de implementación que representan los entornos físicos; por ejemplo, "Dev", "Test", "UAT" y "Production". Puede crear un grupo de implementación o seleccionar uno existente. 
5. Seleccione la canalización de compilación que publica el paquete que se va a implementar en la máquina virtual. Tenga en cuenta que el paquete publicado debe tener un script de implementación _deploy.ps1_ o _deploy.sh_ en la carpeta `deployscripts` de la raíz del paquete. La canalización de Azure DevOps ejecutará este script de implementación en tiempo de ejecución.
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

15. La tarea para ejecutar el script de implementación ejecutará de forma predeterminada el script de implementación _deploy.ps1_ o _deploy.sh_ de la carpeta "deployscripts" del directorio raíz del paquete publicado.  
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)

## <a name="other-deployment-strategies"></a>Otras estrategias de implementación

- [Configuración de la estrategia de implementación de valor controlado](https://aka.ms/AA7jdrz)
- [Configuración de la estrategia de implementación azul-verde](https://aka.ms/AA83fwu)

 
## <a name="azure-devops-project"></a>Proyecto de Azure DevOps 
Empezar a usar Azure nunca fue tan fácil.
 
Con DevOps Projects, empiece a ejecutar la aplicación en cualquier servicio de Azure en tan solo tres pasos: seleccione un idioma para la aplicación, un entorno de ejecución y un servicio de Azure.
 
[Más información](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Recursos adicionales 
- [Implementación en máquinas virtuales de Azure con un proyecto de DevOps](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementación continua de una aplicación en un conjunto de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
