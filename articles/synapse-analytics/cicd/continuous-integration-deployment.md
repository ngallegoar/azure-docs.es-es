---
title: Integración y entrega continuas para las áreas de trabajo de Synapse
description: Aprenda a usar la integración y la entrega continuas para implementar los cambios en el área de trabajo de un entorno (desarrollo, prueba o producción) a otro.
services: synapse-analytics
author: liud
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: liud
ms.reviewer: pimorano
ms.openlocfilehash: 5dbd49312b58dc656e2239e8a0a4acea614023de
ms.sourcegitcommit: e5f9126c1b04ffe55a2e0eb04b043e2c9e895e48
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96317143"
---
# <a name="continuous-integration-and-delivery-for-azure-synapse-workspace"></a>Integración y entrega continuas para las áreas de trabajo de Azure Synapse

## <a name="overview"></a>Información general

La integración continua (CI) es el proceso de automatizar la compilación y las pruebas de código cada vez que un miembro del equipo confirma cambios en el control de versiones. La implementación continua (CD) es el proceso de compilar, probar, configurar e implementar desde varios entornos de pruebas o ensayo en un entorno de producción.

En las áreas de trabajo de Azure Synapse, la integración y la entrega continuas (CI/CD) mueven todas las entidades de un entorno (desarrollo, prueba, producción) a otro. Para promover el área de trabajo a otra área de trabajo, hay dos partes: use [plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview) para crear o actualizar los recursos del área de trabajo (grupos y área de trabajo); migrar artefactos (scripts de SQL, cuadernos, definiciones de trabajos de Spark, canalizaciones, conjuntos de datos, flujos de datos, etc.) con herramientas de CI/CD de Synapse en Azure DevOps. 

En este artículo se describe el uso de canalizaciones de versión de Azure para automatizar la implementación de un área de trabajo de Synapse en varios entornos.

## <a name="prerequisites"></a>Requisitos previos

-   El área de trabajo que se usa para el desarrollo se ha configurado con un repositorio de Git en Studio. Consulte [Control de código fuente en Synapse Studio](source-control.md).
-   Un proyecto de Azure DevOps se ha preparado para ejecutar la canalización de versión.

## <a name="set-up-a-release-pipelines"></a>Configuración una canalización de versión

1.  En [Azure DevOps](https://dev.azure.com/), abra el proyecto creado para la versión.

1.  En el lado izquierdo de la página, seleccione **Canalizaciones** y después **Versiones**.

    ![Selección de Canalizaciones, Versiones](media/create-release-1.png)

1.  Seleccione **Nueva canalización** o, si tiene canalizaciones existentes, seleccione **Nueva** y, luego **Nueva canalización de versión**.

1.  Seleccione la plantilla **Fase vacía**.

    ![Selección de Trabajo vacío](media/create-release-select-empty.png)

1.  En el cuadro **Nombre de la fase**, escriba el nombre del entorno.

1.  Seleccione **Agregar artefacto** y, luego, elija el mismo repositorio de Git configurado con la instancia de Synapse Studio de desarrollo. Seleccione el repositorio de Git que usó para administrar la plantilla de Resource Manager de grupos y áreas de trabajo. Si usa GitHub como origen, debe crear una conexión de servicio para la cuenta de GitHub y los repositorios de incorporación de cambios. Para más información, consulte [Conexiones de servicio](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints). 

    ![Adición de una rama de publicación](media/release-creation-github.png)

1.  Seleccione la rama de la plantilla de Resource Manager para la actualización de recursos. En **Versión predeterminada**, seleccione **Más reciente de la rama predeterminada**.

    ![Adición de una plantilla de Resource Manager](media/release-creation-arm-branch.png)

1.  Seleccione la [rama de publicación](source-control.md#configure-publishing-settings) del repositorio en **Rama predeterminada**. De forma predeterminada, esta rama de publicación es `workspace_publish`. En **Versión predeterminada**, seleccione **Más reciente de la rama predeterminada**.

    ![Agregar un artefacto](media/release-creation-publish-branch.png)

## <a name="set-up-a-stage-task-for-arm-resource-create-and-update"></a>Configuración de una tarea de fase para la creación y actualización de recursos de Azure Resource Manager 

Agregue una tarea de implementación de Azure Resource Manager para crear o actualizar recursos, incluidos los grupos y las áreas de trabajo:

1. En la vista de fase, seleccione **Ver tareas de la fase**.

    ![Vista de fase](media/release-creation-stage-view.png)

1. Cree una nueva tarea. Busque **Implementación de una plantilla de Resource Manager** y, después, seleccione **Agregar**.

1. En la tarea de implementación, seleccione la suscripción, el grupo de recursos y la ubicación del área de trabajo de destino. Proporcione las credenciales si es necesario.

1. En la lista **Acción**, seleccione **Create or update resource group** (Crear o actualizar grupo de recursos).

1. Seleccione el botón de puntos suspensivos ( **...** ) situado junto al cuadro **Plantilla**. Busque la plantilla de Azure Resource Manager de su área de trabajo de destino.

1. Seleccione **…** junto al cuadro **Parámetros de plantilla** para elegir el archivo de parámetros.

1. Seleccione **…** junto al cuadro **Reemplazar parámetros de plantilla** y escriba los valores de los parámetros deseados del área de trabajo de destino. 

1. Seleccione **Incremental** para el **Modo de implementación**.
    
    ![Implementación de áreas de trabajo y grupos](media/pools-resource-deploy.png)

1. (Opcional) Agregue **Azure PowerShell** para la asignación de roles de área de trabajo de concesión y actualización. Si usa la canalización de versión para crear un área de trabajo de Synapse, la entidad de servicio de la canalización se agrega como administrador del área de trabajo predeterminada. Puede ejecutar PowerShell para conceder a otras cuentas acceso al área de trabajo. 
    
    ![Concesión de permisos](media/release-creation-grant-permission.png)

 > [!WARNING]
> En el modo de implementación completa, se **eliminarán** aquellos recursos que existan en el grupo de recursos, pero no estén especificados en la nueva plantilla de Resource Manager. Para más información, consulte [Modos de implementación de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/templates/deployment-modes).

## <a name="set-up-a-stage-task-for-artifacts-deployment"></a>Configuración de una tarea de fase para la implementación de artefactos 

Use la extensión [Synapse workspace deployment](https://marketplace.visualstudio.com/items?itemName=AzureSynapseWorkspace.synapsecicd-deploy) para implementar otros elementos del área de trabajo de Synapse, como conjuntos de datos, scripts de SQL, cuadernos, definiciones de trabajos de Spark, flujos de datos, canalizaciones, servicios vinculados, credenciales e instancias de IR (Integration Runtime).  

1. Busque y obtenga la extensión en el **marketplace de Azure DevOps**(https://marketplace.visualstudio.com/azuredevops) 

     ![Obtención de una extensión](media/get-extension-from-market.png)

1. Seleccione una organización para instalar la extensión. 

     ![Instalación de la extensión](media/install-extension.png)

1. Asegúrese de que se haya concedido a la entidad de servicio de la canalización de Azure DevOps el permiso de suscripción y de que esté asignada como administrador del área de trabajo de destino. 

1. Cree una nueva tarea. Busque **Synapse workspace deployment** y, luego, seleccione **Agregar**.

     ![Adición de extensiones](media/add-extension-task.png)

1.  En la tarea, proporcione información de **workspace_publish** del repositorio de Git relacionado y seleccione el grupo de recursos, la región, el nombre y el entorno de nube del área de trabajo de destino. Proporcione parámetros y valores si es necesario.

    ![Implementación del área de trabajo de Synapse](media/create-release-artifacts-deployment.png)

> [!IMPORTANT]
> En escenarios de CI/CD, el tipo de entorno de ejecución de integración (IR) en otros entornos debe ser el mismo. Por ejemplo, si tiene un IR autohospedado en el entorno de desarrollo, el mismo IR también debe ser de tipo autohospedado en otros entornos, como los de prueba y producción. Del mismo modo, si va a compartir los entornos de ejecución de integración entre varias fases, tendrá que configurarlos como autohospedados vinculados en todos los entornos: desarrollo, prueba y producción.

## <a name="create-release-for-deployment"></a>Creación de una versión para la implementación 

Después de guardar todos los cambios, puede seleccionar **Create release** (Crear versión) para crear manualmente una versión. Para automatizar la creación de versiones, consulte [Desencadenadores de versión de Azure DevOps](https://docs.microsoft.com/azure/devops/pipelines/release/triggers).

   ![Selección de Crear versión](media/release-creation-manually.png)

## <a name="best-practices-for-cicd"></a>Procedimientos recomendados para CI/CD

Si usa la integración de Git con el área de trabajo de Synapse y tiene una canalización de CI/CD que mueve los cambios del entorno de desarrollo al de prueba y, luego, al de producción, los procedimientos recomendados son los siguientes:

-   **Integración de Git**. Configure solo el área de trabajo de desarrollo de Synapse con la integración de Git. Los cambios en las áreas de trabajo de prueba y producción se implementan a través de CI/CD y no se necesita la integración de Git.
-   **Prepare los grupos antes de la migración de los artefactos**. Si tiene un script de SQL o un cuaderno asociados a grupos del área de trabajo de desarrollo, se espera el mismo nombre de los grupos en distintos entornos. 
-   **Infraestructura como código (IaC)** . Administración de la infraestructura (redes, máquinas virtuales, equilibradores de carga y topología de conexión) en un modelo descriptivo, donde se usa el mismo control de versiones que utiliza el equipo de DevOps para el código fuente. 
-   **Otros**. Consulte [Procedimientos recomendados para artefactos de ADF](/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd).


