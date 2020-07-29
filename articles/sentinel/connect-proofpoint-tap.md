---
title: Conexión de los datos de Proofpoint Targeted Attack Protection (TAP) a Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de Proofpoint Targeted Attack Protection (TAP) a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: e4185de879fa6136531e6d4c64908befa1d3bc45
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528193"
---
# <a name="connect-your-proofpoint-tap-to-azure-sentinel-with-azure-function"></a>Conexión de Proofpoint TAP a Azure Sentinel con Azure Functions

El conector de Proofpoint Targeted Attack Protection (TAP) permite conectar fácilmente todos los registros de la solución de seguridad de [Proofpoint TAP](https://www.proofpoint.com/us/products/advanced-threat-protection/targeted-attack-protection) con Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación. La integración entre Proofpoint TAP y Azure Sentinel usa Azure Functions para extraer datos de registro mediante la API REST.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-proofpoint-tap"></a>Configuración y conexión de Proofpoint TAP

Azure Functions puede integrar y extraer eventos y registros directamente de Proofpoint TAP y reenviarlos a Azure Sentinel.

1. En el portal de Azure Sentinel, haga clic en **Conectores de datos** y seleccione el conector **Proofpoint TAP**.

1. Seleccione **Open connector page** (Abrir página del conector).

1. Siga las instrucciones de la página **Proofpoint TAP**.

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida correctamente una conexión, los datos aparecen en Log Analytics en las tablas **ProofpointTAPMessagesBlocked_CL**, **ProofpointTAPMessagesDelivered_CL**, **ProofpointTAPClicksPermitted_CL** y **ProofpointTAPClicksBlocked_CL**.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar Proofpoint TAP a Azure Sentinel con Azure Function Apps. Para más información sobre Azure Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
