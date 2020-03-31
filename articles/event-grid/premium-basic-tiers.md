---
title: Niveles Premium y Básico de Azure Event Grid
description: En este artículo se describe la diferencia entre los niveles Premium y Básico de Azure Event Grid y cuándo usar cada uno.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: overview
ms.date: 03/11/2020
ms.author: babanisa
ms.openlocfilehash: 7ae73337ccccc2b7d2402cfa2a54ae93552d2465
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79300339"
---
# <a name="azure-event-grid-premium-and-basic-tiers"></a>Niveles Premium y Básico de Azure Event Grid
Azure Event Grid tiene dos niveles: **Premium** y **Básico**. El nivel Básico usa precios de pago por uso o consumo. Proporciona todas las herramientas de publicación y suscripción básicas que necesita para usar Event Grid para modelos de programación orientados a eventos. El nivel Premium va un paso más allá con características de seguridad orientadas a la empresa. El nivel Premium se encuentra en una **versión preliminar** temprana con muchas de sus características todavía en desarrollo.

## <a name="overview"></a>Información general
Todos los temas y dominios personalizados de Event Grid pertenecen al nivel Básico o al nivel Premium. A partir de la versión `2020-04-01-preview` de la API, puede elegir el nivel deseado como parte de la creación de un tema o un dominio. El valor predeterminado es el nivel Básico. Los temas y dominios creados con versiones anteriores de la API tienen el nivel Básico como predeterminado. Para cambiar el plan de tarifa de los temas y los dominios, consulte [Cómo actualizar el nivel de temas y dominios](update-tier.md).

## <a name="capabilities-and-features"></a>Características y funcionalidades

En la siguiente tabla se describen las diferencias entre los niveles:

|       &nbsp;                                           | Básica           | Premium        |
| ------------------------------------------------------ | --------------- | -------------- |
| Reglas de firewall de IP para entrada                          | Vista previa  | Vista previa |
| Etiquetas de servicio para salida                                | Vista previa  | Vista previa |
| Integración de red virtual de punto de conexión privado en la entrada          | No disponible   | Vista previa |

## <a name="availability"></a>Disponibilidad
Durante la versión preliminar inicial, los temas y los dominios de nivel Premium con integración de punto de conexión privado están disponibles en las siguientes regiones:

- Este de EE. UU.
- Oeste de EE. UU. 2
- Centro-sur de EE. UU.

## <a name="pricing-and-quotas"></a>Precios y cuotas
Consulte [Precios de Event Grid](https://azure.microsoft.com/pricing/details/event-grid/) para obtener detalles de los precios de uso del nivel Básico. Aún no se ha anunciado el precio del nivel Premium, que es gratis hasta que dicho precio se haga público.

Las cuotas existentes para el número de temas y dominios y el rendimiento se aplican a los recursos de nivel Premium y Básico hasta que se anuncie el precio del nivel Premium.

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- Para actualizar del nivel Básico al nivel Premium, consulte el artículo [Actualización del plan de tarifa](update-tier.md). 
- Puede configurar el firewall de IP para el recurso de Event Grid con el fin de restringir el acceso a través de Internet público desde únicamente un conjunto seleccionado de direcciones IP o intervalos de direcciones IP. Para obtener instrucciones paso a paso, consulte [Configuración del firewall](configure-firewall.md).
- Puede configurar puntos de conexión privados para restringir el acceso solo desde redes virtuales seleccionadas. Para obtener instrucciones paso a paso, consulte [Configuración de puntos de conexión privados](configure-private-endpoints.md).