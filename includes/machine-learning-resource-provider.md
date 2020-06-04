---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 8fd774f8a3a73ceaffa7902b35e1b1dff12ef5af
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "75893965"
---
Al crear un área de trabajo de Azure Machine Learning o un recurso usado por el área de trabajo, puede recibir un mensaje de error similar a los siguientes:

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

Muchos proveedores de recursos se registran automáticamente, aunque no todos. Si recibe este mensaje, debe registrar el proveedor mencionado.

Para obtener más información sobre cómo registrar un proveedor de recursos, consulte [Registro del proveedor de recursos](../articles/azure-resource-manager/templates/error-register-resource-provider.md).