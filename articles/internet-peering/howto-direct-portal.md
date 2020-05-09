---
title: Creación o modificación de un emparejamiento directo mediante Azure Portal
titleSuffix: Azure
description: Creación o modificación de un emparejamiento directo mediante Azure Portal
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: dcd6aaf584691005dd071a7aba5958070f598978
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681055"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Creación o modificación de un emparejamiento directo mediante Azure Portal

En este artículo se describe cómo crear un emparejamiento directo de Microsoft mediante Azure Portal. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía mediante Azure [PowerShell](howto-direct-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Antes de comenzar la configuración, revise los [requisitos previos](prerequisites.md) y el [tutorial del emparejamiento directo](walkthrough-direct-all.md).
* Si ya dispone de conexiones de emparejamiento directo con Microsoft que no convierten en recursos de Azure, consulte [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Creación y aprovisionamiento de un emparejamiento directo

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Inicio de sesión en el portal y selección de su suscripción
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Creación de un emparejamiento directo

Puede crear una solicitud de emparejamiento mediante el recurso **Peering**.

#### <a name="launch-resource-and-configure-basic-settings"></a>Inicio de un recurso y configuración de las opciones básicas
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Configuración de conexiones y envío
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Comprobación del emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Modificación de un emparejamiento directo
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Desaprovisionamiento de un emparejamiento directo
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Para más información, consulte las [preguntas frecuentes sobre el emparejamiento de Internet](faqs.md).
