---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186262"
---
Después de crear el agente de escucha del grupo de disponibilidad, puede que sea necesario ajustar los parámetros de clúster RegisterAllProvidersIP y HostRecordTTL para el recurso del agente de escucha. Estos parámetros pueden reducir el tiempo de reconexión tras una conmutación por error que puede evitar los tiempos de expiración de la conexión. Para más información sobre estos parámetros, así como código de ejemplo, consulte [Crear o configurar un agente de escucha del grupo de disponibilidad](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

