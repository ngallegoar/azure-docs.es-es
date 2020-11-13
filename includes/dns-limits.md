---
author: rothja
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/05/2020
ms.author: rohink
ms.openlocfilehash: 7011b92485c56187021c9043ba84bc85e448a98f
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329564"
---
**Zonas DNS públicas**

| Resource | Límite |
| --- | --- |
| Zonas DNS públicas por suscripción |250 <sup>1</sup> |
| Conjuntos de registros por zona DNS pública |10 000 <sup>1</sup> |
| Registros por conjunto de registros en la zona DNS pública |20 |
| Número de registros de alias para un único recurso de Azure |20|

<sup>1</sup>Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.

**Zonas de DNS privado**

| Recurso | Límite |
| --- | --- |
| Zonas DNS privadas por suscripción |1000|
| Conjuntos de registros por zona DNS privada |25000|
| Registros por conjunto de registros para zonas DNS privadas |20|
| Vínculos de red virtual por zona DNS privada |1000|
| Vínculos de red virtual por zonas DNS privadas con el registro automático habilitado |100|
| Número de zonas DNS privadas a la que se puede vincular una red virtual con el registro automático habilitado |1|
| Número de zonas DNS privadas a la que se puede vincular una red virtual |1000|
| Número de consultas de DNS que una máquina virtual puede enviar al solucionador de Azure DNS por segundo |1000 <sup>1</sup> |
| Número máximo de consultas de DNS en cola (pendientes de respuesta) por máquina virtual |200 <sup>1</sup> |

<sup>1</sup>Estos límites se aplican a cada máquina virtual individual, no al nivel de red virtual. Las consultas de DNS que superan estos límites se omiten.
