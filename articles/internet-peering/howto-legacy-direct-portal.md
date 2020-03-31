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
ms.openlocfilehash: 9900414d38bd597d08a80d15e908228c06ce06ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773899"
---
# <a name="convert-a-legacy-direct-peering-to-azure-resource-using-the-portal"></a>Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal

En este artículo se describe cómo convertir un emparejamiento directo heredado existente en un recurso de Azure mediante el portal.

Si lo prefiere, puede completar esta guía con [PowerShell](howto-legacy-direct-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise la sección [Requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento directo](walkthrough-direct-all.md) antes de comenzar la configuración.


## <a name="convert-legacy-direct-peering-to-azure-resource"></a>Conversión de un emparejamiento directo heredado en un recurso de Azure

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="convert-legacy-direct-peering"></a><a name=create></a>Conversión de un emparejamiento directo heredado

Puede convertir conexiones de emparejamiento heredadas mediante un recurso de **emparejamiento**.

#### <a name="launch-resource-and-configure-basic-settings"></a>Inicio de un recurso y configuración de las opciones básicas
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration-legacy.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Verificación de un emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información, consulte las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
