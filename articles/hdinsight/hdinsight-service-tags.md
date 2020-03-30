---
title: Etiquetas de servicio del grupo de seguridad de red (NSG) para Azure HDInsight
description: Use etiquetas de servicio de HDInsight para permitir el tráfico entrante al clúster desde los nodos de servicios de mantenimiento y administración de HDInsight, sin agregar explícitamente direcciones IP a los grupos de seguridad de red.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/10/2020
ms.openlocfilehash: a72753d5553e79a8ed28c3afcc7e54af6c2d230c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79117235"
---
# <a name="network-security-group-nsg-service-tags-for-azure-hdinsight"></a>Etiquetas de servicio del grupo de seguridad de red (NSG) para Azure HDInsight

Las etiquetas de servicio de HDInsight de los grupos de seguridad de red (NSG) son grupos de direcciones IP para los servicios de mantenimiento y administración. Estos grupos ayudan a minimizar la complejidad de la creación de reglas de seguridad. Las [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) ofrecen un método alternativo que permite el tráfico entrante desde direcciones IP específicas sin especificar cada una de las [direcciones IP de administración](hdinsight-management-ip-addresses.md) en los grupos de seguridad de red.

El servicio HDInsight crea y administra estas etiquetas de servicio. No puede crear su propia etiqueta de servicio ni modificar una etiqueta existente. Microsoft administra los prefijos de direcciones que coinciden con la etiqueta de servicio y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

## <a name="getting-started-with-service-tags"></a>Introducción a las etiquetas de servicio

Tiene dos opciones para usar etiquetas de servicio en los grupos de seguridad de red:

1. Usar una sola etiqueta de servicio de HDInsight: esta opción abrirá la red virtual en todas las direcciones IP que usa el servicio HDInsight para supervisar los clústeres en todas las regiones. Esta opción es el método más sencillo, pero puede no ser adecuada si tiene requisitos de seguridad restrictivos.

1. Usar varias etiquetas de servicio regional: esta opción abrirá la red virtual solo para las direcciones IP que HDInsight usa en esa región específica. No obstante, si usa varias regiones, tendrá que agregar varias etiquetas de servicio a la red virtual.

## <a name="use-a-single-global-hdinsight-service-tag"></a>Usar una sola etiqueta de servicio de HDInsight global

La manera más fácil de empezar a usar etiquetas de servicio con el clúster de HDInsight es agregar la etiqueta global `HDInsight` a una regla de grupo de seguridad de red.

1. Desde [Azure Portal](https://portal.azure.com/), seleccione su grupo de seguridad de red.

1. En **Configuración**, seleccione **Reglas de seguridad de entrada** y, a continuación, seleccione **+ Agregar**.

1. En la lista desplegable **Origen**, seleccione **Etiqueta de servicio**.

1. En la lista desplegable **Etiqueta de servicio de origen**, seleccione **HDInsight**.

    ![Etiqueta de servicio Agregar de Azure Portal](./media/hdinsight-service-tags/azure-portal-add-service-tag.png)

Esta etiqueta contiene las direcciones IP de los servicios de mantenimiento y administración de todas las regiones donde está disponible HDInsight y garantiza que el clúster pueda comunicarse con los servicios de mantenimiento y administración necesarios, independientemente de dónde se cree.

## <a name="use-regional-hdinsight-service-tags"></a>Uso de etiquetas de servicio de HDInsight regionales

Si la primera opción no funcionase porque se necesitan permisos más restrictivos, solo podrá permitir las etiquetas de servicio aplicables a su región. Las etiquetas de servicio aplicables pueden ser una, dos o tres etiquetas de servicio, en función de la región en la que se cree el clúster.

Para averiguar qué etiquetas de servicio agregar a su región, lea las siguientes secciones del documento.

### <a name="use-a-single-regional-service-tag"></a>Uso de una sola etiqueta de servicio regional

Si prefiere la segunda opción de etiqueta de servicio y el clúster se encuentra en una de las regiones enumeradas en esta tabla, solo tiene que agregar una etiqueta de servicio regional única al grupo de seguridad de red.

| Country | Region | Etiqueta de servicio |
| ---- | ---- | ---- |
| Australia | Este de Australia | HDInsight.AustraliaEast |
| &nbsp; | Sudeste de Australia | HDInsight.AustraliaSoutheast |
| &nbsp; | Centro de Australia | HDInsight.AustraliaCentral |
| China | Este de China 2 | HDInsight.ChinaEast2 |
| &nbsp; | Norte de China 2 | HDInsight.ChinaNorth2 |
| Estados Unidos | Centro-Norte de EE. UU | HDInsight.NorthCentralUS |
| &nbsp; | Oeste de EE. UU. 2 | HDInsight.WestUS2 |
| &nbsp; | Centro-Oeste de EE. UU. | HDInsight.WestCentralUS |
| Canadá | Este de Canadá | HDInsight.CanadaEast |
| Brasil | Sur de Brasil | HDInsight.BrazilSouth |
| Corea | Centro de Corea del Sur | HDInsight.KoreaCentral |
| &nbsp; | Corea del Sur | HDInsight.KoreaSouth |
| India | Centro de la India | HDInsight.CentralIndia |
| &nbsp; | Sur de la India | HDInsight.SouthIndia |
| Japón | Japón Occidental | HDInsight.JapanWest |
| Francia | Centro de Francia| HDInsight.FranceCentral |
| Reino Unido | Sur de Reino Unido 2 | HDInsight.UKSouth |
| Azure Government | Departamento de Defensa de centro de EE. UU.   | HDInsight.USDoDCentral |
| &nbsp; | USGov: Texas | HDInsight.USGovTexas |
| &nbsp; | Departamento de Defensa de este de EE. UU. | HDInsight.USDoDEast |
| &nbsp; | USGov: Arizona | HDInsight.USGovArizona |

### <a name="use-multiple-regional-service-tags"></a>Uso de varias etiquetas de servicio regional

Si prefiere la segunda opción de etiqueta de servicio, y la región donde se crea el clúster no se indicó anteriormente, debe permitir varias etiquetas de servicio regional. La necesidad de usar más de una es debido a las diferencias en la organización de los proveedores de recursos para las distintas regiones.

Las regiones restantes se dividen en grupos en función de las etiquetas de servicio regionales que usan.

#### <a name="group-1"></a>Grupo 1

Si el clúster se crea en una de las regiones de la tabla siguiente, permita las etiquetas de servicio `HDInsight.WestUS` y `HDInsight.EastUS` además de la etiqueta de servicio regional enumerada. Las regiones de esta sección requieren tres etiquetas de servicio.

Por ejemplo, si el clúster se crea en la región `East US 2`, tendrá que agregar las siguientes etiquetas de servicio al grupo de seguridad de red:

- `HDInsight.EastUS2`
- `HDInsight.WestUS`
- `HDInsight.EastUS`

| Country | Region | Etiqueta de servicio |
| ---- | ---- | ---- |
| Estados Unidos | Este de EE. UU. 2 | HDInsight.EastUS2 |
| &nbsp; | Centro de EE. UU. | HDInsight.CentralUS |
| &nbsp; | Centro-Norte de EE. UU. | HDInsight. NorthCentralUS |
| &nbsp; | Centro-sur de EE. UU. | HDInsight.SouthCentralUS |
| &nbsp; | Este de EE. UU. | HDInsight.EastUS |
| &nbsp; | Oeste de EE. UU. | HDInsight.WestUS |
| Japón | Japón Oriental | HDInsight.JapanEast |
| Europa | Norte de Europa | HDInsight.NorthEurope |
| &nbsp; | Oeste de Europa| HDInsight.WestEurope |
| Asia | Este de Asia | HDInsight.EastAsia |
| &nbsp; | Sudeste de Asia | HDInsight.SoutheastAsia |
| Australia | Este de Australia | HDInsight.AustraliaEast |

#### <a name="group-2"></a>Grupo 2

Los clústeres de las regiones de **Norte de China** y **Este de China**, deben permitir dos etiquetas de servicio: `HDInsight.ChinaNorth` y `HDInsight.ChinaEast`.

#### <a name="group-3"></a>Grupo 3

Los clústeres de las regiones de **US Gov Iowa** y **US Gov Virginia**, deben permitir dos etiquetas de servicio: `HDInsight.USGovIowa` y `HDInsight.USGovVirginia`.

#### <a name="group-4"></a>Grupo 4

Los clústeres de las regiones de **Centro de Alemania** y **Nordeste de Alemania**, deben permitir dos etiquetas de servicio: `HDInsight.GermanyCentral` y `HDInsight.GermanyNorthEast`.

## <a name="next-steps"></a>Pasos siguientes

- [Grupos de seguridad de red: etiquetas de servicio](../virtual-network/security-overview.md#security-rules)
- [Creación de redes virtuales para clústeres de Azure HDInsight](hdinsight-create-virtual-network.md)
