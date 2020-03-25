---
title: archivo de inclusión
description: archivo de inclusión
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/19/2019
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: e32b55ed655b1e47f85640eb7f494a89f3274667
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "75456748"
---
Proporcionamos diferentes plantillas para abordar escenarios de incorporación específicos. Elija la opción que mejor funcione y asegúrese de modificar el archivo de parámetros para que refleje su entorno. Para más información sobre cómo usar estos archivos en su implementación, consulte [Incorporación de un cliente a la administración de recursos delegados de Azure](../articles/lighthouse/how-to/onboard-customer.md).

| **Plantilla** | **Descripción** |
|---------|---------|
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management) | Incorpora una suscripción de cliente a la administración de recursos delegados de Azure. Se debe realizar una implementación independiente para cada suscripción. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) | Incorpora uno o varios de los grupos de recursos de un cliente a la administración de recursos delegados de Azure. Use **rgDelegatedResourceManagement** para un solo grupo de recursos o **multipleRgDelegatedResourceManagement** para incorporar varios grupos de recursos en la misma suscripción. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management) | Si ha [publicado una oferta de servicios administrados en Azure Marketplace](../articles/lighthouse/how-to/publish-managed-services-offers.md) también puede usar esta plantilla para incorporar recursos para los clientes que han aceptado la oferta. Los valores de Marketplace en el archivo de parámetros deben coincidir con los valores que usó al publicar la oferta. |

Normalmente, se requiere una implementación independiente para cada suscripción que se incorpora, pero también puede implementar plantillas en varias suscripciones.

| **Plantilla** | **Descripción** |
|---------|---------|
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-subscription-deployment) | Implementa plantillas de Azure Resource Manager en varias suscripciones. |
