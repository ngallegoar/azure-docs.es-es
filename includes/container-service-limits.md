---
title: archivo de inclusión
description: archivo de inclusión
services: container-service
author: mlearned
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: mlearned
ms.custom: include file
ms.openlocfilehash: a42bba1b6524825aa571e4c18319b61b97829792
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96584517"
---
| Resource | Límite |
| --- | :--- |
| Número máximo de clústeres por suscripción | 1000 |
| Número máximo de nodos por clúster con los conjuntos de disponibilidad de máquina virtual y el SKU básico de Load Balancer  | 100 |
| Número máximo de nodos por clúster con Virtual Machine Scale Sets y el [SKU estándar de Load Balancer][standard-load-balancer] | 1000 (100 nodos por [grupo de nodos][node-pool]) |
| Número máximo de pods por nodo: [Redes básicas][basic-networking] con Kubenet | 110 |
| Número máximo de pods por nodo: [Conexiones de red avanzadas][advanced-networking] con la interfaz de red de contenedor de Azure | Implementación de la CLI de Azure: 30<sup>1</sup><br />Plantilla de Azure Resource Manager: 30<sup>1</sup><br />Implementación del portal: 30 |

<sup>1</sup>Cuando implementa un clúster de Azure Kubernetes Service (AKS) con la CLI de Azure o una plantilla de Resource Manager, este valor es configurable en hasta 250 pods por nodo. No puede configurar un número máximo de pods por nodo después de haber implementado un clúster AKS, o si implementa un clúster mediante Azure Portal.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest