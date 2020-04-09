---
title: Seguridad de los contenedores en Azure Security Center | Microsoft Docs
description: Obtenga más información sobre las características de seguridad de los contenedores de Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: memildin
ms.openlocfilehash: d46e2a9820ec0c45d197f135428f1ace712b2fb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125138"
---
# <a name="container-security-in-security-center"></a>Seguridad de los contenedores en Security Center

Azure Security Center es la solución nativa de Azure para la seguridad de los contenedores. Asimismo, Security Center también es el único panel óptimo que ofrece una experiencia transparente para garantizar la seguridad de sus cargas de trabajo, VM, servidores y contenedores en la nube.

En este artículo se describe la forma en que Security Center ayuda a mejorar, supervisar y mantener la seguridad de los contenedores y sus aplicaciones. Aprenderá a usar Security Center para que le ayude con estos aspectos básicos de la seguridad de los contenedores:

* Administración de vulnerabilidades
* Protección del entorno del contenedor
* Protección en tiempo de ejecución

[![Pestaña de seguridad de los contenedores de Azure Security Center](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

Para obtener instrucciones sobre cómo usar estas características, consulte [Supervisión de la seguridad de los contenedores](monitor-container-security.md).

## <a name="vulnerability-management---scanning-container-images"></a>Administración de vulnerabilidades: análisis de imágenes de contenedor
Para supervisar la instancia de Azure Container Registry basada en Azure Resource Manager, asegúrese de que está en el nivel estándar de Security Center (consulte [precios](/azure/security-center/security-center-pricing)). Luego, habilite el conjunto opcional de registros de contenedor. Cuando se inserta una nueva imagen, Security Center examina la imagen con un escáner del proveedor de detección de vulnerabilidades líder del sector, Qualys.

Si Qualys o Security Center encuentran incidencias, recibirá una notificación en el panel de Security Center. Por cada vulnerabilidad, Security Center proporciona recomendaciones útiles, junto con una clasificación de gravedad e información sobre cómo corregir el problema. Para más información sobre las recomendaciones de Security Center para los contenedores, consulte la [lista de recomendaciones de referencia](recommendations-reference.md#recs-containers).

Security Center filtra y clasifica los resultados del análisis. Cuando una imagen es correcta, Security Center la marca como tal. Security Center solo genera recomendaciones de seguridad para las imágenes que tienen incidencias sin resolver. Al enviar notificaciones solo cuando hay problemas, Security Center reduce las alertas informativas no deseadas.

## <a name="environment-hardening"></a>Protección del entorno

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Supervisión continua de la configuración de Docker
Azure Security Center identifica contenedores no administrados y que están hospedados en VM de IaaS Linux u otras máquinas de Linux que ejecutan contenedores de Docker. Security Center evalúa continuamente las configuraciones de estos contenedores. A continuación, las compara con el [Banco de prueba para Docker del Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/docker/).

Security Center incluye todo el conjunto de reglas del banco de prueba de Docker de CIS y le avisa si los contenedores no cumplen ninguno de los controles. Cuando encuentra configuraciones inactivas, Security Center genera recomendaciones de seguridad. Use la **página de recomendaciones** para ver las recomendaciones y corregir los problemas. También verá las recomendaciones en la pestaña de **contenedores** que muestra todas las máquinas virtuales implementadas con Docker. 

Para obtener información sobre las recomendaciones pertinentes de Security Center que pueden aparecer para esta característica, consulte la [sección de contenedores](recommendations-reference.md#recs-containers) de la tabla de referencia de recomendaciones.

Cuando explora los problemas de seguridad de una máquina virtual, Security Center proporciona información adicional sobre los contenedores de la máquina. Esta información incluye la versión de Docker y el número de imágenes que se ejecutan en el host. 

>[!NOTE]
> Estas comprobaciones del banco de prueba de CIS no se ejecutarán en instancias que administre AKS o en VM que administre Databricks.

### <a name="continuous-monitoring-of-your-kubernetes-clusters"></a>Supervisión continua de los clústeres de Kubernetes
Security Center funciona junto con Azure Kubernetes Service (AKS), el servicio de orquestación de contenedores administrados de Microsoft que le permite desarrollar, implementar y administrar las aplicaciones en contenedores.

AKS proporciona controles de seguridad y visibilidad sobre la postura de seguridad de los clústeres. Security Center utiliza estas características para:
* Supervisar constantemente la configuración de los clústeres de AKS
* Generar recomendaciones de seguridad que cumplan con los estándares del sector

Para obtener información sobre las recomendaciones pertinentes de Security Center que pueden aparecer para esta característica, consulte la [sección de contenedores](recommendations-reference.md#recs-containers) de la tabla de referencia de recomendaciones.

## <a name="run-time-protection---real-time-threat-detection"></a>Protección en tiempo de ejecución: detección de amenazas en tiempo real

Security Center proporciona detección de amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Detectamos amenazas en el nivel de clúster del host y AKS. Para obtener una información completa, consulte [Detección de amenazas para contenedores de Azure](threat-protection.md#azure-containers).


## <a name="container-security-faq"></a>Preguntas frecuentes sobre la seguridad de los contenedores

### <a name="what-types-of-images-can-azure-security-center-scan"></a>¿Qué tipos de imágenes puede examinar Azure Security Center?
Security Center analiza imágenes basadas en el sistema operativo Linux que proporcionan acceso a shell. 

El escáner de Qualys no admite imágenes excesivamente minimalistas, como las imágenes [base de Docker](https://hub.docker.com/_/scratch/) o imágenes "sin distribución" que solo contienen su aplicación y sus dependencias del runtime sin un administrador de paquetes, shell o sistema operativo.

### <a name="how-does-azure-security-center-scan-an-image"></a>¿De qué forma analiza Azure Security Center las imágenes?
La imagen se extrae del registro. Luego, se ejecuta en un espacio aislado con el escáner de Qualys, que extrae una lista de puntos vulnerables conocidos.

Security Center filtra y clasifica los resultados del análisis. Cuando una imagen es correcta, Security Center la marca como tal. Security Center solo genera recomendaciones de seguridad para las imágenes que tienen incidencias sin resolver. Al enviar notificaciones solo cuando hay problemas, Security Center reduce las alertas informativas no deseadas.

### <a name="how-often-does-azure-security-center-scan-my-images"></a>¿Con qué frecuencia examina Azure Security Center mis imágenes?
Los exámenes de imágenes se desencadenan en todas las inserciones.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>¿Se pueden obtener los resultados del examen mediante la API REST?
Sí. Los resultados se encuentran en [API REST de valoración secundaria](/rest/api/securitycenter/subassessments/list/). Asimismo, puede usar Azure Resource Graph (ARG), la API similar a Kusto para todos los recursos: una consulta puede recuperar un análisis específico.
 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la seguridad de los contenedores en Azure Security Center, consulte estos artículos relacionados:

* Para ver la postura de seguridad de los recursos relacionados con la seguridad, consulte la sección de contenedores de [Protección de máquinas y aplicaciones](security-center-virtual-machine-protection.md#containers).

* Detalles de la [integración con Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* Detalles de la [integración con Azure Container Registry](azure-container-registry-integration.md)