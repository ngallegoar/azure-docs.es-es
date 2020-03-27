---
title: Exportación de la información de nivel superior de la suscripción de Azure
description: Describe cómo puede ver todos los identificadores de la suscripción de Azure asociados con su cuenta.
author: bandersmsft
ms.reviewer: matrive
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: 87135b309f0406528bcada1cd906dd2f8535d1ae
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202903"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exportación y visualización de la información de nivel superior de la suscripción
Si necesita ver el conjunto de identificadores de suscripción asociados con las credenciales de usuario, [descargue un archivo .json con información de la suscripción desde el Centro de cuentas de Azure](https://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

El archivo .json descargado proporciona la siguiente información:
- Correo electrónico: dirección de correo electrónico asociada a la cuenta.
- Puid: identificador único asociado a la cuenta de facturación.
- SubscriptionIds: lista de suscripciones que pertenecen a la cuenta, enumeradas por identificador de suscripción.

### <a name="subscriptionsjson-sample"></a>Ejemplo de subscriptions.json

```json
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
```
