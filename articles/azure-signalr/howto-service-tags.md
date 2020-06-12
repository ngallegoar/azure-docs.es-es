---
title: Uso de etiquetas de servicio
titleSuffix: Azure SignalR Service
description: Uso de etiquetas de servicio para permitir el tráfico saliente a Azure SignalR Service
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: article
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 8810309fef5dbbb35465a2af15d42fa8a59d5401
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84302107"
---
# <a name="use-service-tags-for-azure-signalr-service"></a>Uso de etiquetas de servicio para Azure SignalR Service

Puede usar [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) para Azure SignalR Service al configurar [Grupo de seguridad de red](../virtual-network/security-overview.md#network-security-groups). Permite definir una regla de seguridad de red de salida para los puntos de conexión de Azure SignalR Service sin necesidad de codificar direcciones IP.

Azure SignalR Service administra estas etiquetas de servicio. No puede crear su propia etiqueta de servicio ni modificar una etiqueta existente. Microsoft administra estos prefijos de direcciones para que coincidan con la etiqueta de servicio y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

## <a name="use-service-tag-on-portal"></a>Uso de la etiqueta de servicio en el portal

Puede permitir el tráfico saliente a Azure SignalR Service si agrega una nueva regla de seguridad de red de salida:

1. Vaya al grupo de seguridad de red.

1. Haga clic en el menú configuración denominado **Reglas de seguridad de salida**.

1. Haga clic en el botón **+ Agregar** en la parte superior.

1. Elija **Etiqueta de servicio** en **Destino**.

1. Elija **AzureSignalR** en **Etiqueta de servicio de destino**.

1. Rellene **443** en **Intervalos de puertos de destino**.

    ![Creación de una regla de seguridad de salida](media/howto-service-tags/portal-add-outbound-security-rule.png)

1. Ajuste otros campos según sus necesidades.

1. Haga clic en **Agregar**.


## <a name="next-steps"></a>Pasos siguientes

- [Grupos de seguridad de red: etiquetas de servicio](../virtual-network/security-overview.md#security-rules)
