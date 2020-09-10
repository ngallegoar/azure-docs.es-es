---
title: Novedades del agente de servidores habilitados para Azure Arc (versión preliminar)
description: En este artículo se incluyen las notas de la versión del agente de servidores habilitados para Azure Arc (versión preliminar). Muchos de los problemas resumidos incluyen vínculos para obtener detalles adicionales.
ms.topic: conceptual
ms.date: 08/31/2020
ms.openlocfilehash: 0f5060189d6f9cac620b49f414e0e27bed3e356e
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89236574"
---
# <a name="whats-new-with-azure-arc-enabled-servers-preview-agent"></a>Novedades del agente de servidores habilitados para Azure Arc (versión preliminar)

El agente de Connected Machine de los servidores habilitados para Azure Arc (versión preliminar) se mejora de manera continua. Para mantenerse al día de los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores

## <a name="august-2020"></a>Agosto de 2020

Versión: 0,11

- Compatibilidad con Ubuntu 20.04.

- Mejoras de la confiabilidad para las implementaciones de extensiones.

### <a name="known-issues"></a>Problemas conocidos

Si usa una versión anterior del agente de Linux y la ha configurado para usar un servidor proxy, debe volver a configurar los valores del servidor proxy después de la actualización. Para ello, ejecute `sudo azcmagent_proxy add http://proxyserver.local:83`.

## <a name="next-steps"></a>Pasos siguientes

Antes de evaluar o habilitar los servidores habilitados para Arc (versión preliminar) en varias máquinas híbridas, consulte el artículo [Introducción al agente de Connected Machine ](agent-overview.md) para conocer los requisitos, los detalles técnicos sobre el agente y los métodos de implementación.