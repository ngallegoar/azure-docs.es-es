---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 2/14/2020
ms.author: rohink
ms.openlocfilehash: 3fbbb9d491eef202309c9e64f9a5a839a5d4516d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334677"
---
**Zonas DNS públicas**

| Resource | Límite |
| --- | --- |
| Zonas DNS públicas por suscripción |250 <sup>1</sup> |
| Conjuntos de registros por zona DNS pública |10 000 <sup>1</sup> |
| Registros por conjunto de registros en la zona DNS pública |20 |
| Número de registros de alias para un único recurso de Azure |20|
| Zonas DNS privadas por suscripción |1000|
| Conjuntos de registros por zona DNS privada |25000|
| Registros por conjunto de registros para zonas DNS privadas |20|
| Vínculos de red virtual por zona DNS privada |1000|
| Vínculos de red virtual por zonas DNS privadas con el registro automático habilitado |100|
| Número de zonas DNS privadas a la que se puede vincular una red virtual con el registro automático habilitado |1|
| Número de zonas DNS privadas a la que se puede vincular una red virtual |1000|
| Número de consultas de DNS que una máquina virtual puede enviar al solucionador de Azure DNS por segundo |500 <sup>2</sup> |
| Número máximo de consultas de DNS en cola (pendientes de respuesta) por máquina virtual |200 <sup>2</sup> |

<sup>1</sup>Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.

<sup>2</sup>Estos límites se aplican a cada máquina virtual individual y no al nivel de red virtual. Las consultas de DNS que superan estos límites se omiten.