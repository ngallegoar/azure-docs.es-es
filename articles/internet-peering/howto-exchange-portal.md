---
title: Creación o modificación de un emparejamiento de Exchange mediante el portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento de Exchange mediante el portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 2c186decf68d167ab2c5ab7696c2dfb51d77a071
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773751"
---
# <a name="create-or-modify-an-exchange-peering-using-the-portal"></a>Creación o modificación de un emparejamiento de Exchange mediante el portal

En este artículo se describe cómo crear un emparejamiento de Microsoft Exchange mediante el portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía con [PowerShell](howto-exchange-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise la sección [Requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md) antes de comenzar la configuración.
* Si ya dispone de un emparejamiento de Exchange con Microsoft, que no se convierte en un recurso de Azure, consulte el artículo [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Creación y aprovisionamiento de un emparejamiento de Exchange

### <a name="sign-in-to-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Creación de un emparejamiento de Exchange

Puede crear una nueva solicitud de emparejamiento mediante el recurso **Emparejamiento**.

#### <a name="launch-resource-and-configure-basic-settings"></a>Inicio de un recurso y configuración de las opciones básicas
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name="verify-an-exchange-peering"></a><a name=get></a>Comprobación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Modificación de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Desaprovisionamiento de un emparejamiento de Exchange
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
* [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información, consulte las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).
