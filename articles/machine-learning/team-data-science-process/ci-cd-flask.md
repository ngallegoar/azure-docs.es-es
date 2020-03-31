---
title: 'Creación de una canalización de CI/CD con Azure Pipelines: Proceso de ciencia de datos en equipo'
description: Creación de una canalización de integración continua y entrega continua para aplicaciones de Inteligencia artificial (IA) mediante Docker y Kubernetes.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=jainr, previous-ms.author=jainr
ms.openlocfilehash: 42433ec419ac9e02077cd0359e18b5114206f27d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721836"
---
# <a name="create-cicd-pipelines-for-ai-apps-using-azure-pipelines-docker-and-kubernetes"></a>Creación de canalizaciones de CI/CD para aplicaciones de inteligencia artificial con Azure Pipelines, Docker y Kubernetes

Una aplicación de inteligencia artificial (IA) es un código de aplicación insertado con un modelo de aprendizaje automático (ML) previamente entrenado. Para una aplicación de inteligencia artificial siempre hay dos flujos de trabajo: Los científicos de datos crean el modelo de ML y los desarrolladores de aplicaciones compilan la aplicación y la exponen a los usuarios finales para su consumo. En este artículo se describe cómo implementar una canalización de integración continua y entrega continua (CI/CD) para una aplicación de inteligencia artificial que inserta el modelo de ML en el código fuente de la aplicación. En el código de ejemplo y el tutorial se usa una aplicación web de Python Flask y se captura un modelo previamente entrenado de una cuenta privada de Azure Blob Storage. También puede usar una cuenta de almacenamiento de AWS S3.

> [!NOTE]
> El siguiente proceso es una de las diversas formas de realizar integración continua o entrega continua. Existen alternativas a esta herramienta y a los requisitos previos.

## <a name="source-code-tutorial-and-prerequisites"></a>Código fuente, tutorial y requisitos previos

Puede descargar un [código fuente](https://github.com/Azure/DevOps-For-AI-Apps) y un [tutorial en detalle](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) desde GitHub. Siga los pasos del tutorial para implementar una canalización de CI/CD para su propia aplicación.

Para usar el código fuente y el tutorial, necesita los siguientes requisitos previos: 

- El [repositorio de código fuente](https://github.com/Azure/DevOps-For-AI-Apps) bifurcado a su cuenta de GitHub
- Una [organización de Azure DevOps](/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [CLI de Azure](/cli/azure/install-azure-cli)
- Un [clúster de Azure Container Service para Kubernetes (AKS)](/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) para ejecutar comandos y capturar la configuración del clúster de AKS 
- Una [cuenta de Azure Container Registry (ACR)](/azure/container-registry/container-registry-get-started-portal)

## <a name="cicd-pipeline-summary"></a>Resumen de la canalización de CI/CD

Cada nueva confirmación de Git inicia la canalización de compilación. La compilación extrae de forma segura el último modelo de ML automático de una cuenta de almacenamiento de blobs y lo empaqueta con el código de la aplicación en un único contenedor. Este desacoplamiento del desarrollo de aplicaciones y de los flujos de secuencias de ciencia de datos garantiza que la aplicación de producción siempre ejecuta el código más reciente con el último modelo de ML. Si la aplicación pasa las pruebas, la canalización almacena de forma segura la imagen de compilación en un contenedor de Docker en Azure Container Registry. A continuación, la canalización de versión implementa el contenedor mediante AKS. 

## <a name="cicd-pipeline-steps"></a>Pasos de la canalización de CI/CD

En el diagrama y los pasos siguientes se describe la arquitectura de canalización de CI/CD:

![Arquitectura de canalización de CI/CD](./media/ci-cd-flask/architecture.png)

1. Los desarrolladores trabajan en el código de aplicación en el entorno de desarrollo integrado que prefieran.
2. Los desarrolladores confirman el código a Azure Repos, GitHub u otro proveedor de control de código fuente de Git. 
3. Por separado, los científicos de datos trabajan en el desarrollo de su modelo de ML.
4. Los científicos de datos publican el modelo acabado en un repositorio de modelos; en este caso, una cuenta de almacenamiento de blobs. 
5. Azure Pipelines inicia una compilación basada en la confirmación de Git.
6. La canalización de compilación extrae el modelo más reciente de ML del almacenamiento de blobs y crea un contenedor.
7. La canalización envía la imagen de compilación al repositorio de imágenes privadas de ACR.
8. La canalización de versión se inicia basándose en la compilación correcta.
9. La canalización extrae la imagen más reciente de ACR y la implementa en el clúster de Kubernetes en AKS.
10. Las solicitudes de usuario para la aplicación pasan por el servidor DNS.
11. El servidor DNS pasa la solicitud al equilibrador de carga y devuelve las respuestas al usuario.

## <a name="see-also"></a>Consulte también

- [Proceso de ciencia de datos en equipo (TDSP)](/azure/machine-learning/team-data-science-process/)
- [Azure Machine Learning (AML)](/azure/machine-learning/)
- [Azure DevOps](https://azure.microsoft.com/services/devops/)
- [Azure Kubernetes Services (AKS)](/azure/aks/intro-kubernetes)
