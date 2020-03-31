---
title: Mejora de la disponibilidad de la aplicación con Azure Advisor
description: Utilice Azure Advisor para mejorar la alta disponibilidad de las implementaciones de Azure.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 997681ed62fa9985e3122ece22565dbae0e65b53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443105"
---
# <a name="improve-availability-of-your-application-with-azure-advisor"></a>Mejora de la disponibilidad de la aplicación con Azure Advisor

Azure Advisor lo ayuda a garantizar y mejorar la continuidad de las aplicaciones empresariales críticas. Puede obtener recomendaciones sobre alta disponibilidad de Advisor en la pestaña **High Availability** (Alta disponibilidad) del panel de Advisor.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Garantía de la tolerancia a errores en máquinas virtuales

Para proporcionar redundancia a la aplicación, recomendamos que agrupe dos máquinas virtuales o más en un conjunto de disponibilidad. Advisor identifica las máquinas virtuales que no forman parte de un conjunto de disponibilidad y recomienda moverlas a un conjunto de disponibilidad. Esta configuración garantiza que, durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual esté disponible y cumpla el Acuerdo de Nivel de Servicio de máquina virtual de Azure. Puede elegir crear un conjunto de disponibilidad de la máquina virtual o agregar la máquina virtual a uno existente.

> [!NOTE]
> Si decide crear un conjunto de disponibilidad, debe agregar al menos una máquina virtual más a él. Se recomienda que agrupe dos o más máquinas virtuales en un conjunto de disponibilidad para garantizar que al menos una de ellas esté disponible durante una interrupción.

## <a name="ensure-availability-set-fault-tolerance"></a>Garantía de la tolerancia a errores del conjunto de disponibilidad

Para proporcionar redundancia a la aplicación, recomendamos que agrupe dos máquinas virtuales o más en un conjunto de disponibilidad. Advisor identifica conjuntos de disponibilidad que contienen una sola máquina virtual y recomienda que se agregue una o más máquinas virtuales a él. Esta configuración garantiza que, durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual esté disponible y cumpla el Acuerdo de Nivel de Servicio de máquina virtual de Azure. Puede elegir crear una máquina virtual o agregar una existente al conjunto de disponibilidad.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Uso de Managed Disks para mejorar la confiabilidad de los datos

Las máquinas virtuales que están en un conjunto de disponibilidad con discos que comparten las cuentas de almacenamiento o las unidades de escalado de almacenamiento no son resistentes a errores de unidad de escalado de almacenamiento único durante las interrupciones. Advisor identificará estos conjuntos de disponibilidad y recomendará migrar a Azure Managed Disks. Esto garantiza que los discos de las distintas máquinas virtuales del conjunto de disponibilidad estén lo suficientemente aislados entre sí como para evitar un único punto de error. 

## <a name="ensure-application-gateway-fault-tolerance"></a>Garantía de la tolerancia a errores de Application Gateway

Esta recomendación garantiza la continuidad empresarial de las aplicaciones críticas con tecnología de Application Gateway. Advisor identifica las instancias de Application Gateway que no están configuradas para la tolerancia a errores y recomienda acciones correctoras que puede llevar a cabo. Advisor identifica instancias únicas de Application Gateway medianas o grandes, y recomienda que agregue al menos una instancia más. También identifica instancias múltiples de Application Gateway pequeñas y recomienda migrar a SKU medianas o grandes. Advisor recomienda estas acciones para asegurarse de que las instancias de Application Gateway estén configuradas para satisfacer los requisitos actuales del Acuerdo de Nivel de Servicio para estos recursos.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Protección de los datos de máquinas virtuales contra la eliminación accidental

La configuración de la copia de seguridad de máquinas virtuales garantiza la disponibilidad de los datos críticos y ofrece protección contra la eliminación accidental o los daños. Advisor identifica las máquinas virtuales que no tienen la copia de seguridad habilitada y recomienda habilitarla. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Asegúrese de tener acceso a los expertos en la nube de Azure cuando lo necesite

Cuando se ejecuta una carga de trabajo crítica para la empresa, es importante tener acceso al soporte técnico cuando sea necesario. Advisor identifica posibles suscripciones críticas para la empresa que no tienen soporte técnico incluido en su plan de soporte técnico y recomienda actualizar a una opción que incluya soporte técnico.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Creación de alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten

Es recomendable configurar alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten. [Azure Service Health](https://azure.microsoft.com/features/service-health/) es un servicio gratuito que ofrece una guía personalizada y soporte técnico cuando experimenta un problema con algún servicio de Azure. Advisor identifica las suscripciones que no tienen alertas configuradas y recomienda crearlas.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Configuración de puntos de conexión de Traffic Manager para lograr resistencia

Los perfiles de Traffic Manager con más de un punto de conexión experimentan una mayor disponibilidad si se produce un error en cualquier punto de conexión determinado. Colocar puntos de conexión en diferentes regiones mejora aún más la confiabilidad del servicio. Advisor identifica los perfiles de Traffic Manager donde hay solo un punto de conexión y recomienda agregar al menos uno más en otra región.

Si todos los puntos de conexión de un perfil de Traffic Manager configurado para el enrutamiento de proximidad están en la misma región, los usuarios de otras regiones pueden experimentar retrasos en la conexión. Si agrega o mueve un punto de conexión a otra región, mejorará el rendimiento global y proporcionará una mejor disponibilidad en caso de que se produzcan errores en todos los puntos de conexión de una región. Advisor identifica los perfiles de Traffic Manager configurados para el enrutamiento de proximidad en los que todos los puntos de conexión están en la misma región. También recomienda agregar o mover un punto de conexión a otra región de Azure.

Si un perfil de Traffic Manager está configurado para el enrutamiento geográfico, el tráfico se enrutará a puntos de conexión en función de las regiones definidas. Si se produce un error en una región, no hay ninguna conmutación por error predefinida. Tener un punto de conexión en el que la agrupación regional está configurada en "All (World)" evitará que se quite tráfico y mejorará la disponibilidad del servicio. Advisor identifica los perfiles de Traffic Manager configurados para enrutamiento geográfico, en los que no hay ningún punto de conexión que tenga la agrupación regional configurada como "All (World)". Y recomienda cambiar la configuración para que el punto de conexión tenga esa configuración.

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-after-accidental-overwrite-or-deletion"></a>Uso de la eliminación temporal en la cuenta de Azure Storage para guardar y recuperar datos después de una sobrescritura o eliminación accidentales

Habilite la [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) en la cuenta de almacenamiento para que los blobs eliminados pasen a un estado de eliminación temporal en lugar de eliminarse de forma definitiva. Cuando se sobrescriben datos, se genera una instantánea de la eliminación temporal para guardar el estado de los datos sobrescritos. El uso de la eliminación temporal le permite efectuar una recuperación en caso de que se produzca una eliminación o sobrescritura accidentales. Advisor identifica las cuentas de Azure Storage que no tienen habilitada la eliminación temporal y sugiere habilitarla.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>Configuración de la puerta de enlace VPN como activo-activo para lograr la resistencia de conexión

En la configuración activo-activo, ambas instancias de una puerta de enlace VPN establecerán túneles VPN S2S al dispositivo VPN local. Cuando se produce un evento de mantenimiento planeado o no planeado para una instancia de la puerta de enlace, el tráfico cambiará automáticamente al otro el túnel IPsec activo. Azure Advisor identificará las puertas de enlace VPN que no están configuradas como activo-activo y sugerirá que las configure para alta disponibilidad.

## <a name="use-production-vpn-gateways-to-run-your-production-workloads"></a>Uso de puertas de enlace VPN de producción para ejecutar las cargas de trabajo de producción

Azure Advisor buscará las puertas de enlace VPN que sean una SKU básica y le recomendará que use una SKU de producción en su lugar. La SKU básica se ha diseñado para usarse con fines de desarrollo y pruebas. Las SKU de producción ofrecen un mayor número de túneles, compatibilidad con BGP, opciones de configuración activo-activo, directiva Ipsec/IKE personalizada y una mayor estabilidad y disponibilidad.

## <a name="repair-invalid-log-alert-rules"></a>Reparación de las reglas de alertas de registro no válidas

Azure Advisor detectará las reglas de alertas que tienen consultas no válidas especificadas en la sección de condiciones. Las reglas de alertas de registro se crean en Azure Monitor y se usan para ejecutar consultas de análisis en intervalos especificados. Los resultados de la consulta determinan si se debe desencadenar una alerta. Las consultas de análisis pueden convertirse en no válidas con el paso del tiempo debido a cambios en los comandos, las tablas o los recursos a los que se hace referencia. Advisor le recomendará que corrija la consulta en la regla de alertas para evitar que se deshabilite automáticamente y garantizar la cobertura de la supervisión de los recursos de Azure. [Más información sobre cómo solucionar problemas de las reglas de alertas](https://aka.ms/aa_logalerts_queryrepair)

## <a name="configure-consistent-indexing-mode-on-your-cosmos-db-collection"></a>Configuración de un modo de indexación coherente en la colección de Cosmos DB

Los contenedores de Azure Cosmos DB configurados con el modo de indexación diferido pueden afectar a la actualización de los resultados de la consulta. Advisor detectará los contenedores configurados de esta forma y le recomendará que cambie al modo coherente. [Más información sobre las directivas de indexación de Cosmos DB](https://aka.ms/cosmosdb/how-to-manage-indexing-policy)

## <a name="configure-your-azure-cosmos-db-containers-with-a-partition-key"></a>Configuración de los contenedores de Azure Cosmos DB con una clave de partición

Azure Advisor identificará las colecciones sin particiones de Azure Cosmos DB que se estén aproximando a su cuota de almacenamiento aprovisionado. Le recomendará que migre estas colecciones a nuevas colecciones con una definición de clave de partición para que el servicio las pueda escalar horizontalmente de forma automática. [Más información sobre la elección de una clave de partición](https://aka.ms/cosmosdb/choose-partitionkey)

## <a name="upgrade-your-azure-cosmos-db-net-sdk-to-the-latest-version-from-nuget"></a>Actualización del SDK para NET de Azure Cosmos DB a la versión más reciente desde Nuget

Azure Advisor identificará las cuentas de Azure Cosmos DB que usan versiones anteriores del SDK de .NET y le recomendará que actualice a la versión más reciente de Nuget para obtener las correcciones más recientes, mejoras de rendimiento y nuevas funcionalidades de características. [Más información sobre el SDK de .NET de Cosmos DB](https://aka.ms/cosmosdb/sql-api-sdk-dotnet)

## <a name="upgrade-your-azure-cosmos-db-java-sdk-to-the-latest-version-from-maven"></a>Actualización del SDK para Java de Azure Cosmos DB a la versión más reciente desde Maven

Azure Advisor identificará las cuentas de Azure Cosmos DB que usan versiones anteriores del SDK de Java y le recomendará que actualice a la versión más reciente de Maven para obtener las correcciones más recientes, mejoras de rendimiento y nuevas funcionalidades de características. [Más información sobre el SDK de Java de Cosmos DB](https://aka.ms/cosmosdb/sql-api-sdk-dotnet)

## <a name="upgrade-your-azure-cosmos-db-spark-connector-to-the-latest-version-from-maven"></a>Actualización del conector de Spark de Azure Cosmos DB a la versión más reciente desde Maven

Azure Advisor identificará las cuentas de Azure Cosmos DB que usan versiones anteriores del conector de Spark de Cosmos DB y le recomendará que actualice a la versión más reciente de Maven para obtener las correcciones más recientes, mejoras de rendimiento y nuevas funcionalidades de características. [Más información sobre el conector de Spark de Cosmos DB](https://aka.ms/cosmosdb/spark-connector)

## <a name="enable-virtual-machine-replication"></a>Habilitar la replicación de máquinas virtuales
Las máquinas virtuales que no tienen habilitada la replicación en otra región no son resistentes a las interrupciones regionales. La replicación de las máquinas virtuales reduce cualquier impacto adverso en el negocio durante el transcurso de una interrupción regional de Azure. Advisor detectará las máquinas virtuales que no tienen la replicación habilitada y le recomendará que habilite la replicación para que, en caso de que se produzca una interrupción, pueda poner en funcionamiento rápidamente las máquinas virtuales en una región de Azure remota. [Más información sobre la replicación de máquinas virtuales](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Obtención de acceso a las recomendaciones sobre alta disponibilidad en Advisor

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

2.  En el panel de Advisor, haga clic en la pestaña **Alta disponibilidad**.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Azure Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
