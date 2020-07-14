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
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: 94d1bccc9a7f45d24d8c5b92aecba54d9f7f630a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800191"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Integración de Azure Kubernetes Services con Security Center

Azure Kubernetes Service (AKS) es un servicio que administra Microsoft y que permite desarrollar, implementar y administrar las aplicaciones en contenedores. 

Si se encuentra en el nivel estándar de Azure Security Center, puede agregar el conjunto de AKS (consulte los [precios](security-center-pricing.md)) para obtener una mayor visibilidad de los nodos de AKS, el tráfico en la nube y los controles de seguridad.

En conjunto, Security Center y AKS proporcionan la mejor oferta de seguridad Kubernetes nativa de la nube.

## <a name="what-are-the-components-of-security-centers-kubernetes-protection"></a>¿Cuáles son los componentes de la protección de Security Center para Kubernetes?

Las protecciones de Security Center para Kubernetes se ofrecen mediante una combinación de dos elementos:

- **Protección contra amenazas de Azure Security Center para máquinas virtuales**: Con el mismo agente de Log Analytics que usa Security Center en otras VM, Security Center puede mostrarle los problemas de seguridad que se producen en los nodos de AKS. El agente también supervisa el análisis específico de los contenedores.

- **Conjunto opcional para Kubernetes de Azure Security Center**: El conjunto para Kubernetes recibe registros e información del subsistema Kubernetes a través del servicio AKS. Estos registros ya están disponibles en Azure a través del servicio AKS. Cuando se habilita el conjunto para Kubernetes de Security Center, se concede a Security Center acceso a los registros. Por lo tanto, Security Center aporta ventajas de seguridad a los clústeres de AKS gracias a los datos recopilados por el nodo principal de AKS. Algunos de los datos del entorno de Kubernetes que examina Azure Security Center pueden contener información confidencial.

    ![Información general de alto nivel de Azure Security Center y Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

## <a name="what-protections-are-provided"></a>¿Qué protecciones se ofrecen?

Si usa de forma conjunta estos dos servicios, obtendrá lo siguiente:

* **Recomendaciones de seguridad**: Security Center identifica los recursos de AKS y los categoriza; desde clústeres hasta máquinas virtuales individuales. Puede ver las recomendaciones de seguridad de cada recurso. Para más información, consulte las recomendaciones de contenedores en la [lista de referencias de recomendaciones](recommendations-reference.md#recs-containers). 

* **Protección del entorno**: Security Center supervisa constantemente la configuración de los clústeres de Kubernetes y configuraciones de Docker. A continuación, genera recomendaciones de seguridad que reflejan los estándares del sector.

* **Protección en tiempo de ejecución**: Gracias al análisis continuo de los siguientes orígenes de AKS, Security Center le alerta de las amenazas y actividades malintencionadas que se detectan en el host *y* el nivel de clúster de AKS. [Obtenga más información sobre la protección contra amenazas para contenedores](threat-protection.md#azure-containers).


     

![Información detallada de Azure Security Center y Azure Kubernetes Service (AKS)](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)



## <a name="aks-with-security-center-faq"></a>Preguntas más frecuentes sobre AKS con Security Center

### <a name="can-i-still-get-aks-protections-without-the-log-analytics-agent"></a>¿Puedo obtener protecciones de AKS aún sin el agente de Log Analytics?

Como se mencionó anteriormente, el conjunto opcional para Kubernetes proporciona protecciones a nivel de clúster, el agente de Log Analytics del nivel estándar de Azure Security Center protege los nodos. 

Se recomienda implementar ambos para obtener la protección más completa posible.

Si decide no instalar el agente en los hosts, solo recibirá un subconjunto de las ventajas de la protección contra amenazas y las alertas de seguridad. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.



## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Container Registry](azure-container-registry-integration.md)

* [Administración de datos en Microsoft](https://www.microsoft.com/trust-center/privacy/data-management): Aquí se describen las directivas de datos de los servicios de Microsoft (incluidos Azure, Intune y Microsoft 365), los detalles de la administración de datos de Microsoft y las directivas de retención que afectan a los datos.
