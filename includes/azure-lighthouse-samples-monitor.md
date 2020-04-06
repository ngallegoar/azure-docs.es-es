---
title: archivo de inclusión
description: archivo de inclusión
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 03/30/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: aacf1c287dca01b5993d35b311eed22e88ef7e87
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421161"
---
En estos ejemplos se muestra cómo usar Azure Monitor para crear alertas para las suscripciones que se han incorporado a la administración de recursos delegados de Azure.

| **Plantilla** | **Descripción** |
|---------|---------|
| [monitor-delegation-changes](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/tools/monitor-delegation-changes) | Consulta el último día de actividad en un inquilino de administración y [notifica sobre cualquier delegación agregada o eliminada](../articles/lighthouse/how-to/monitor-delegation-changes.md) (o intentos de delegaciones que no se realizaron correctamente).|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/alert-using-actiongroup) | Esta plantilla crea una alerta de Azure y se conecta a un grupo de acciones existente.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/multiple-loganalytics-alerts) | Crea varias alertas de registro basadas en consultas de Kusto.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegation-alert-for-customer) | Implementa una alerta en un inquilino cuando un usuario delega una suscripción en un inquilino de administración.|
