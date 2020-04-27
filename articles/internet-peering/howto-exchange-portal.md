---
title: Creación o modificación de un emparejamiento de Exchange mediante Azure Portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento de Exchange mediante Azure Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e4f2ee72cbe17c094567aab5c7cc4720b02cde68
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680931"
---
# <a name="create-or-modify-an-exchange-peering-by-using-the-azure-portal"></a>Creación o modificación de un emparejamiento de Exchange mediante Azure Portal

En este artículo se describe cómo crear un emparejamiento de Microsoft Exchange mediante Azure Portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía mediante [PowerShell](howto-exchange-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md) antes de comenzar la configuración.
* Si ya dispone de emparejamientos de Exchange con Microsoft que no se convierten en recursos de Azure, consulte el artículo [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Creación y aprovisionamiento de un emparejamiento de Exchange

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Creación de un emparejamiento de Exchange

Puede crear una nueva solicitud de emparejamiento mediante el recurso **Emparejamiento**.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Inicio de un recurso y configuración de las opciones básicas
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
