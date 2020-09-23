---
title: Azure Security Center y Azure Container Registry
description: Obtenga información sobre cómo examinar los registros de contenedor con Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 1335b1034304b7efe2b113f7ff2d2927fea41638
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90977364"
---
# <a name="azure-container-registry-image-scanning-by-security-center"></a>Análisis de imágenes de Azure Container Registry por Security Center

Azure Container Registry (ACR) es un servicio de registro de Docker privado y administrado que almacena y administra las imágenes de contenedor de las implementaciones de Azure en un registro central. Se basa en el registro 2.0 de Docker de código abierto.

Habilite **Azure Defender para registros de contenedor** para una mayor visibilidad de las vulnerabilidades de las imágenes de los registros basados en Azure Resource Manager. Habilite o deshabilite el plan en el nivel de suscripción para abarcar todos los registros de una suscripción. Esta característica se cobra por imagen, tal como se muestra en la [página de precios](security-center-pricing.md). Al habilitar Azure Defender se asegura de que Security Center esté listo para examinar las imágenes que se inserten en el registro. 


## <a name="when-are-images-scanned"></a>¿Cuándo se examinan las imágenes?

Cada vez que se inserta una imagen en el registro, Security Center la examina de forma automática. Para desencadenar el examen de una imagen, insértela en el repositorio.

Cuando finaliza el examen (al cabo de 2 minutos aproximadamente, aunque puede tardar hasta 15 minutos), los resultados están disponibles en recomendaciones de Security Center como esta:

[![Ejemplo de recomendación Azure Security Center sobre puntos vulnerables detectados en una imagen hospedada de Azure Container Registry (ACR)](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Ventajas de la integración

Security Center identifica los registros de ACR basados en Azure Resource Manager en su suscripción y proporciona de forma fácil lo siguiente:

* **Detección de vulnerabilidades nativas de Azure** para todas las imágenes de Linux insertadas. Security Center examina la imagen con un escáner del proveedor de detección de vulnerabilidades líder del sector, Qualys. Esta solución nativa se integra sin problemas de forma predeterminada.

* **Recomendaciones de seguridad** para las imágenes de Linux con vulnerabilidades conocidas. Security Center proporciona detalles de cada vulnerabilidad detectada y una clasificación de gravedad. Asimismo, ofrece instrucciones sobre cómo corregir las vulnerabilidades específicas encontradas en cada imagen insertada en el registro.

![Introducción de alto nivel de Azure Security Center y Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="faq-for-azure-container-registry-image-scanning"></a>Preguntas más frecuentes sobre el análisis de imágenes de Azure Container Registry

### <a name="how-does-security-center-scan-an-image"></a>¿De qué forma analiza Security Center las imágenes?
La imagen se extrae del registro. Luego, se ejecuta en un espacio aislado con el escáner de Qualys, que extrae una lista de puntos vulnerables conocidos.

Security Center filtra y clasifica los resultados del análisis. Cuando una imagen es correcta, Security Center la marca como tal. Security Center solo genera recomendaciones de seguridad para las imágenes que tienen incidencias sin resolver. Al enviar notificaciones solo cuando hay problemas, Security Center reduce las alertas informativas no deseadas.

### <a name="how-often-does-security-center-scan-my-images"></a>¿Con qué frecuencia examina Security Center mis imágenes?
Los exámenes de imágenes se desencadenan en todas las inserciones.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>¿Se pueden obtener los resultados del examen mediante la API REST?
Sí. Los resultados se encuentran en [API REST de valoración secundaria](/rest/api/securitycenter/subassessments/list/). Asimismo, puede usar Azure Resource Graph (ARG), la API similar a Kusto para todos los recursos: una consulta puede recuperar un análisis específico.
 
### <a name="what-registry-types-are-scanned-what-types-are-billed"></a>¿Qué tipos de registros se analizan? ¿Qué tipos se facturan?
En la sección de disponibilidad se enumeran los tipos de registros de contenedor que admite el conjunto de registros de contenedor. 

Si los registros que no se admiten están conectados a su suscripción de Azure, no se examinarán y no se facturarán.


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Kubernetes Service](azure-kubernetes-service-integration.md)


