---
title: Introducción a la comprobación de flujo de IP de Azure Network Watcher | Microsoft Docs
description: En esta página se proporciona una información general sobre la funcionalidad de la comprobación de flujo de IP de Network Watcher
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: damendo
ms.openlocfilehash: 69aca5e0901a0da8aa98fe310ac220898bf650b2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76845013"
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Introducción a la comprobación de flujo de IP de Azure Network Watcher

La comprobación de flujo de IP permite comprobar si se permite o deniega un paquete hacia o desde una máquina virtual. La información está formada por la dirección, el protocolo, la dirección IP local, la dirección IP remota, el puerto local y el puerto remoto. Si un grupo de seguridad deniega un paquete, se devuelve el nombre de la regla que lo deniega. Aunque se puede elegir cualquier dirección IP de origen o destino, la comprobación de flujo de IP ayuda a los administradores a diagnosticar rápidamente problemas de conectividad desde o hacia Internet y desde o hacia el entorno local.

La comprobación del flujo de IP examina las reglas de todos los grupos de seguridad de red (NSG) aplicadas a la interfaz de red, como una subred o una NIC de máquina virtual. El flujo de tráfico hacia o desde esa interfaz de red se comprueba en función de los valores configurados. La comprobación de flujo de IP es útil para confirmar si una regla de un grupo de seguridad de red está bloqueando el tráfico de entrada o salida de una máquina virtual.

Es necesario crear una instancia de Network Watcher en todas las regiones en las que planea ejecutar la comprobación de flujo de IP. Network Watcher es un servicio regional y solo se puede ejecutar con recursos de la misma región. La instancia usada no afecta a los resultados de la comprobación del flujo de IP, ya que se siguen devolviendo las rutas asociadas con la NIC o la subred.

![1][1]

## <a name="next-steps"></a>Pasos siguientes

Visite el siguiente artículo para obtener información sobre si un paquete está permitido o no para una máquina virtual específica a través del portal. [Compruebe si se permite el tráfico en una máquina virtual con comprobación de flujo de IP mediante el portal](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png

