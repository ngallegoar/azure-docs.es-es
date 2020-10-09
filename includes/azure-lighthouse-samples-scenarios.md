---
title: archivo de inclusión
description: archivo de inclusión
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 04/24/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 4a6d9ef04a33c84d68dff1429fb39a193a249280
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82204633"
---
En estos ejemplos se muestran varias tareas que se pueden realizar en escenarios de administración entre inquilinos.

| **Plantilla** | **Descripción** |
|---------|---------|
| [create-keyvault-secret](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/create-keyvault-secret) | Crea una instancia de Key Vault en el inquilino del cliente y crea directivas de acceso.
| [cross-rg-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/cross-rg-deployment) | Implementa cuentas de almacenamiento en dos grupos de recursos diferentes.|
| [deploy-azure-mgmt-services](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/deploy-azure-mgmt-services) | Crea los servicios de administración de Azure, los vincula entre sí e implementa soluciones adicionales. Para una implementación completa, use la plantilla **rgWithAzureMgmt.json**. |
| [deploy-azure-security-center](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/deploy-azure-security-center) | Habilita y configura Azure Security Center dentro de la suscripción de Azure de destino. |
| [deploy-azure-sentinel](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/deploy-azure-sentinel) | Implementa y habilita Azure Sentinel en un área de trabajo de Log Analytics existente de una suscripción delegada. |
| [deploy-log-analytics-vm-extensions](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/deploy-log-analytics-vm-extensions) | Estas plantillas le permiten implementar extensiones de máquinas virtuales de Log Analytics en máquinas virtuales Windows y Linux mediante la conexión al área de trabajo de Log Analytics designada. |
