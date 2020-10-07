---
title: Registro del proveedor de recursos de Azure VMware Solution
description: Pasos para registrar el proveedor de recursos de Azure VMware Solution.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 96d15546c5102a69a0b19f92de33d35d2e9ab6c2
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91575748"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

Para usar Azure VMware Solution, primero debe registrar el proveedor de recursos con la suscripción.

```azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

>[!TIP]
>También puede usar la GUI para registrar el proveedor de recursos de **Microsoft.AVS**.  Para más información, consulte el artículo [Registros de tipos y proveedores de recursos](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider).  
