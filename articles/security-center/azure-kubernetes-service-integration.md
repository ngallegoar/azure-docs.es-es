---
title: Azure Security Center y Azure Kubernetes Service
description: Obtenga más información sobre la integración de Azure Security Center con Azure Kubernetes Services
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: d5c74ac760b5746d37468d692c2a46d5aedbee72
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436175"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Integración de Azure Kubernetes Services con Security Center

Azure Kubernetes Service (AKS) es un servicio que administra Microsoft y que permite desarrollar, implementar y administrar las aplicaciones en contenedores. 

Use AKS junto con el nivel estándar de Azure Security Center (consulte los [precios](security-center-pricing.md)) para obtener una mayor visibilidad de los nodos de AKS, el tráfico en la nube y los controles de seguridad.

Security Center aporta ventajas de seguridad a los clústeres de AKS gracias a los datos que recopiló el nodo principal de AKS. 

![Información general de alto nivel de Azure Security Center y Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

Si trabajan juntas, estas dos herramientas proporcionan la mejor oferta de seguridad Kubernetes nativa en la nube. 

## <a name="benefits-of-integration"></a>Ventajas de la integración

Si usa de forma conjunta estos dos servicios, obtendrá lo siguiente:

* **Recomendaciones de seguridad**: Security Center identifica los recursos de AKS y los categoriza; desde clústeres hasta máquinas virtuales individuales. Puede ver las recomendaciones de seguridad de cada recurso. Para más información, consulte las recomendaciones de contenedores en la [lista de referencias de recomendaciones](recommendations-reference.md#recs-containers). 

* **Protección del entorno**: Security Center supervisa constantemente la configuración de los clústeres de Kubernetes y configuraciones de Docker. A continuación, genera recomendaciones de seguridad que reflejan los estándares del sector.

* **Protección en tiempo de ejecución**: gracias al análisis continuo de los siguientes orígenes de AKS, Security Center le alerta de las amenazas y actividades malintencionadas que se detectan en el host *y* el nivel de clúster de AKS:
    * Eventos de seguridad sin procesar, como datos de red y creación de procesos
    * Registro de auditoría de Kubernetes

    Para más información, consulte el artículo sobre la [protección contra amenazas para contenedores de Azure](threat-protection.md#azure-containers).

    Para obtener la lista de posibles alertas, consulte estas secciones en la tabla de referencia de alertas: [Alertas de nivel de clúster de AKS](alerts-reference.md#alerts-akscluster) y [alertas de nivel de host de contenedor](alerts-reference.md#alerts-containerhost).  

![Información detallada de Azure Security Center y Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)

> [!NOTE]
> Algunos de los datos del entorno de Kubernetes que examina Azure Security Center pueden contener información confidencial.


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Container Registry](azure-container-registry-integration.md)

* [Administración de datos en Microsoft](https://www.microsoft.com/trust-center/privacy/data-management): aquí se describen las directivas de datos de los servicios de Microsoft (incluidos Azure, Intune y Office 365), los detalles de la administración de datos de Microsoft y las directivas de retención que afectan a los datos.
