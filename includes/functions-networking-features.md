---
ms.openlocfilehash: 8d5c3fff2646536ffe5c593e13ccdab23397d44b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84906737"
---


| Característica |[Plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan)|[Plan Premium](../articles/azure-functions/functions-scale.md#premium-plan)|[Plan dedicado](../articles/azure-functions/functions-scale.md#app-service-plan)|[Azure App Service Environment](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Restricciones de IP de entrada y acceso al sitio privado](../articles/azure-functions/functions-networking-options.md#inbound-ip-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|✅Sí|
|[Integración de redes virtuales](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Sí (regional)|✅Sí (regional y puerta de enlace)|✅Sí| ✅Sí|
|[Desencadenadores de red virtual (no HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Sí |✅Sí|✅Sí|✅Sí|
|[Conexiones híbridas](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (solo Windows)|❌No|✅Sí|✅Sí|✅Sí|✅Sí|
|[Restricciones de IP de salida](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Sí|✅Sí|✅Sí|✅Sí|