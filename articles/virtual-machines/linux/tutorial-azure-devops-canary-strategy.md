---
title: 'Tutorial: Configuración de implementaciones de valor controlado para Azure Linux Virtual Machines'
description: En este tutorial, aprenderá a configurar una canalización de implementación continua (CD) que actualiza un grupo de máquinas virtuales Linux de Azure mediante la estrategia de implementación de valor controlado.
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
ms.openlocfilehash: b51b4aed85f737e436565ce8ba1ab4a295714734
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82120533"
---
# <a name="tutorial---configure-canary-deployment-strategy-for-azure-linux-virtual-machines"></a>Tutorial: Configuración de la estrategia de implementación de valor controlado para Azure Linux Virtual Machines


## <a name="iaas---configure-cicd"></a>IaaS: configuración de CI/CD 
Azure Pipelines ofrece un conjunto completo de herramientas de automatización de CI/CD para implementaciones en máquinas virtuales. Desde Azure Portal puede configurar directamente una canalización de entrega continua para una máquina virtual de Azure. Este documento contiene los pasos asociados a la configuración de una canalización de CI/CD que emplea la estrategia de valor controlado para implementaciones de varias máquinas. También puede echar un vistazo a otras estrategias, como [Rolling](https://aka.ms/AA7jlh8) (Gradual) y [Blue-Green](https://aka.ms/AA83fwu) (Azul-verde), que se admiten de forma predeterminada en Azure Portal. 


**Configuración de CI/CD en máquinas virtuales**

Se pueden agregar máquinas virtuales como destino en un [grupo de implementación](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) y se pueden destinar para actualizaciones de varias máquinas. Tras la implementación, el **historial de implementación** del grupo de implementación permite realizar un seguimiento desde la máquina virtual hasta la canalización y, posteriormente, hasta la confirmación. 
 
  
**Implementaciones de valor controlado**: las implementaciones de valor controlado reducen el riesgo, ya que implementan el cambio gradualmente en un pequeño subconjunto de usuarios. A medida que vaya ganando confianza con la nueva versión, puede empezar a usarla en más servidores de la infraestructura y enrutar más usuarios a ella. Puede configurar implementaciones de valor controlado en las máquinas virtuales con la opción de entrega continua de Azure Portal. Este es el tutorial paso a paso. 
1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. En el panel de la izquierda de la máquina virtual, vaya al menú  **Entrega continua** . Haga clic en **Configurar**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. En el panel de configuración, haga clic en **Organización de Azure DevOps** para seleccionar una cuenta existente o crear una. A continuación, seleccione el proyecto en el que desea configurar la canalización.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Un grupo de implementación es un conjunto lógico de máquinas de destino de implementación que representan los entornos físicos; por ejemplo, "Dev", "Test", "UAT" y "Production". Puede crear un grupo de implementación o seleccionar uno existente. 
5. Seleccione la canalización de compilación que publica el paquete que se va a implementar en la máquina virtual. Tenga en cuenta que el paquete publicado debe tener un script de implementación _deploy.ps1_ o _deploy.sh_ en la carpeta `deployscripts` de la raíz del paquete. La canalización de Azure DevOps ejecutará este script de implementación en tiempo de ejecución.
6. Seleccione la estrategia de implementación que prefiera. Seleccione **Canary** (Valor controlado).
7. Agregue la etiqueta "canary" a las máquinas virtuales que forman parte de las implementaciones de valor controlado y la etiqueta "prod" a las que lo hacen después de que la implementación de valor controlado se haya realizado correctamente. Las etiquetas le ayudan a dirigirse solo a las máquinas virtuales que tienen un rol concreto.
![AzDevOps_configure_canary](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure-canary.png)

8. Haga clic en **Aceptar** en el cuadro de diálogo para configurar la canalización de entrega continua. Ahora, tendrá una canalización de entrega continua configurada para implementarla en la máquina virtual.
![AzDevOps_canary_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-pipeline.png)


9. Haga clic en **Editar** la canalización de versión en Azure DevOps para ver la configuración de la canalización. La canalización consta de tres fases. La primera es una fase de grupo de implementación y se implementa en las máquinas virtuales que tengan la etiqueta _canary_ (valor controlado). En la segunda fase, la canalización se detiene y espera la intervención manual para reanudar la ejecución. Una vez que un usuario está seguro de que la implementación de valor controlado es estable, puede reanudar la ejecución de la canalización y, seguidamente, se ejecutará la tercera fase, en la que se implementa en las máquinas virtuales con la etiqueta _prod_ (producción). ![AzDevOps_canary_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-canary-task.png)

10. Antes de reanudar la ejecución de la canalización, asegúrese de que al menos una máquina virtual esté etiquetada como _prod_ (producción). En la tercera fase de la canalización, la aplicación se implementará solo en las máquinas virtuales que tengan la etiqueta _prod_ (producción).

11. La tarea para ejecutar el script de implementación ejecutará de forma predeterminada el script de implementación _deploy.ps1_ o _deploy.sh_ de la carpeta "deployscripts" del directorio raíz del paquete publicado. Asegúrese de que la canalización de compilación seleccionada lo publica en la carpeta raíz del paquete. 
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)




## <a name="other-deployment-strategies"></a>Otras estrategias de implementación
- [Configuración de la estrategia de implementación gradual](https://aka.ms/AA7jlh8)
- [Configuración de la estrategia de implementación azul-verde ](https://aka.ms/AA83fwu)

## <a name="azure-devops-project"></a>Proyecto de Azure DevOps 
Empezar a usar Azure nunca fue tan fácil.
 
Con DevOps Projects, empiece a ejecutar la aplicación en cualquier servicio de Azure en tan solo tres pasos: seleccione un idioma para la aplicación, un entorno de ejecución y un servicio de Azure.
 
[Más información](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Recursos adicionales 
- [Implementación en máquinas virtuales de Azure con un proyecto de DevOps](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementación continua de una aplicación en un conjunto de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
