---
ms.openlocfilehash: 82571d1a0e651f638dec29184f0ecdc88562b3ad
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96021012"
---


| Característica |[Plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan)|[Plan Premium](../articles/azure-functions/functions-scale.md#premium-plan)|[Plan dedicado](../articles/azure-functions/functions-scale.md#app-service-plan)|[Azure App Service Environment](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Restricciones de IP de entrada y acceso al sitio privado](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|✅Sí|
|[Integración de redes virtuales](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Sí (regional)|✅Sí (regional y puerta de enlace)|✅Sí| ✅Sí|
|[Desencadenadores de red virtual (no HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Sí |✅Sí|✅Sí|✅Sí|
|[Conexiones híbridas](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (solo Windows)|❌No|✅Sí|✅Sí|✅Sí|✅Sí|
|[Restricciones de IP de salida](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Sí|✅Sí|✅Sí|✅Sí|