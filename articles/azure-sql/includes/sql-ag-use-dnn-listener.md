---
title: Use un cliente de escucha de nombre de red distribuida (DNN) en lugar de un cliente de escucha de VNN para los grupos de disponibilidad en máquinas virtuales con SQL Server.
description: Mensaje para indicar a los clientes que usen el cliente de escucha de DNN en lugar del cliente de escucha de VNN.
ms.topic: include
author: MashaMSFT
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: b4ea8428ae23537cde584bf6944732f65ebd2f26
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92168006"
---
> [!NOTE]
> Los clientes de SQL Server 2019 CU8 y versiones posteriores en Windows 2016 y versiones posteriores pueden reemplazar el cliente de escucha de VNN tradicional y Azure Load Balancer con un [cliente de escucha de nombre de red distribuida (DNN)](../virtual-machines/windows/availability-group-distributed-network-name-dnn-listener-configure.md) en su lugar. Omita el resto de los pasos de este artículo que crean el cliente de escucha y el equilibrador de carga.
