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
ms.openlocfilehash: aaa97c2cb062f30b1260ec7f80f85a3caccf932f
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152323"
---
# <a name="use-service-tags-for-azure-signalr-service"></a>Uso de etiquetas de servicio para Azure SignalR Service

Puede usar [etiquetas de servicio](../virtual-network/network-security-groups-overview.md#service-tags) para Azure SignalR Service al configurar [Grupo de seguridad de red](../virtual-network/network-security-groups-overview.md#network-security-groups). Permite definir una regla de seguridad de red de salida para los puntos de conexión de Azure SignalR Service sin necesidad de codificar direcciones IP.

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

- [Grupos de seguridad de red: etiquetas de servicio](../virtual-network/network-security-groups-overview.md#security-rules)