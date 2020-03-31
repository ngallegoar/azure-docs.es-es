---
title: Azure Monitor para las asignaciones de región de contenedores
description: En este artículo se describen las asignaciones de región compatibles entre Azure Monitor para contenedores, el área de trabajo de Log Analytics y las métricas personalizadas.
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: a058f9cac987bb5c7130019f50370c6a176b09ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75403421"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Asignaciones de región compatibles con Azure Monitor para contenedores

 Cuando se habilita Azure Monitor para los contenedores, solo ciertas regiones son compatibles para vincular un área de trabajo de Log Analytics y un clúster de AKS y recopilar las métricas personalizadas enviadas a Azure Monitor.

## <a name="log-analytics-workspace-supported-mappings"></a>Asignaciones admitidas por el área de trabajo de Log Analytics

Los recursos de clúster de AKS o el área de trabajo de Log Analytics pueden encontrarse en otras regiones. En la tabla siguiente se muestran las asignaciones.

|**Región de clúster de AKS** | **Región del área de trabajo de Log Analytics** |
|-----------------------|------------------------------------|
|**África** | |
|SouthAfricaNorth |Oeste de Europa |
|SouthAfricaWest |Oeste de Europa |
|**Australia** | |
|AustraliaEast |AustraliaEast |
|AustraliaCentral |AustraliaCentral |
|AustraliaCentral2 |AustraliaCentral |
|AustraliaEast |AustraliaEast |
|**Asia Pacífico** | |
|EastAsia |EastAsia |
|SoutheastAsia |SoutheastAsia |
|**Brasil** | |
|BrazilSouth | SouthCentralUS |
|**Canadá** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Europa** | |
|FranceCentral |FranceCentral |
|FranceSouth |FranceCentral |
|Norte de Europa |Norte de Europa |
|UKSouth |UKSouth |
|UKWest |UKSouth |
|Oeste de Europa |Oeste de Europa |
|**India** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japón** | |
|JapanEast |JapanEast |
|JapanWest |JapanEast |
|**Corea** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**EE. UU.** | |
|CentralUS |CentralUS|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|WestUS |WestUS |
|WestUS2 |WestUS2 |
|WestCentralUS<sup>1</sup>|EastUS<sup>1</sup>|
|US Gov - Virginia |US Gov - Virginia |

<sup>1</sup> Debido a restricciones de capacidad, la región no está disponible cuando se crean recursos nuevos. Esto incluye un área de trabajo de Log Analytics. Sin embargo, los recursos vinculados preexistentes en la región deberían seguir funcionando.

## <a name="custom-metrics-supported-regions"></a>Regiones compatibles con las métricas personalizadas

Las métricas recopiladas de los pods y los nodos de clústeres de Azure Kubernetes Service (AKS) se admiten para publicar como métricas personalizadas solo en las [regiones de Azure](../platform/metrics-custom-overview.md#supported-regions) siguientes.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a supervisar el clúster de AKS, revise [Cómo habilitar Azure Monitor para contenedores](container-insights-onboard.md), a fin de conocer los requisitos y los métodos disponibles para habilitar la supervisión.  
