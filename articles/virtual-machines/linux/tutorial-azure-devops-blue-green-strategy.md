---
title: 'Tutorial: Configuración de implementaciones de valor controlado para Azure Linux Virtual Machines'
description: En este tutorial, aprenderá a configurar una canalización de implementación continua (CD) que actualiza un grupo de máquinas virtuales de Azure mediante la estrategia de implementación azul-verde.
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
ms.openlocfilehash: b1a57245434bb188ffaab56a8891b4b0ee27f044
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82120553"
---
# <a name="tutorial---configure-blue-green-deployment-strategy-for-azure-linux-virtual-machines"></a>Tutorial: Configuración de la estrategia de implementación azul-verde para Azure Linux Virtual Machines


## <a name="iaas---configure-cicd"></a>IaaS: configuración de CI/CD 
Azure Pipelines ofrece un conjunto completo de herramientas de automatización de CI/CD para implementaciones en máquinas virtuales. Desde Azure Portal puede configurar directamente una canalización de entrega continua para una máquina virtual de Azure. Este documento contiene los pasos asociados a la configuración de una canalización de CI/CD que emplea la estrategia azul-verde para implementaciones de varias máquinas. También puede echar un vistazo a otras estrategias, como [Rolling](https://aka.ms/AA7jlh8) (Gradual) y [Canary](https://aka.ms/AA7jdrz) (Valor controlado), que se admiten de forma predeterminada en Azure Portal. 

 
 **Configuración de CI/CD en máquinas virtuales**

Se pueden agregar máquinas virtuales como destino en un [grupo de implementación](https://docs.microsoft.com/azure/devops/pipelines/release/deployment-groups) y se pueden destinar para actualizaciones de varias máquinas. Tras la implementación, la vista del **historial de implementación** del grupo de implementación permiten realizar un seguimiento desde la máquina virtual hasta la canalización y, posteriormente, hasta la confirmación. 
 
  
**Implementaciones azul-verde**: Las implementaciones azul-verde reducen el tiempo de inactividad, ya que tienen un entorno en espera idéntico. Siempre hay uno de los entornos activo. Mientras se prepara para una nueva versión, puede realizar la fase final de las pruebas en el entorno verde. Una vez que el software esté funcionando en el entorno Green, cambie el tráfico para que todas las solicitudes entrantes vayan a este (el entorno Blue estará inactivo).
Puede configurar implementaciones Blue-Green en sus "**máquinas virtuales**" con la opción de entrega continua de Azure Portal. 

Este es el tutorial paso a paso.

1. Inicie sesión en Azure Portal y vaya a una máquina virtual. 
2. En el panel de la izquierda de la máquina virtual, vaya **Entrega continua**. A continuación, haga clic en **Configurar**. 

   ![AzDevOps_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-configure.png) 
3. En el panel de configuración, haga clic en **Organización de Azure DevOps** para seleccionar una cuenta existente o crear una. A continuación, seleccione el proyecto en el que desea configurar la canalización.  


   ![AzDevOps_project](media/tutorial-devops-azure-pipelines-classic/azure-devops-rolling.png) 
4. Un grupo de implementación es un conjunto lógico de máquinas de destino de implementación que representan los entornos físicos; por ejemplo, "Dev", "Test", "UAT" y "Production". Puede crear un grupo de implementación o seleccionar uno existente. 
5. Seleccione la canalización de compilación que publica el paquete que se va a implementar en la máquina virtual. Tenga en cuenta que el paquete publicado debe tener un script de implementación _deploy.ps1_ o _deploy.sh_ en la carpeta `deployscripts` de la raíz del paquete. La canalización de Azure DevOps ejecutará este script en tiempo de ejecución.
6. Seleccione la estrategia de implementación que prefiera. Seleccione **Blue-Green** (Azul-verde).
7. Agregue una etiqueta "blue" (azul) o "green" (verde) a las máquinas virtuales que van a formar parte de las implementaciones azul-verde. Si la máquina virtual es para un rol en espera, se debe etiquetar como "green" (verde), de lo contrario "blue" (azul).
![AzDevOps_bluegreen_configure](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-configure.png)

8. Haga clic en **Aceptar** para configurar la canalización de entrega continua. Ahora, tendrá una canalización de entrega continua configurada para implementarla en la máquina virtual.
![AzDevOps_bluegreen_pipeline](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-pipeline.png)


9. Haga clic en **Editar** la canalización de versión en Azure DevOps para ver la configuración de la canalización. La canalización consta de tres fases. La primera es una fase de grupo de implementación y se implementa en las máquinas virtuales que tengan la etiqueta _green_ (verde) (máquinas virtuales en espera). En la segunda fase, la canalización se detiene y espera la intervención manual para reanudar la ejecución. Una vez que un usuario se ha asegurado de que la implementación es estable, puede redirigir el tráfico a las máquinas virtuales con la etiqueta _green_ (verde) y reanudar la ejecución de la canalización, que, a partir de ese momento, intercambiará las etiquetas _blue_ (azul) y _green_ (verde) en las máquinas virtuales. De esta forma se asegura de que las máquinas virtuales que tengan una versión anterior de la aplicación se etiqueten como _green_ (verde) y se implementen en la siguiente ejecución de la canalización.
![AzDevOps_bluegreen_task](media/tutorial-devops-azure-pipelines-classic/azure-devops-blue-green-tasks.png)


10. La tarea para ejecutar el script de implementación ejecutará de forma predeterminada el script de implementación _deploy.ps1_ o _deploy.sh_ de la carpeta `deployscripts` del directorio raíz del paquete publicado. Asegúrese de que la canalización de compilación seleccionada lo publica en la carpeta raíz del paquete.
![AzDevOps_publish_package](media/tutorial-deployment-strategy/package.png)




## <a name="other-deployment-strategies"></a>Otras estrategias de implementación
- [Configuración de la estrategia de implementación gradual](https://aka.ms/AA7jlh8)
- [Configuración de la estrategia de implementación de valor controlado](https://aka.ms/AA7jdrz)

## <a name="azure-devops-project"></a>Proyecto de Azure DevOps 
Empezar a usar Azure nunca fue tan fácil.
 
Con DevOps Projects, empiece a ejecutar la aplicación en cualquier servicio de Azure en tan solo tres pasos: seleccione un idioma para la aplicación, un entorno de ejecución y un servicio de Azure.
 
[Más información](https://azure.microsoft.com/features/devops-projects/ ).
 
## <a name="additional-resources"></a>Recursos adicionales 
- [Implementación en máquinas virtuales de Azure con un proyecto de DevOps](https://docs.microsoft.com/azure/devops-project/azure-devops-project-vms)
- [Implementación continua de una aplicación en un conjunto de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)
