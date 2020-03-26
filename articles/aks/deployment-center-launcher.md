---
title: Centro de implementación para Azure Kubernetes
description: En Azure DevOps, el centro de implementación simplifica la configuración de una canalización de implementación sólida de Azure DevOps para su aplicación
ms.author: puagarw
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
ms.openlocfilehash: b0a9597e370648faab3787218c7d038798dbd455
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048106"
---
# <a name="deployment-center-for-azure-kubernetes"></a>Centro de implementación para Azure Kubernetes

En Azure DevOps, el centro de implementación simplifica la configuración de una canalización de implementación sólida de Azure DevOps para su aplicación. De forma predeterminada, el centro de implementación configura una canalización de Azure DevOps para implementar las actualizaciones de la aplicación en el clúster de Kubernetes. Puede extender la canalización de Azure DevOps configurada predeterminada y agregar funcionalidades más enriquecidas: la capacidad de conseguir la aprobación previa a la implementación, el aprovisionamiento de recursos de Azure adicionales, la ejecución de scripts, la actualización de la aplicación o incluso la ejecución de pruebas de validación adicionales.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Configure una canalización de Azure DevOps para implementar las actualizaciones de la aplicación en el clúster de Kubernetes.
> * Examine la canalización de integración continua (CI).
> * Examine la canalización de entrega continua (CD).
> * Limpie los recursos.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

* Un clúster de Azure Kubernetes Service (AKS).

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione la opción [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) en la barra de menús derecha de Azure Portal.

1. Para crear el clúster de AKS, ejecute los siguientes comandos:

    ```azurecli
    # Create a resource group in the South India location:

    az group create --name azooaks --location southindia

    # Create a cluster named azookubectl with one node.

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-a-kubernetes-cluster"></a>Implementación de actualizaciones de la aplicación en un clúster de Kubernetes

1. Vaya al grupo de recursos que creó en la sección anterior.

1. Seleccione el clúster de AKS y **Centro de implementación (versión preliminar)** en la hoja de la izquierda. Seleccione **Comenzar**.

   ![configuración](media/deployment-center-launcher/settings.png)

1. Elija la ubicación del código y seleccione **Siguiente**. A continuación, seleccione uno de los repositorios admitidos actualmente: **[Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)** o **GitHub**.

    Azure Repos es un conjunto de herramientas de control de versiones que ayuda a administrar el código. Independientemente de que el proyecto de software sea grande o pequeño, se recomienda usar el control de versiones lo antes posible.

    - **Azure Repos**: elija un repositorio en el proyecto y la organización existentes.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub**: Autorice y seleccione el repositorio para su cuenta de GitHub.

        ![GitHub](media/deployment-center-launcher/github.gif)


1. El centro de implementación analiza el repositorio y detecta el Dockerfile. Si desea actualizarlo, puede editar el número de puerto identificado.

    ![Configuración de la aplicación](media/deployment-center-launcher/application-settings.png)

    Si el repositorio no contiene el Dockerfile, el sistema mostrará un mensaje para confirmar uno.

    ![Dockerfile](media/deployment-center-launcher/dockerfile.png)

1. Seleccione un registro de contenedor existente o cree uno y, a continuación, seleccione **Finalizar**. La canalización se crea automáticamente y pone en cola una compilación en [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=azure-devops).

    Azure Pipelines es un servicio en la nube que puede usar para compilar y probar automáticamente el proyecto del código y hacer que esté disponible para otros usuarios. Azure Pipelines combina la integración continua y la entrega continua para probar y compilar el código de forma constante y coherente, y enviarlo a cualquier destino.

    ![Container Registry](media/deployment-center-launcher/container-registry.png)

1. Seleccione el vínculo para ver la canalización en curso.

1. Verá los registros correctos una vez que la implementación se haya completado.

    ![Registros](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>Examen de la canalización de CI

El centro de implementación configura automáticamente la canalización de CI/CD de su organización de Azure DevOps. Dicha canalización se puede explorar y personalizar.

1. Vaya al panel del centro de implementación.  

1. Seleccione el número de compilación de la lista de registros correctos para ver la canalización de compilación del proyecto.

1. Seleccione los puntos suspensivos (...) en la esquina superior derecha. Un menú muestra varias opciones, como poner en cola una nueva compilación, conservar una compilación y editar la canalización de compilación. Seleccione **Editar canalización**. 

1. En este panel puede examinar las distintas tareas de la canalización de compilación. La compilación realiza varias tareas, como recopilar los orígenes del repositorio de Git, crear una imagen, insertar una imagen en el registro de contenedor y publicar los resultados que se usan para las implementaciones.

1. Seleccione el nombre de la canalización de compilación en la parte superior de esta.

1. Cambie el nombre de la canalización de compilación por otro más descriptivo, seleccione **Guardar y poner en cola** y, luego, seleccione **Guardar**.

1. Seleccione **Historial** en la canalización de compilación. En este panel se muestra una pista de auditoría de los últimos cambios en la compilación. Azure DevOps supervisa todos los cambios realizados en la canalización de compilación y permite comparar las versiones.

1. Seleccione **Desencadenadores**. Puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**. Puede especificar directivas para conservar o quitar un número determinado de compilaciones en función del escenario.

## <a name="examine-the-cd-pipeline"></a>Examen de la canalización de CD

El centro de implementación crea y configura automáticamente la relación entre su organización de Azure DevOps y la suscripción de Azure. Esto incluye la configuración de una conexión de servicio de Azure para realizar la autenticación en su suscripción a Azure con Azure DevOps. El proceso automatizado también crea una canalización de versión de entrega continua a Azure.

1. Seleccione **Canalizaciones** y **Versiones**.

1. Para editar la canalización de versión, seleccione **Editar**.

1. Seleccione **Anular** en la lista **Artifacts**. En los pasos anteriores, la canalización de la construcción que ha examinado genera la salida que se usa para el artefacto. 

1. Seleccione el desencadenador **Implementación continua** a la derecha de la opción **Anular**. Esta canalización de versión tiene un desencadenador de implementación continua habilitado que ejecuta una implementación cuando hay un nuevo artefacto de compilación disponible. También puede deshabilitar el desencadenador para exigir la ejecución manual de sus implementaciones.

1. Para examinar todas las tareas de la canalización, seleccione **Tareas**. La versión establece el entorno de Tiller, configura el parámetro `imagePullSecrets`, instala las herramientas de Helm e implementa los gráficos de Helm en el clúster de Kubernetes.

1. Para ver el historial de versiones, seleccione **Ver versiones**.

1. Para ver el resumen, seleccione **Versión**. Seleccione cualquiera de las fases para explorar varios menús, como un resumen de las versiones, los elementos de trabajo asociados y las pruebas. 

1. Seleccione **Confirmaciones**. Esta vista muestra las confirmaciones de código relacionadas con esta implementación. Compare las versiones para ver las diferencias de confirmación entre las implementaciones.

1. Seleccione **Registros**. Los registros contienen información de implementación útil que puede ver durante las implementaciones y después de estas.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no necesite los recursos relacionados que haya creado puede eliminarlos. Use la funcionalidad de eliminación del panel de DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes

Puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. O también puede usar este modelo de CI/CD como plantilla para las demás canalizaciones.
