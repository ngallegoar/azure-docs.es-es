---
title: Notas de la versión de Seller Insights
description: Proporciona información sobre los cambios realizados en la característica Seller Insights.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 3/3/2019
ms.author: dsindona
ms.openlocfilehash: 1b88a8353c1cb9f97f18d720171f0827d7d85254
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285408"
---
# <a name="seller-insights-release-notes"></a>Notas de la versión de Seller Insights 

(Fecha de lanzamiento: 1 de marzo de 2019)

En este artículo se proporciona información sobre los cambios realizados en la característica Seller Insights de [Cloud Partner Portal](https://cloudpartner.azure.com/#insights).

## <a name="release-highlights-for-march-1-2019"></a>Cambios destacados para el 1 de marzo de 2019

* *Customer Trend* (Tendencia de clientes) se agregó al Resumen.
* *Principales cinco clientes* en el Resumen reflejan todas las suscripciones a Azure que tiene un cliente.
* *Normalized Usage Trend & Active Orders Trend* (Tendencia de uso normalizada y tendencia de pedidos activos) en el Resumen se movió *Pedidos mensuales de un vistazo*.
* *Payout Reconciliation Report* (Informe de conciliación de pagos) se actualizó.
* *Principales cinco clientes* en el Pago reflejan todas las suscripciones a Azure que tiene un cliente.
* *Informe de uso* se actualizó con el Id. de cliente.
* *Customer Tenure* (Antigüedad del cliente) en Orders & Usage (Pedidos y uso) refleja todas las suscripciones de Azure que tiene un cliente.


(Fecha de lanzamiento: 28 de julio de 2018)

## <a name="release-highlights-for-july-28-2018"></a>Cambios destacados para el 28 de julio de 2018


-   *Precios estimados*: proporciona una vista de los gastos de cliente con implicaciones de conversión de moneda.
-   *Pagos previstos*: proporciona una vista anterior de posibles pagos.
-  *Identificadores de referencia de uso*: proporcionan una gran fidelidad de los datos entre el uso del cliente y los pedidos con pagos.
-   *Uso a intervalo diario*: proporciona mayor granularidad y una mejor información sobre el uso por parte del cliente.


### <a name="changes-to-data-structure-and-taxonomy"></a>Cambios en la estructura de datos y la taxonomía

En la tabla siguiente se enumeran las métricas que se han agregado o que han cambiado sustancialmente en esta versión. 

| **Nuevo término**                   |    **Definición**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Price (CC) [Precio (CC)]                     | Precio de una unidad de uso para una SKU determinada (en la moneda del cliente).       |
| Estimated Extended Charge (CC) [Cargo total estimado (CC)] | Cargo total estimado de la cantidad de unidades de uso de una SKU determinada (en la moneda del cliente). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Publisher Currency (PC) [Moneda del anunciante (PC)]        | Moneda preferida del anunciante para el pago.                               |
| Estimated Price (PC) [Precio estimado (PC)]           | Precio estimado de una unidad de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Estimated Extended Charge (PC) [Cargo total estimado (PC)] | Cargo total estimado de la cantidad de unidades de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo. |
| Estimated Payout (PC) [Pago estimado (PC)]          | Pago estimado de la cantidad de unidades de uso de una SKU determinada en función de la conversión de moneda el día en que se calcula el uso (en la moneda del anunciante). Este valor puede no ser exacto debido a errores de truncamiento o redondeo.   |
| Usage Reference (Referencia de uso)                | El identificador de uno o varios días de uso del cliente para una SKU determinada asociada con una entrada del informe de pagos. |
|  |  |
