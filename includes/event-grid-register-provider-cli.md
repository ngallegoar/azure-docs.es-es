---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: ace22a6896a121f5cd8af838c7b0e427bd0287dc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "67185997"
---
## <a name="enable-event-grid-resource-provider"></a>Habilitación del proveedor de recursos de Event Grid

Si aún no ha usado anteriormente Event Grid en su suscripción de Azure, puede que tenga que registrar el proveedor de recursos de Event Grid. Ejecute el siguiente comando para registrar el proveedor:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Puede tardar unos instantes en finalizarse el registro. Para comprobar el estado, ejecute:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

Cuando `registrationState` sea `Registered`, estará preparado para continuar.