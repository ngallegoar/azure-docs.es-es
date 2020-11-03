---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/26/2020
ms.author: larryfr
ms.openlocfilehash: 1fde2947719079e411bc233bcce426feec8fa996
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631057"
---
> [!IMPORTANT]
> La instancia de Cosmos DB se crea en un grupo de recursos administrados por Microsoft en __la suscripción__ , junto con los recursos que necesita. Esto significa que se le cobrará por esta instancia de Cosmos DB. El grupo de recursos administrado se denomina con el formato `<AML Workspace Resource Group Name><GUID>`. Si el área de trabajo de Azure Machine Learning usa un punto de conexión privado, también se crea una red virtual para la instancia de Cosmos DB. Esta red virtual se usa para proteger la comunicación entre Cosmos DB y Azure Machine Learning.
> 
> * __No elimine el grupo de recursos__ que contiene esta instancia de Cosmos DB, ni ninguno de los recursos que se crean de forma automática en este grupo. Si necesita eliminar el grupo de recursos, la instancia de Cosmos DB, etc., primero debe eliminar el área de trabajo de Azure Machine Learning que la usa. El grupo de recursos, la instancia de Cosmos DB y los otros recursos que se crean automáticamente se eliminan cuando se elimina el área de trabajo asociada.
> * Las [__Unidades de solicitud__](../articles/cosmos-db/request-units.md) predeterminadas para esta cuenta de Cosmos DB son __8000__. __Este valor no se puede cambiar__.
> * No __puede proporcionar una red virtual propia para usarla con la instancia de Cosmos DB__ que se crea. Tampoco __puede modificar la red virtual__. Por ejemplo, no puede cambiar el rango de direcciones IP que usa.