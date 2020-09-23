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
ms.openlocfilehash: 7f43f50fc35b5ecbc9720224036ea13e2ba753f5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90894937"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Integración de Azure Kubernetes Services con Security Center

Azure Kubernetes Service (AKS) es un servicio que administra Microsoft y que permite desarrollar, implementar y administrar las aplicaciones en contenedores. 

Habilite **Azure Defender para Kubernetes** para obtener una mayor visibilidad de los nodos de AKS, el tráfico en la nube y los controles de seguridad.

En conjunto, Security Center y AKS proporcionan la mejor oferta de seguridad Kubernetes nativa de la nube.

## <a name="what-are-the-components-of-security-centers-kubernetes-protection"></a>¿Cuáles son los componentes de la protección de Security Center para Kubernetes?

Las protecciones de Security Center para Kubernetes se ofrecen mediante una combinación de dos elementos:

- **Protección contra amenazas de Azure Security Center para máquinas virtuales**: Con el mismo agente de Log Analytics que usa Security Center en otras VM, Security Center puede mostrarle los problemas de seguridad que se producen en los nodos de AKS. El agente también supervisa el análisis específico de los contenedores.

- **Plan opcional de Azure Defender para Kubernetes de Azure Security Center**: el plan de Kubernetes recibe registros e información del subsistema de Kubernetes a través del servicio AKS. Estos registros ya están disponibles en Azure a través del servicio AKS. Cuando habilita Azure Defender para Kubernetes, se concede a Security Center acceso a los registros. Por lo tanto, Security Center aporta ventajas de seguridad a los clústeres de AKS gracias a los datos recopilados por el nodo principal de AKS. Algunos de los datos del entorno de Kubernetes que examina Azure Security Center pueden contener información confidencial.


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las características de seguridad de los contenedores de Security Center, consulte:

* [Seguridad de los contenedores y Azure Security Center](container-security.md)

* [Integración con Azure Container Registry](azure-container-registry-integration.md)

* [Administración de datos en Microsoft](https://www.microsoft.com/trust-center/privacy/data-management): Aquí se describen las directivas de datos de los servicios de Microsoft (incluidos Azure, Intune y Microsoft 365), los detalles de la administración de datos de Microsoft y las directivas de retención que afectan a los datos.
