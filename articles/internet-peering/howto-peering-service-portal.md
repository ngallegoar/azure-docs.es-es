---
title: Habilitación de Azure Peering Service en un emparejamiento directo mediante Azure Portal
titleSuffix: Azure
description: Habilitación de Azure Peering Service en un emparejamiento directo mediante Azure Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: 625a17e5acff00f78c5a19725653eec629936f87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687042"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-the-azure-portal"></a>Habilitación de Azure Peering Service en un emparejamiento directo mediante Azure Portal

En este artículo se describe cómo habilitar [Peering Service](overview-peering-service.md) en un emparejamiento directo mediante Azure Portal.

Si lo prefiere, puede completar esta guía mediante [PowerShell](howto-peering-service-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) antes de comenzar la configuración.
* Elija un emparejamiento directo en la suscripción en la que quiera habilitar Peering Service. Si no tiene una, convierta un emparejamiento directo heredado o cree uno.
    * Para convertir un emparejamiento directo heredado, siga las instrucciones que se indican en [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md).
    * Para crear un emparejamiento directo, siga las instrucciones que se indican en [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md).

## <a name="enable-peering-service-on-a-direct-peering"></a>Habilitación de Peering Service en un emparejamiento directo

### <a name="view-direct-peering"></a><a name= get></a>Visualización de un emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Habilitación del emparejamiento directo para Peering Service

Después de abrir un emparejamiento directo en el paso anterior, habilítelo para Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Modificación de una conexión de emparejamiento directo

Para modificar la configuración de la conexión, consulte la sección "Modificación de un emparejamiento directo" del artículo [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Puede consultar las preguntas frecuentes en [Preguntas más frecuentes sobre Peering Service](service-faqs.md).