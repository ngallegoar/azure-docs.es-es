---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/26/2020
ms.author: larryfr
ms.openlocfilehash: 2bba53410834aadce5627a34a759e65aa0e11c28
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94574485"
---
> [!IMPORTANT]
> La instancia de Cosmos DB se crea en un grupo de recursos administrados por Microsoft en __la suscripción__, junto con los recursos que necesita. Esto significa que se le cobrará por esta instancia de Cosmos DB. El grupo de recursos administrado se denomina con el formato `<AML Workspace Resource Group Name><GUID>`. Si el área de trabajo de Azure Machine Learning usa un punto de conexión privado, también se crea una red virtual para la instancia de Cosmos DB. Esta red virtual se usa para proteger la comunicación entre Cosmos DB y Azure Machine Learning.
> 
> * __No elimine el grupo de recursos__ que contiene esta instancia de Cosmos DB, ni ninguno de los recursos que se crean de forma automática en este grupo. Si necesita eliminar el grupo de recursos, la instancia de Cosmos DB, etc., primero debe eliminar el área de trabajo de Azure Machine Learning que la usa. El grupo de recursos, la instancia de Cosmos DB y los otros recursos que se crean automáticamente se eliminan cuando se elimina el área de trabajo asociada.
> * Las [__Unidades de solicitud__](../articles/cosmos-db/request-units.md) predeterminadas para esta cuenta de Cosmos DB son __8000__.
> * No __puede proporcionar una red virtual propia para usarla con la instancia de Cosmos DB__ que se crea. Tampoco __puede modificar la red virtual__. Por ejemplo, no puede cambiar el rango de direcciones IP que usa.