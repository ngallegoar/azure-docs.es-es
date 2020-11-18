---
ms.openlocfilehash: 82571d1a0e651f638dec29184f0ecdc88562b3ad
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578848"
---


| Característica |[Plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan)|[Plan Premium](../articles/azure-functions/functions-scale.md#premium-plan)|[Plan dedicado](../articles/azure-functions/functions-scale.md#app-service-plan)|[Azure App Service Environment](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Restricciones de IP de entrada y acceso al sitio privado](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|✅Sí|
|[Integración de redes virtuales](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Sí (regional)|✅Sí (regional y puerta de enlace)|✅Sí| ✅Sí|
|[Desencadenadores de red virtual (no HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Sí |✅Sí|✅Sí|✅Sí|
|[Conexiones híbridas](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (solo Windows)|❌No|✅Sí|✅Sí|✅Sí|✅Sí|
|[Restricciones de IP de salida](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Sí|✅Sí|✅Sí|✅Sí|