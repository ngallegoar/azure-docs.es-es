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
ms.date: 06/28/2020
ms.author: memildin
ms.openlocfilehash: c01ed6dbbd6e1f7febfb99df11d2ee67cb1e5465
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800617"
---
# <a name="container-security-in-security-center"></a>Seguridad de los contenedores en Security Center

Azure Security Center es la solución nativa de Azure para proteger los contenedores. Security Center puede proteger los siguientes tipos de recursos de contenedor:



|Recurso |Nombre  |Detalles  |
|:---------:|---------|---------|
|![Host de contenedor](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png)|Hosts de contenedor (máquinas virtuales que ejecutan Docker)|Security Center examina las configuraciones de Docker y le ofrece visibilidad sobre los errores de configuración al proporcionarle una lista de todas las reglas con errores que se han evaluado. Security Center proporciona directrices para ayudarle a resolver estos problemas rápidamente y ahorrar tiempo. Security Center evalúa continuamente las configuraciones de Docker y le proporciona su estado más reciente.|
|![Servicio Kubernetes](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png)|Clústeres de Azure Kubernetes Service (AKS)|Obtenga más visibilidad de los nodos de AKS, el tráfico en la nube y los controles de seguridad mediante la [agrupación de AKS opcional de Security Center](azure-kubernetes-service-integration.md) para usuarios del nivel estándar.|
|![Registro de contenedor](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png)|Registros de Azure Container Registry (ACR)|Obtener una mayor visibilidad de las vulnerabilidades de las imágenes de los registros de ACR basados en ARM con la [agrupación de ACR opcional de Security Center](azure-kubernetes-service-integration.md) para usuarios del nivel estándar.|
||||


En este artículo se describe cómo puede usar estas agrupaciones para mejorar, supervisar y mantener la seguridad de los contenedores y sus aplicaciones. Aprenderá a usar Security Center para que le ayude con estos aspectos básicos de la seguridad de los contenedores:

- [Administración de vulnerabilidades: análisis de imágenes de contenedor](#vulnerability-management---scanning-container-images)
- [Protección del entorno: supervisión continua de los clústeres de Kubernetes y la configuración de Docker](#environment-hardening)
- [Protección en tiempo de ejecución: detección de amenazas en tiempo real](#run-time-protection---real-time-threat-detection)

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

[!INCLUDE [AKS in ASC threat protection](../../includes/security-center-azure-kubernetes-threat-protection.md)]




## <a name="next-steps"></a>Pasos siguientes

En esta información general, ha aprendido los elementos básicos de la seguridad de los contenedores en Azure Security Center. Siga con [cómo supervisar la seguridad de los contenedores](monitor-container-security.md).
> [!div class="nextstepaction"]
> [Supervisión de la seguridad de los contenedores](monitor-container-security.md)