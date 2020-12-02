---
title: Archivo de inclusión
description: archivo de inclusión
author: rothja
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/18/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 74f9c5304237ec77a629bc914d95c345882b784f
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95559158"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| vCPU por [suscripción](https://azure.microsoft.com/pricing/)<sup>1</sup> |20 |10 000 |
| [Coadministradores](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) por suscripción |200 |200 |
| [Cuentas de almacenamiento](../articles/storage/common/storage-account-create.md) por suscripción<sup>2</sup> |100 |100 |
| [Servicios en la nube](../articles/cloud-services/cloud-services-choose-me.md) por suscripción |20 |200 |
| [Redes locales](/previous-versions/azure/reference/jj157100(v=azure.100)) por suscripción |10 |500 |
| Servidores DNS por suscripción |9 |100 |
| Direcciones IP reservadas por suscripción |20 |100 |
| [Grupos de afinidad](/previous-versions/azure/virtual-network/virtual-networks-migrate-to-regional-vnet) por suscripción |256 |256 |
| Longitud del nombre de la suscripción (caracteres) | 64 | 64 |

<sup>1</sup>Recuento de instancias extrapequeñas como una vCPU hacia el límite de vCPU a pesar de usar un núcleo de CPU parcial.

<sup>2</sup>El límite de la cuenta de almacenamiento incluye las cuentas de almacenamiento Estándar y Premium.