---
title: Conexión de datos de Azure Security Center a Azure Sentinel
description: Aprenda a conectar datos de Azure Security Center a Azure Sentinel.
author: yelevin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: e6e24a97a8b98bdd5447295880811914100563fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588542"
---
# <a name="connect-data-from-azure-security-center"></a>Conectar datos de Azure Security Center





Azure Sentinel permite conectar alertas de [Azure Security Center](../security-center/security-center-intro.md) y transmitirlos a Azure Sentinel. 

## <a name="prerequisites"></a>Prerequisites

- Para exportar alertas desde Azure Security Center, debe tener el rol de lector de seguridad en la suscripción de los registros que transmita.

- En la suscripción debe ejecutarse el [nivel estándar de Azure Security Center](../security-center/security-center-pricing.md). Si no es así, [actualícela al nivel estándar](https://azure.microsoft.com/pricing/details/security-center/).



## <a name="connect-to-azure-security-center"></a>Conectar a Azure Security Center

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Security Center**.

1. A la derecha, haga clic en **Connect** (Conectar) junto a cada suscripción cuyas alertas quiera transmitir a Azure Sentinel. Asegúrese de actualizar cada suscripción al nivel estándar de Azure Security Center para poder transmitir alertas a Azure Sentinel.

1. Puede seleccionar si desea que las alertas de Azure Security Center generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes** seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Luego, puede editar esta regla en **Analytics** (Análisis) y **Active rules** (Reglas activas).

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure Security Center, busque **SecurityAlert**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Security Center a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
