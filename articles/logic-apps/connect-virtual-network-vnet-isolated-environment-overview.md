---
title: Acceso a redes virtuales de Azure
description: Introducción sobre la forma en que los entornos de servicio de integración (ISE) facilitan el acceso de las aplicaciones lógicas a las redes virtuales (VNET) de Azure.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: 9d5e0c088fe773f16e1fc57f292ca812906aa09c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127252"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acceso a recursos de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)

A veces, sus aplicaciones lógicas necesitan tener acceso a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una red de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Para configurar este acceso, puede [crear un *entorno del servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md). Un ISE es una instancia aislada del servicio Logic Apps que usa recursos dedicados y se ejecuta de forma independiente del servicio Logic Apps público, "global" y multiinquilino.

La ejecución de aplicaciones lógicas en una instancia aislada e individual ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación, también conocido como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). Un ISE también proporciona estas ventajas:

* Direcciones IP estáticas propias, que son independientes de las direcciones IP estáticas que comparten las aplicaciones lógicas en el servicio multiinquilino. Puede configurar una dirección IP de salida única, pública, estática y predecible para comunicarse con los sistemas de destino. De ese modo, no es necesario configurar aperturas adicionales del firewall en los sistemas de destino para cada ISE.

* Se aumentan los límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes de conector personalizado. Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](logic-apps-limits-and-config.md).

> [!NOTE]
> Algunas redes virtuales de Azure usan puntos de conexión privados ([Azure Private Link](../private-link/private-link-overview.md)) para proporcionar acceso a los servicios de PaaS de Azure, como Azure Storage, Azure Cosmos DB o Azure SQL Database, servicios de asociados o servicios de clientes que se hospedan en Azure. Si las aplicaciones lógicas necesitan acceder a redes virtuales que usan puntos de conexión privados, debe crear, implementar y ejecutar esas aplicaciones lógicas dentro de un ISE.

Cuando se crea un ISE, Azure *inserta* o implementa dicho ISE en su red virtual de Azure. A continuación, puede usar ese ISE como la ubicación de las aplicaciones lógicas y cuentas de integración que necesitan acceso.

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Las aplicaciones lógicas pueden acceder a recursos que están dentro de la red virtual o conectados a esta mediante estos elementos, que se ejecutan dentro del mismo ISE que la aplicación lógica:

* Una acción o desencadenador integrado con la etiqueta **CORE**, como la acción o el desencadenador HTTP.
* Un conector con la etiqueta **ISE** para ese sistema o servicio.
* Un conector personalizado.

También puede usar conectores que no tienen la etiqueta **CORE** o **ISE** con las aplicaciones lógicas del ISE. En su lugar, estos conectores se ejecutan en el mismo servicio de Logic Apps multiinquilino. Para obtener más información, consulte estas secciones:

* [Diferencias entre aislamiento y servicio multiinquilino ](#difference)
* [Conexión desde un entorno del servicio de integración](../connectors/apis-list.md#integration-service-environment)
* [Conectores de ISE](../connectors/apis-list.md#ise-connectors)

> [!IMPORTANT]
> Las aplicaciones lógicas, los desencadenadores integrados, las acciones integradas y los conectores que se ejecutan en el ISE usan un plan de tarifa diferente al plan de tarifa basado en el consumo. Para obtener más información, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Para obtener información sobre precios, consulte los [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

En esta introducción se describe más información sobre cómo un ISE proporciona a las aplicaciones lógicas acceso directo a la red de Azure y se comparan las diferencias entre un ISE y el servicio Logic Apps multiinquilino.

<a name="difference"></a>

## <a name="isolated-versus-multi-tenant"></a>Diferencias entre aislamiento y servicio multiinquilino

Cuando se crean y ejecutan aplicaciones lógicas en un ISE, se proporcionan las mismas experiencias de usuario y funcionalidades similares que el servicio multiinquilino de Logic Apps. Puede usar los mismos desencadenadores, acciones y conectores administrados integrados que están disponibles en el servicio multiinquilino de Logic Apps. Algunos conectores administrados ofrecen versiones de ISE adicionales. La diferencia entre los conectores de ISE y los que no son de ISE radica en la ubicación en la que se ejecutan y las etiquetas que tienen en el diseñador de aplicación lógica cuando se trabaja en un ISE.

![Conectores con y sin etiquetas en un ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/labeled-trigger-actions-integration-service-environment.png)

* Las acciones y desencadenadores integrados muestran la etiqueta **CORE**. Siempre se ejecutan en el mismo ISE que la aplicación lógica. Los conectores administrados en los que se muestra la etiqueta **ISE** también se ejecutan en el mismo ISE que la aplicación lógica.

  Por ejemplo, estos son algunos conectores que ofrecen versiones de ISE:

  * Azure Blob Storage, File Storage y Table Storage
  * Azure Queues, Azure Service Bus, Azure Event Hubs e IBM MQ
  * FTP y SFTP-SSH
  * SQL Server, Azure SQL Data Warehouse, Azure Cosmos DB
  * AS2, X12 y EDIFACT

* Los conectores administrados que no muestran etiquetas adicionales siempre se ejecutan en el servicio multiinquilino de Logic Apps, pero puede seguir usando estos conectores en una aplicación lógica hospedada en ISE.

<a name="on-premises"></a>

### <a name="access-to-on-premises-systems"></a>Acceso a los sistemas locales

Para acceder a los sistemas u orígenes de datos locales que están conectados a una red virtual de Azure, las aplicaciones en un ISE pueden usar los siguientes elementos:

* Acción HTTP

* Un conector con la etiqueta ISE para ese sistema

  > [!NOTE]
  > Para usar autenticación de Windows con el conector de SQL Server en un [entorno de servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), use la versión no ISE del conector con la [puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md). La versión con etiqueta ISE no admite autenticación de Windows.

* Conector personalizado

  * Si tiene conectores personalizados que requieren la puerta de enlace de datos local y los ha creado fuera de un ISE, las aplicaciones lógicas en un ISE también pueden usar esos conectores.

  * Los conectores personalizados creados en un ISE no funcionan con la puerta de enlace de datos local. Aun así, estos conectores pueden acceder directamente a orígenes de datos locales que están conectados a la red virtual en la que se hospeda el ISE. Por tanto, es muy probable que las aplicaciones lógicas en un ISE no necesiten la puerta de enlace de datos cuando se comuniquen con esos recursos.

En el caso de sistemas locales que no están conectados a una red virtual o que no tienen conectores con etiqueta ISE, primero debe [configurar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md) para que las aplicaciones lógicas puedan conectarse a esos sistemas.

<a name="ise-level"></a>

## <a name="ise-skus"></a>SKU de ISE

Al crear el ISE, puede seleccionar la SKU de desarrollador o SKU Premium. Las diferencias entre estas SKU son las siguientes:

* **Developer**

  Proporciona un ISE de menor costo que puede usar para experimentación, desarrollo y pruebas, pero no para producción ni para pruebas de rendimiento. La SKU de desarrollador incluye desencadenadores y acciones integrados, conectores estándar, conectores empresariales y una sola cuenta de integración de [nivel Gratis](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) por un precio mensual fijo. Sin embargo, esta SKU no incluye ningún contrato de nivel de servicio (SLA), opciones para escalar verticalmente la capacidad o redundancia durante el reciclaje, lo que significa que puede experimentar retrasos o tiempos de inactividad.

* **Premium**

  Proporciona un ISE que puede usar para producción y que incluye soporte de SLA, desencadenadores y acciones integrados, conectores estándar, conectores empresariales, una sola cuenta de integración de [nivel Estándar](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits), opciones para el escalado vertical de la capacidad y redundancia durante el reciclaje por un precio fijo al mes.

> [!IMPORTANT]
> La opción del SKU solo está disponible durante la creación del ISE y no se puede cambiar más adelante.

Para ver las tarifas de precios, consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Para saber cómo funcionan los precios y la facturación para los ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing).

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>Acceso al punto de conexión del ISE

Al crear el ISE, puede optar por usar puntos de conexión de acceso internos o externos. La selección determina si los desencadenadores de solicitudes o de webhooks de las aplicaciones lógicas del ISE pueden recibir llamadas desde fuera de la red virtual o no.

Estos puntos de conexión también afectan a la manera en que puede acceder a las entradas y salidas en el historial de ejecución de las aplicaciones lógicas.

* **Internas**: puntos de conexión privados que permiten llamadas a aplicaciones lógicas del ISE donde puede ver las entradas y salidas de las aplicaciones lógicas del historial de ejecuciones, así como acceder a dichas entradas y salidas, *solo desde dentro de la red virtual*.

* **Externas**: puntos de conexión públicos que permiten llamadas a aplicaciones lógicas del ISE donde puede ver las entradas y salidas de las aplicaciones lógicas del historial de ejecuciones, así como acceder a dichas entradas y salidas, *desde fuera de la red virtual*. Si usa grupos de seguridad de red (NSG), asegúrese de que están configurados con reglas de entrada para permitir el acceso a las entradas y salidas del historial de ejecución. Para más información, consulte [Habilitar el acceso para el ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access).

> [!IMPORTANT]
> La opción del punto de conexión de acceso solo está disponible durante la creación del ISE y no se puede cambiar más adelante.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Cuentas de integración con ISE

Puede usar cuentas de integración con aplicaciones lógicas dentro de un entorno de servicio de integración (ISE). Sin embargo, esas cuentas de integración deben usar el *mismo ISE* que las aplicaciones lógicas vinculadas. Las aplicaciones lógicas de una instancia de ISE solo pueden hacer referencia a aquellas cuentas de integración que se encuentran en la misma instancia de ISE. Cuando se crea una cuenta de integración, puede seleccionar la instancia de ISE como la ubicación de la cuenta de integración. Para saber cómo funcionan los precios y la facturación de las cuentas de integración con un ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Para ver las tarifas de precios, consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Pasos siguientes

* [Conexión a redes virtuales de Azure desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
