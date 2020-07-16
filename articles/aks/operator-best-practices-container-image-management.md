---
title: 'Procedimientos recomendados para operadores: Administración de imágenes de contenedor en Azure Kubernetes Services (AKS)'
description: Obtenga información sobre los procedimientos recomendados del operador de clústeres para saber cómo administrar y proteger las imágenes de contenedor en Azure Kubernetes Service (AKS).
services: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 018dc1802d4f0b4b498d3993b8753990598a16ba
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86251151"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Procedimientos recomendados para la administración y la protección de las imágenes de contenedor en Azure Kubernetes Service (AKS)

A medida que desarrolla y ejecuta aplicaciones en Azure Kubernetes Service (AKS), es importante tener en cuenta la seguridad de los contenedores y de sus imágenes. Los contenedores que incluyen imágenes base obsoletas o entornos de ejecución de la aplicación sin revisiones presentan un riesgo de seguridad y un posible vector de ataque. Para minimizar estos riesgos, debe integrar herramientas que busquen y corrijan errores en los contenedores tanto en el momento de la compilación como en el momento de la ejecución. Cuanto más temprano en el proceso se detecte la vulnerabilidad o la imagen de base desactualizada, más seguro será el clúster. En este artículo, *contenedores* se refiere tanto a las imágenes de contenedor almacenadas en un registro de contenedor como a los contenedores en ejecución.

En este artículo se indica cómo proteger los contenedores de AKS. Aprenderá a:

> [!div class="checklist"]
> * Buscar y corregir vulnerabilidades de la imagen
> * Desencadenar automáticamente y volver a implementar imágenes de contenedor cuando se actualiza una imagen base

También puede leer los procedimientos recomendados para la [seguridad del clúster][best-practices-cluster-security] y la [seguridad de pod][best-practices-pod-security].

También puede usar la [seguridad de contenedor de Security Center][security-center-containers] para facilitar el análisis de los contenedores en busca de vulnerabilidades.  También hay [integración de Azure Container Registry][security-center-acr] con Security Center para ayudar a proteger las imágenes y el Registro de vulnerabilidades.

## <a name="secure-the-images-and-run-time"></a>Proteger las imágenes y el tiempo de ejecución

**Orientación con procedimientos recomendados**: analizar las imágenes del contenedor en busca de vulnerabilidades y solo implementar las imágenes que han pasado dicha validación. Actualizar regularmente las imágenes base y el entorno de ejecución de la aplicación y luego volver a implementar las cargas de trabajo en el clúster de AKS.

Un problema con la adopción de cargas de trabajo basadas en contenedores es comprobar la seguridad de las imágenes y el entorno de ejecución usados para compilar sus propias aplicaciones. ¿Cómo asegurarse de que no introduce vulnerabilidades de seguridad en sus implementaciones? El flujo de trabajo de implementación debe incluir un proceso para examinar las imágenes de contenedor con herramientas como [Twistlock][twistlock] o [Aqua][aqua], y, después, solo permitir que se implementen imágenes comprobadas.

![Analizar y corregir imágenes de contenedor, validar e implementar](media/operator-best-practices-container-security/scan-container-images-simplified.png)

En un ejemplo del mundo real, puede usar una integración continua y una canalización de implementación continua (CI/CD) para automatizar el análisis, la comprobación y la implementación de imágenes. Azure Container Registry incluye estas funcionalidades de análisis de vulnerabilidades.

## <a name="automatically-build-new-images-on-base-image-update"></a>Compilar automáticamente nuevas imágenes en la actualización de imagen base

**Orientación con procedimientos recomendados**: cuando usa imágenes base para las imágenes de la aplicación, use la automatización para crear nuevas imágenes cuando se actualiza la imagen base. Como estas imágenes base suelen incluir correcciones de seguridad, actualice cualquier imagen de contenedor de aplicación de bajada.

Cada vez que se actualiza una imagen base, también se deben actualizar las imágenes de contenedor de bajada. Este proceso de compilación debe integrarse en las canalizaciones de validación e implementación tipo [Azure Pipelines][azure-pipelines] o Jenkins. Estas canalizaciones aseguran que tus aplicaciones siguen ejecutándose en las imágenes base actualizadas. Una vez que se validan las imágenes de contenedor de aplicación, se pueden actualizar las implementaciones de AKS para ejecutar las imágenes más recientes y seguras.

Azure Container Registry Tasks también puede actualizar automáticamente las imágenes de contenedor cuando se actualiza la imagen base. Esta característica permite compilar un número pequeño de imágenes base, y mantenerlas actualizadas regularmente con las correcciones de errores y de seguridad.

Para más información sobre las actualizaciones de imagen base, consulte el artículo de [Automatización de compilaciones de imágenes al actualizarse una imagen base con Azure Container Registry Tasks][acr-base-image-update].

## <a name="next-steps"></a>Pasos siguientes

En este artículo se indica cómo proteger los contenedores. Para implementar algunas de estas áreas, consulte los artículos siguientes:

* [Automatización de compilaciones de imágenes al actualizarse una imagen base con Azure Container Registry Tasks][acr-base-image-update]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md
[security-center-containers]: ../security-center/container-security.md
[security-center-acr]: ../security-center/azure-container-registry-integration.md
