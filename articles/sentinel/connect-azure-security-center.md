---
title: Conexión de datos de Azure Security Center a Azure Sentinel
description: Descubra cómo conectar alertas desde el nivel estándar de Azure Security Center (ASC) y transmitirlas a Azure Sentinel.
author: yelevin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 2fc7744600a9652ad43fd0aae8d886dc94acd58f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559157"
---
# <a name="connect-data-from-azure-security-center-asc"></a>Conectar datos de Azure Security Center (ASC)

Azure Sentinel permite conectar alertas de [Azure Security Center](../security-center/security-center-intro.md) y transmitirlos a Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Para exportar alertas desde Azure Security Center, debe tener el rol de lector de seguridad en la suscripción de los registros que transmita.

- En la suscripción debe ejecutarse el [nivel estándar de Azure Security Center](../security-center/security-center-pricing.md). Si no es así, [actualícela al nivel estándar](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="connect-to-azure-security-center"></a>Conectar a Azure Security Center

1. En el menú de navegación de Azure Sentinel, seleccione **Conectores de datos**.

1. En la galería de conectores de datos, seleccione **Azure Security Center** y, a continuación, haga clic en el botón **Abrir la página del conector**.

1. En **Configuration** (Configuración), haga clic en **Connect** (Conectar) junto a cada suscripción cuyas alertas quiera transmitir a Azure Sentinel. El botón Conectar solo estará disponible si tiene los permisos necesarios y la suscripción de nivel estándar de ASC.

1. Puede seleccionar si quiere que las alertas de Azure Security Center generen incidentes en Azure Sentinel. En **Create incidents** (Crear incidentes), seleccione **Enabled** (Habilitado) para activar la regla de análisis predeterminada que crea automáticamente incidentes a partir de alertas. Luego, puede editar esta regla en **Analytics** (Análisis), en la pestaña **Active rules** (Reglas activas).

1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure Security Center, busque **SecurityAlert**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Security Center a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
