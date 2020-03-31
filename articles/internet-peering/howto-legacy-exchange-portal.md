---
title: Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal
titleSuffix: Azure
description: Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: a40c7bbc9f37135814b7bba3396d368faf97a166
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773943"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-the-portal"></a>Conversión de un emparejamiento de Exchange heredado a un recurso de Azure mediante el portal

En este artículo se describe cómo convertir un emparejamiento de Exchange heredado existente en un recurso de Azure mediante el portal.

Si lo prefiere, puede completar esta guía con [PowerShell](howto-legacy-exchange-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise la sección [Requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md) antes de comenzar la configuración.

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Conversión de un emparejamiento de Exchange heredado en un recurso de Azure

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-exchange-peering"></a><a name=create></a>Conversión del emparejamiento de Exchange heredado

Puede convertir conexiones de emparejamiento heredadas mediante un recurso de **emparejamiento**.

#### <a name="launch-resource-and-configure-basic-settings"></a>Inicio de un recurso y configuración de las opciones básicas
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration-legacy.md)]

### <a name="verify-exchange-peering"></a><a name=get></a>Comprobación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información, consulte las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
