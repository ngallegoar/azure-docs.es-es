---
title: 'Escenario: Azure Firewall: enrutamiento entre varios centros'
titleSuffix: Azure Virtual WAN
description: 'Escenarios para el enrutamiento: enrutamiento entre varios centros que tienen Azure Firewall'
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 1057dc28682c90374b2f8e8e0297f3d769c08bd6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568052"
---
# <a name="scenario-azure-firewall---interhub-preview"></a>Escenario: Azure Firewall: Interhub (versión preliminar)

Al trabajar con el enrutamiento de centros virtuales de Virtual WAN, hay bastantes escenarios disponibles. En este escenario, el objetivo es enrutar entre varios centros que tienen Azure Firewall. Para obtener información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Arquitectura del escenario

En este escenario, asumimos la siguiente configuración:

* Tiene varios centros con Azure Firewall en cada centro.
* Está usando un centro virtual seguro.
* Se han configurado las directivas adecuadas para el tráfico privado (VNet), Internet y ramas.
* De red virtual a Internet (V2I), de red virtual a rama (V2B), de rama a red virtual (B2V), todos pasan por Azure Firewall en cada centro.

Consideraciones adicionales:

* En el caso de un escenario entre centros con Azure Firewall, se supone que las redes virtuales de radios no se asocian a tablas de rutas independientes. (por ejemplo **Red virtual 1** asociada a **Tabla de rutas A** y **Red virtual 2** asociada a **Tabla de rutas B**). Todas las redes virtuales se asocian a la misma tabla de rutas en la que residen las rutas para Azure Firewall.
* La protección a través de Azure Firewall está limitada a **Rama < > Red virtual** y al tráfico solo de **Internet**. De momento no se admite el flujo de rama a rama a través de Azure Firewall.

**Ilustración 1**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/architecture.png" alt-text="Arquitectura"::::

## <a name="scenario-workflow"></a><a name="workflow"></a>Flujo de trabajo del escenario

Para poder configurar este escenario, tenga en cuenta los siguientes pasos:

### <a name="step-1"></a><a name="step-1"></a>Paso 1.

Suponiendo que ya ha protegido las conexiones mediante Azure Firewall Manager, el primer paso es asegurarse de que todas las conexiones de red virtual y de rama propagan **Ninguna**. Esto es necesario para garantizar que el tráfico se establezca mediante Azure Firewall.

1. Seleccione el centro y edite la tabla de rutas **Ninguna**.
1. Actualice la **propagación** para seleccionar todas las ramas y todas las redes virtuales.

### <a name="step-2"></a><a name="step-2"></a> Paso 2

Configure una tabla de rutas personalizada por centro.

1. En el caso de **Centro 1**, cree la tabla de rutas **RT_Hub1**.

    * Si usó Azure Firewall Manager, crea automáticamente una ruta para 0.0.0.0/0 con el próximo salto **AZFW1** en la tabla de rutas predeterminada del centro. Esta configuración se modificará en el paso 3. Para **RT_Hub1**, cree una entrada para 0.0.0.0/0 explícitamente con el próximo salto **AZFW1**. Esta configuración activará V2V, B2V y V2I a través de AZFW1.
    * Dado que desea que se alcancen las ramas y las redes virtuales de **Centro 2** a través de **AZFW2** desde **Centro 1** (en lugar de a través de AZFW1), debe agregar otras dos rutas agregadas para las ramas de **Centro 2** (10.5.0.0/16-> AZFW2) y redes virtuales (10.2.0.0/16-> AZFW2).

1. En **Centro 2**, cree una tabla de rutas **RT_Hub2**.

    * Si usó Azure Firewall Manager, crea automáticamente una ruta para 0.0.0.0/0 con el próximo salto **AZFW2** en la tabla de rutas predeterminada del centro. Esta configuración se modificará en el paso 3. Para **RT_Hub2**, cree una entrada para 0.0.0.0/0 explícitamente con el próximo salto **AZFW2**. Esta configuración activará V2V, B2V y V2I a través de **AZFW2**.
    * Dado que desea que el tráfico entre centros esté protegido por **AZFW2**de Hub 2, no es necesario agregar explícitamente un paso similar a la segunda viñeta en el anterior paso del concentrador 1.

### <a name="step-3"></a><a name="step-3"></a>Paso 3

Modifique la **tabla de rutas predeterminada** en cada uno de los centros para agregar una ruta estática al flujo **rama a red virtual** (B2V) y al flujo **rama a Internet** (B2I) a través de Azure Firewall. De momento no se admite el flujo de rama a rama a través de Azure Firewall.

1. Para la **tabla de rutas predeterminada del centro 1**:

    * **Rama a ramas del centro 2 a través de AZFW2**: 10.5.0.0/16-> AZFW2. Esta configuración configura una ruta para el firewall del centro 2.
    * **Rama a redes virtuales del centro 2 a través de AZFW2**: 10.2.0.0/16-> AZFW2. Esta configuración configura una ruta para el firewall del centro 2.
    * **Rama a rama (local)/rama a red virtual (local)/rama a Internet**: 0.0.0.0/0-> AZFW1.

2. Para la **tabla de rutas predeterminada del centro 2**:

    * Rama a rama (local y remota)/rama a red virtual (local y remota)/rama a Internet: 0.0.0.0/0-> AZFW2.

Esto hará que la configuración de enrutamiento cambie como se muestra en la figura siguiente.

**Ilustración 2**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/workflow.png" alt-text="workflow":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).
* Para más información sobre cómo configurar el enrutamiento de centros virtuales, consulte [Configuración del enrutamiento de centro virtual](how-to-virtual-hub-routing.md).
